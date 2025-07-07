# Programming conventions - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Programming_conventions
# Programming conventions

## Contents

  * 1 System architecture
    * 1.1 Core structure
    * 1.2 Module structure
    * 1.3 Other libraries
  * 2 Names
    * 2.1 File names
    * 2.2 Module names
    * 2.3 Class names
    * 2.4 Variable names
    * 2.5 Function names
  * 3 Testing/Validation
    * 3.1 Developer tests
    * 3.2 Validation autotests
  * 4 Download installers
  * 5 See also
Programming conventions \- GridLAB-D project conventions for source code 

This page describes the conventions used by GridLAB-D programmers when writing source code. 

## System architecture

GridLAB-D is structured as a simulation core system that acts much like an operating system (e.g., it's own clock, I/O system, thread control) with loadable [modules] that provide all the functionality that users need to model. 

Most developers work with modules exclusively but sometime you will need to work with the core. The difference between the two is immediately apparent: the [module API] is deployed using C++ classes, whereas the core is almost entirely written in C. 

Module are runtime libraries and can be compiled and deployed separately from the core 

### Core structure

The core provide all the general functionality that is needed to load, link and run a multi-agent simulation of a complex systems. This includes the following general functions: 

  * Command line processing
  * Global variable management
  * Thread and job control
  * Linkages with other simulations and remote tools
  * Module loading
  * Class definitions
  * Object instantiation
  * Variable definition and control
  * Unit conversion
  * Searching and iteration
  * Internal clock control
  * Function and message routing
  * Locking and synchronization
  * Time series simulation control
### Module structure

[Modules] are intended to provide a mechanism for group [classes] that provide related functionality. For example, the [powerflow] module provides the classes needed to implement the underlying electro-mechanical behavior for a general three-phase unbalanced time-series simulation of a power system. The interchange of data between classes is usually very limited and always optional, i.e., no class should so depend on another that it cannot be loaded and used without it. Currently available modules include are listed in the [modules] page. 

### Other libraries

Numerous libraries are used by GridLAB-D and must be installed on the developer's system before any development can be done. Which you will need to install will depend on your development platform. In addition, additional optional libraries are often needed to provide enhanced features. See [needed libraries] for details. 

There is a strictly policy that all third-party components in GridLAB-D not have GPL-like contamination clauses in their licenses. All libraries or source code with such clauses must be deployed as add-options that must be separately installed by the end-user. This policy ensures that those using GridLAB-D can be assured that any product or tool they develop using the open-source version of GridLAB-D will be unencumbered by the GPL-like license. 

## Names

GridLAB-D does not have too many restrictions on naming conventions for developers like there are definite restrictions for users. These are more guidelines than hard and fast rules. 

### File names

  * Only "a-z0-9_." are permitted in file names. The first character of a file name must "a-z".
  * File names are lowercase simple description names, e.g., "file" is ok, but "File", "FILE", and "my_long_file_name_handler" are not.
  * All files should have the appropriate 3 character extension as the only thing following the ".". No other dots may be used in the name. Some file names do not have any extension as a matter of convention. This is ok.
  * Numbers may be used in the rare cases where it is necessary, but otherwise should be avoided.
  * Abbreviations are generally not viewed favorably unless they are widely known to a general audience, e.g., "http" is ok, but "fidvr" is not.
  * If the file name must have multiple words, use "_" to separate the words, e.g., "my_file" is ok, but "myfile" is not.
### Module names

  * Only "a-z" are permitted in module names.
  * Module names are not case sensitive.
  * Module names should be descriptive of what superclass of objects the module's classes support, e.g., "buildings", "markets", "climate".
  * Abbreviations should be avoided.
### Class names

  * Only "A-Za-z" are permitted in class names.
  * Class names are case sensitive.
  * Class names should be short enough to describe what the objects are, but no longer.
  * Abbreviations should be avoided.
### Variable names

  * Only "A-Za-z0-9_" are permitted in variable names.
  * Variable names are case sensitive.
  * Abbreviations should be avoided.
### Function names

  * Only "A-Za-z0-9_" are permitted in function names.
  * Variable names are case sensitive.
  * Abbreviations should be avoided.
## Testing/Validation

There are two kinds of testing files in GridLAB-D. Developer tests are maintained in folders named "test". Validation autotests are maintained in folders named "autotest". The conventions for each are very different and important to observe. 

### Developer tests

Developer test files are quite capricious and arbitrary. You should have no expectations about the correctness or usefulness of developer test files. They are generally used as aids in developing or debugging something. Often they are posted to the repository without much explanation or even association with a ticket. As a general rule, if a test file is not very recent, it's fair game to use as you see fit. Be aware that sometimes test files use auxiliary player files and that changing those can affect other test files. 

### Validation autotests

Validation test files are critical to the [build/release process]. This process can be run from the command line using the [validate] command option. Only validation test GLM files placed in folders called **autotest** are run. In addition, the folder must not contain a file name **autotest.no** for the validation test files to be run as the presence of this file prevents the validation script from scanning such folders. 

The file naming convention is critical to how the validation process works: 

test__name_.glm
    The test file must complete successfully for the validation test to pass.
test_err__name_.glm
    The test file must complete with an error for the validation test to pass.
test_exc__name_.glm
    The test file must complete with an exception for the validation test to pass.
test_opt__name_.glm
    The test file is optional and will only be run and reported on when optional tests are enabled.

The results of the validation test are placed in the [workdir] when the tests are completed. 

## Download installers

The naming convention for stable and release candidate download installers is as follows: 
    
    
    gridlabd-_major_ __minor_ __patch_ -_platform_._ext_
    

Nightly builds are named as follows if they pass validation 
    
    
    gridlabd-_major_ __minor_ -_year_ __month_ __day_ -_platform_ -nightly._ext_
    

## See also

  * [Guide to Programming GridLAB-D]
    * Introduction 
      * [Developer prerequisites]
      * Programming conventions
      * [Build/release process]
      * [Documentation Guide]
      * [Theory of operation]
    * [Creating a module]
      * [Module globals]
      * [Module functions]
      * [Subsecond processing]
      * [Import/export]
      * [Check]
      * [KML output]
      * [Example 1]
    * [Creating a class]
      * [Class functions]
      * [Class globals]
      * [Publishing properties]
      * [Publishing methods]
      * [Notifications]
      * [Load methods] 
      * [Example 2]
    * Special Topics 
      * [Data types]
      * [Multithreading]
      * [Application links]
      * [Realtime server]
      * [Graphical user interfaces]
      * [Troubleshooting messages]
      * [Example 3]
    * [Source documentation]
      * [C/C++ Module API documentation (trunk)](http://gridlab-d.sourceforge.net/doxygen/trunk/group__module__api.html)
      * [C/C++ Module API Guide]
      * [Example 4]
    * [Validation]
      * [Example 5]
    * Debugging 
      * [Debug option]
      * [VS2005 (MS Windows)]
        * [use_msvc]
      * [gdb option (linux/mac)]
        * [gdb_window]
      * [Runtime Class Debugging]
        * [compile_once]
    * [Code templates]
