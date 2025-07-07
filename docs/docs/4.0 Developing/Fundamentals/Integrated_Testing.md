# Integrated Testing - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Integrated_Testing
# Integrated Testing

## Contents

  * 1 Requirements
    * 1.1 Autotest Script
    * 1.2 Model Validation
      * 1.2.1 How to use the _assert_ module
    * 1.3 Future Tests
      * 1.3.1 Residential
    * 1.4 See also
The objective of a GridLab-D validation "module" is to develop a system with which model calculation errors and discrepancies can be detected. This module can be included as part of an auto-build sequence, contextually for BuildBot. Included with this module will be a set of models with which the existing GridLab-D objects are tested for accuracy and consistency. 

# 

Requirements

There must be a tool that can be run by BuildBot or some other auto-compile/build script to run a list of GridLab-D models. This tool must be able to detect the presence of simulation failures (specifics optional). The models themselves must include tests for the various objects and states in the Climate, Powerflow, and Residential modules. 

The tests themselves need not be complicated. The goal is to verify that the calculations and the state changes are consistent either with previous internal results, or with external reference models (such as the IEEE 13 node test feeder). 

## Autotest Script

There currently exists a script file in the SVN tree, named "validate.py", that is used by BuildBot and some developers to rapidly execute a series of test files within the source tree. 

To run the script, open a console in the trunk directory of the source tree and run "python validate_dev.py [start_dir]". By default it will start in the working directory that the script was executed from. 

If the script is run with the "--help" flag, it will print a short message on how it works and how to use it, then exit. 

If the script is run with the "--error" flag, it will toggle the output of the errors to the autotest output stream. 

The script operates in a fairly simple fashion: it walks through the directory tree seeking directories named "autotest" on its first pass, locates any file of the format "test_***.glm", creates a directory with the name "test_***" next to each test file, copies the test file into the test directory, and executes GridLAB-D with the test file. The stderr and stdout for the executable are directed into the test directory, and GridLAB-D should either return a nonzero value or create a gridlabd.xml dump file in that directory if the simulation fails. For each file that is tested, a message will be printed if the test does not behave as expected, and will optionally copy all error messages from the run onto stdout. 

If a test file includes the substring "err_" or "_err" in its name, it is assumed that the file is designed to fail, and will only count against the number of failed tests if the test passes. This is meant to deliberately test the bounds of boundary conditions and to locate both sides of the condition. 

If a test file includes the substring "exc_" or "_exc" in its name, the validate script will expect it to fail the load process, and consider the test to fail if the file successfully loads. This is useful for checking syntax and inline code generation standards. 

If a test file includes the substring "opt_" or "_opt" in its name, it will be considered an optional test that will not count against the number of failed tests. These optional tests are intended for phasing in new features and proprietary modules that may not be public domain. This will work in conjunction with "err_" and "exc_" tests. 

The autotest script does not currently play well with players or shapers, and does not validate output recorder or collector files. 

Work is being done to trace through the input models to locate any input player or shaper files and copy those into the test directory. 

Future revisions of the autotest script will provide better cleanup routines, will suffer fewer false positives for failure conditions, and email any output or dump files from failed test files to the users listed on the blamelist. 

## Model Validation

While the autotest script creates a means for automatically validating test files, the test files themselves must be created to conform to "known" answers. There is a range of tests that may be performed. Anywhere from a simple input test to verify that error catches are performing properly to a large-scale validation of an entire distribution feeder, where each voltage, current, and power is confirmed. A module called _assert_ was added to aid in the development of these tests and, hopefully, includes the ability to create any scenario that may be envisioned for testing. _Assert_ allows the user to assert that an answer is true during a simulation, by comparing a provided value with the value that is actually calculated during the simulation. However, there are a number of different tests that may be performed by the assert, depending on the application. This document will attempt to explain each case. 

### How to use the _assert_ module

Assert objects work by taking a user defined value and comparing it against a property that is also specified by the user. There are three different assert objects that may be used, _complex_assert_ , _double_assert_ , and _enum_assert_. These are used to test against complex numbers (r + j*x), double values (real numbers), and enumerations (strings), respectively. Each object also contains a number of operating modes that will further explained below. 

To create an assert test, an assert object is created as a child of the object to be tested. Three inputs must be specified to create the assert: 

  * _value_ \- This is the "right" answer, or the value to be checked against, that is provide by the user. The type of value used depends on the type of assert used (complex, double, or enumeration). Units are not used in this value, so must be in the same units as the answer to be compared against.
  * _within_ \- This allows the user to specify a range of answer. **Not used in _enum_assert_ \- _value_ must be the exact string.**
  * _target_ \- This tells the assert object what _value_ is being tested against. Quotes must encompass the name of the property. Any public variable may be used. The default units of the target property will be used when comparing against _value_.
Here's an example: 
    
    
    object node {
      name test_node_1;
      phases ABCN;
      nominal_voltage 7200;
      voltage_A +7199.558+0.000j;
      voltage_B -3599.779-6235.000j;
      voltage_C -3599.779+6235.000j; 
      object double_assert {
         value 7199;
         within 2;
         target "nominal_voltage";
      };
    }
    

This assert test says that _nominal_voltage_ of _test_node_1_ must equal 7199 +/- 2 (or be within the range of 7197-7201). Since _nominal_voltage_ is 7200, this test would pass. When an assert passes, it will appear nothing happens (unless verbose is set to true). However, when an assert fails, the system will throw an error message and end the simulation. This error throw is what allows the autotest script to know that the test failed and collect the needed information. To actually see what the error was and how far apart the values were, verbose needs to be set to true (#set verbose=1). 

Additionally, other operating modes may be used: 

  * _status_ \- This allows the user to set whether the assert should test for a true or false value. 
    * ASSERT_TRUE (default) - This test will pass if the _value_ given is within the specified range of the _target_ property.
    * ASSERT_FALSE - This test will pass if the _value_ given is outside of the specified range of the _target_ property.
    * ASSERT_NONE - This will disengage the assert test and pass every value given. This is useful when fed in via a tape player (e.g. ASSERT_NONE may be played in via a tape player, except at given times where it is switched to ASSERT_TRUE. The test will then only pass/fail when ASSERT_TRUE is used.)
  * _once_ (not used with _enum_assert_) - This allows the user to specify that a test be run only once during a simulation. Used in conjunction with _in_. 
    * ONCE_FALSE (default) - In this mode, the assert value will be tested continuously at all times during the simulation.
    * ONCE_TRUE - When used, this tells the assert to only test the given _value_ against the _target_ value at the time specified by _in_.
  * _in_ \- Specifies the time at which a test is to be performed. Uses standard GridLAB-D time formats with single quotes around it (e.g. '2001-01-02 00:00:00 CST').
Finally, _complex_assert_ has the ability to look at only the real value, the imaginary, the magnitude, or the angle: 

  * _operation_ \- Allows the user to look at various aspects of the complex number. 
    * FULL (default) - This compares both the real and imaginary portion of the complex _value_. _within_ will be applied to both components ( real(_target_) must be _within_ xx of real(_value_) and same applies to the imaginary portion).
    * REAL - Only compares the real portion of _value_ to be _within_ xx of _target_. _value_ is still specified as a complex number.
    * IMAGINARY - Only compares the imaginary portion of _value_ to be _within_ xx of _target_.
    * MAGNITUDE - Compares the magnitude of _value_ to be _within_ xx of _target_.
    * ANGLE - Compares the angle of _value_ to be _within_ xx of _target_ (must be specified in radians).
For example: 
    
    
    object node {
      name test_node_1;
      phases ABCN;
      nominal_voltage 7200;
      voltage_A +7199.558+0.000j;
      voltage_B -3599.779-6235.000j;
      voltage_C -3599.779+6235.000j; 
      object complex_assert {
         value -2600-7000j;
         within 2;
         target "voltage_B";
         operation MAGNITUDE;
         status ASSERT_FALSE;
         once ONCE_TRUE;
         in '2001-01-02 00:00:00 CST';
      };
    }
    

This test will test the magnitude of -2600-7000j against -3599.779-6235.000j at only midnight on January 1st, 2001 (all other times the test will be ignored). Since ASSERT_FALSE is used, if the magnitudes are NOT _within_ 2 of each other the test will pass. In this case, the test would pass (7467 of _value_ versus 7200 of _voltage_B_). 

Sample autotests can be found in nearly all modules within GridLAB-D. They should be found under trunk/module/autotest/. One oddity to point out when using the assert object with autotests - when specifying the location of tape or player files, the player file should be placed in the same folder as the *.glm file, but must be directed to a sub-folder one higher than the current location. This is an oddity of the validation script and should only be used during the autotest process. Here is an example: 
    
    
    object node {
      name test_node_1;
      phases ABCN;
      nominal_voltage 7200;
      voltage_A +7199.558+0.000j;
      voltage_B -3599.779-6235.000j;
      voltage_C -3599.779+6235.000j; 
      object complex_assert {
         object player {
            file ../test_voltage.player;
            property value;
         }
         within 2;
         target "voltage_B";
      };
    }
    

This will point to the file "test_voltage.player" when used with the autotest script. The voltage found within that player will be compared at each time step to the target voltage (_voltage_B_). If at any time during the simulation these answers are different, the simulation will fail. 

## Future Tests

### Residential

The following residential objects and states need to be validated: 

  * Waterheater power use and house heat transfer.
  * Waterheater one-node state, including stationary heat loss, and heat recovery, both from depletion and temperature decay.
  * Waterheater two-node state, including recovery rates and depletion rates.
  * Waterheater stable states, demonstrating that they exist. (experimental validation).
  * House insulation heat loss, appliance heat gain, HVAC heat gain/loss, internal power use.
  * Refrigerator, internal temperature decay & recovery, power usage, house heat transfer.
  * Range, power usage, house heat transfer
  * Plugload, power usage, house heat transfer
  * Occupant load, house heat transfer
  * Microwave, power usage, house heat transfer
  * Lights, power usage and house heat transfer, for each type and both indoor & outdoor
  * Freezer, internal temperature decay & recovery, power usage, house heat transfer.
  * EVCharger, power usage, charge times
  * Dryer, power usage, house heat transfer, stalling with low voltage & timer resets
  * Dishwasher, power usage, house heat transfer
  * Clotheswasher, power usage, house heat transfer, stalling with low voltage & timer resets
Note that this is not an exhaustive list. 

## See also

  * [assert (module)]
    * [assert (object)]
    * [complex_assert (object)]
    * [double_assert (object)]
    * [enum_assert (object)]
  * Integrated Testing
