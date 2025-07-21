# Built in schedules and loadshapes

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Built_in_schedules_and_loadshapes

Modelers can create schedules for built-in loadshapes using the **schedule** directive. Each schedule item creates a reusable named schedule that can be referenced later by built-in loadshapes. A schedule can either be a binary or analog series of values that vary according to time. The format for schedules is highly analogous to the _crontab_ format used to schedule jobs in Unix and Linux (see [Wikipedia for cron](http://en.wikipedia.org/wiki/Cron#Fields)). 

# Schedules

Schedules provide an alternative means of defining the behavior of objects in the model. While player files allow for unique values to be defined at arbitrary times in the simulation, schedules use a more compact format that allow the definition of values that follow patterns based on the simulated time. For example, a schedule could be useful to define an increase in load every winter weekday morning to replicate the effects of office buildings turning on their heat in preparation for the workday.

Schedules are defined using a format similar to that of cron, a scheduling tool common in the Unix/Linux world. As in the player files, schedules are used in the model files to play values into object parameters. The format consists of five fields used to specify the times in which the sixth field, the value, will be active. The fields are, in order, minutes, hours, days, months, and days of the week. Ranges of values are acceptable for each of these fields (for example, 8-20 in the hours field indicates 8am to 8pm) and any field whose value is unnecessary in defining the schedule is so designated with a “*”. Schedules are typically defined inside a model file but may also be defined in an external file and included with a `#include filename.txt` statement (where "filename.txt" is file containing the schedule).

Schedules are used to defined a value that changes over time in a pre-defined manner. All times in schedules are considered in local time, including timezone offset and daylight-saving/summer time offsets. Schedules are used by [loadshapes] and by [transforms] to apply the current value to other [built-in types]. 

The general form of a simple schedule is 
    
    
     schedule my_schedule {
       minutes hours days months weekdays value // normal GLM comments
       minutes hours days months weekdays value # schedule-specific comments
       minutes hours days months weekdays value; minutes hours days months weekdays value // semicolon or line delimited
     }

The schedule directive can contain either a simple schedule, such as 
    
    
     schedule officehours {
       * 8-17 * * 1-5 # M-F 8a to 5p
     }
    

or a complex schedule with multiple blocks, such as 
    
    
     schedule officehours {
       weekdays {
         * 8-16 * * 1-5 # Monday through Friday, 8am to 5pm
       }
       weekends {
         * 9-11,13-15 * * 6 # Saturdays, 9am-noon and 1pm to 4pm
       }
     }
    

If you want to provide values for each time interval, they can be listed after the time specification, such as 
    
    
     schedule tou_price {
        * 21-8 * * 1-5 35 # weekdays 9pm-9am, $35
        * 9-20 * * 1-5 135 # weekdays 9am-9pm, $135
        * * * * 6-0 35 # weekends, $35
     }
    

Omitted values on schedule items take on the default value of 1. Omitted times in the schedule take on the default value of 0. 

### Example - Scheduling Values Directly

There are several ways to use schedule and the most obvious is to define the scheduled values as the values to be directly used. Open up the [Players/Schedules/Direct/schedules-directschedules-direct.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%204%20-%20Recorders%20and%20) file and you'll see that there are two schedules that only change based on the minute value in the current simulation time. Running this simulation and looking at the "overhead_line_losses.csv" files shows the following:

```
property.. timestamp sum(power_losses_A.real) std(power_losses_A.real)
2009-01-01 00:01:00 PST 44.961 8.76247
2009-01-01 00:02:00 PST 44.961 8.76247
2009-01-01 00:03:00 PST 44.961 8.76247
2009-01-01 00:04:00 PST 44.961 8.76247
...
2009-01-01 00:12:00 PST 44.961 8.76247
2009-01-01 00:13:00 PST 44.961 8.76247
2009-01-01 00:14:00 PST 44.961 8.76247
2009-01-01 00:15:00 PST 66.0219 18.7422
...
2009-01-01 00:27:00 PST 66.0219 18.7422
2009-01-01 00:28:00 PST 66.0219 18.7422
2009-01-01 00:29:00 PST 66.0219 18.7422
2009-01-01 00:30:00 PST 127.063 25.2421
2009-01-01 00:31:00 PST 127.063 25.2421
...
```

We can clearly see the values changing every fifteen minutes as the load changes. If the simulation is extended in duration to run from midnight to 2am, the values will repeat each hour since the "hour" field in the schedule definitions was left "*" indicating it is valid for all hours. Go ahead and adjust the simulation to run until 2am and also change the hour fields in both schedules to "0". This will only make the schedules only applicable during the times when the hour is 00 (from midnight to 1am). Re-running the simulation shows the following:

```
property.. timestamp sum(power_losses_A.real) std(power_losses_A.real) 2009-01-01 00:01:00 PST 44.961 8.76247
2009-01-01 00:02:00 PST 44.961 8.76247
2009-01-01 00:03:00 PST 44.961 8.76247
2009-01-01 00:04:00 PST 44.961 8.76247
...
2009-01-01 00:12:00 PST 44.961 8.76247
2009-01-01 00:13:00 PST 44.961 8.76247
2009-01-01 00:14:00 PST 44.961 8.76247
2009-01-01 00:15:00 PST 66.0219 18.7422
...
2009-01-01 00:27:00 PST 66.0219 18.7422
2009-01-01 00:28:00 PST 66.0219 18.7422
2009-01-01 00:29:00 PST 66.0219 18.7422
2009-01-01 00:30:00 PST 127.063 25.2421
2009-01-01 00:31:00 PST 127.063 25.2421
2009-01-01 00:32:00 PST 127.063 25.2421
...
2009-01-01 00:42:00 PST 127.063 25.2421
2009-01-01 00:43:00 PST 127.063 25.2421
2009-01-01 00:44:00 PST 127.063 25.2421
2009-01-01 00:45:00 PST 96.0241 13.2564
2009-01-01 00:46:00 PST 96.0241 13.2564
...
2009-01-01 00:58:00 PST 96.0241 13.2564
2009-01-01 00:59:00 PST 96.0241 13.2564
2009-01-01 01:00:00 PST 1.14E-24 4.42E-25
2009-01-01 01:01:00 PST 5.79E-24 2.35E-24
2009-01-01 01:02:00 PST 1.06E-24 3.91E-25
2009-01-01 01:03:00 PST 5.79E-24 2.38E-24
...
```

As you can see, the results for the first hour match that of our previous simulation. Starting at 1am, when our schedules are no longer valid, the losses go to zero, implying that the loads are no longer being applied to the system. This is in contrast to player files where the last defined value is retained by GridLAB-D™and used for any undefined period.

### Example - Schedules as Modifiers

An alternative application for schedulers is to use them to modify existing values rather than defining them directly. This allows schedules to be used to scale all of a given value in a system (load, voltage, etc) by a linear amount while allowing unique specific quantities for each object. Without this capability, getting equivalent functionality would require unique schedules for each object and it would not be obvious from examining the model file that they were all being scaled together.

Open "schedules-modify.glm" to see how this can be implemented. As we can see right away, the schedules in this model have been defined in an external file and are associated with the main model by using "#include". Opening up the referenced file "schedules.glm" we can see the two schedules used in the previous example with new values being applied.

Back in the main file, down where the loads are declared, you can see the load has been declared slightly differently with the real and reactive components as separate parameters of the object; we'll discuss why that is shortly. For each of these components the name of a schedule object is referenced multiplied by a constant value representing a baseline load level. Running this model produces the following from the "underground_line_losses.csv"

```
property.. timestamp sum(power_losses_A.real) std(power_losses_A.real)
2009-01-01 00:01:00 PST 331.432 130.199
2009-01-01 00:02:00 PST 331.432 130.199
...
2009-01-01 00:13:00 PST 331.432 130.199
2009-01-01 00:14:00 PST 331.432 130.199
2009-01-01 00:15:00 PST 548.136 215.328
2009-01-01 00:16:00 PST 548.136 215.328
2009-01-01 00:17:00 PST 548.136 215.328
...
2009-01-01 00:26:00 PST 548.136 215.328
2009-01-01 00:27:00 PST 548.136 215.328
2009-01-01 00:28:00 PST 548.136 215.328
2009-01-01 00:29:00 PST 548.136 215.328
2009-01-01 00:30:00 PST 1347.87 529.494
2009-01-01 00:31:00 PST 1347.87 529.494
...
2009-01-01 00:42:00 PST 1347.87 529.494
2009-01-01 00:43:00 PST 1347.87 529.494
2009-01-01 00:44:00 PST 1347.87 529.494
2009-01-01 00:45:00 PST 832.959 327.217
2009-01-01 00:46:00 PST 832.959 327.217
...
2009-01-01 00:57:00 PST 832.959 327.217
2009-01-01 00:58:00 PST 832.959 327.217
2009-01-01 00:59:00 PST 832.959 327.217
```

As in the previous example, the load changes every fifteen minutes as expected.

Using schedules in this way (called ["/wiki/Transform">"schedule transforms"]) comes with some non-intuitive restrictions.
* Schedule transforms don't support the complex-to-scalar operations, only scalar-to-scalar operations. For example, the following will result in an error message from GridLAB-D:
  
  * `constant_power_A_real load_schedule_2*(20000+5000j);`

* Schedule transforms don't support the use of external units such as kV,MVA,mA, etc. GridLAB-D™assumes all values are the base units even if you do something like:

  * `constant_power_A_real load_schedule_2*20 kVA;`

GridLAB-D™ will not throw an error but simply assumes the load is 20 Watts and compute the powerflow accordingly.
* Schedule transform statements must have the schedule name as the first operand. This will throw an error:

  * `constant_power_A_real 20000*load_schedule_2;`

Because of this, it is not possible to have a schedule transform where both operands are schedules.
* Schedule transform statements don't support the division operation (but do support multiplication by a fraction).

Given these restrictions, the reasoning behind the separation of  the loads in the model into real and reactive components becomes clear. Because schedule transforms don't allow for multiplication between a complex and scalar values, a complex load must be split into its real and reactive components and each one independently modified by the schedule. As was seen in the example above, this was done by appending the load values with `_real` and `_reac`.

## Limitations

There may be no more than 4 blocks, and each block may not contain more than 63 _distinct_ non-zero values. Although you may have more than 64 schedule entries in a block, the number of distinct values cannot exceed 64, and zero is always a value. If you have defined more than 63 distinct values, you can either limit the resolution over the dynamic range, or you can use a orphaned player (i.e., having no parent) as a source instead.

## Normalization

Some schedules need to be normalized before they are used, depending on the application (e.g., loadshapes). When normalization takes place it is done separately over each block. The (optionally weighted) sum of the values given within the block is the normalization coefficient — each value in the block is divided by the sum of all the values in the block. Some applications may need the signed sum and others may use the sum of the absolute values. 

When weighting is used it is based on the fraction of minutes over which the value applies with respect to the total minutes over which the block applies. Only minutes that are explicitly listed in the block are count—omitted times (which are associated with the value 0.0) are ignored. If you wish to have the value 0 counted in the weighting, you must include the times for which it applies as well. 

Normalization is controlled using the following options 

* **normal**
    enables normalization so that all values in each block are divided by the sum of the values in the block.
* **absolute**
    normalization sums uses absolute values instead signed values. Its inclusion implies **normal**.
* **weighted**
    normalization sums uses time-weighted values instead of simple values. Its inclusion implies **normal**.

Normalization options should be provided in-line, such as 
    
    
     schedule demand {
        weighted; 
        * 21-8 * * 1-5 1.2 # weekdays 9pm-9am, weeknights
        * 9-20 * * 1-5 1.5 # weekdays 9am-9pm, weekdays
        * * * * 6-0    0.8 # weekends, weekends
     }
    

which enables normalization using time-weighted values. 

### Nonzero

The `nonzero` flag can be used to ensure that the schedule does not contain any undefined or zero values: 
    
    
     schedule demand {
        nonzero; 
        * 21-8 * * 1-5 1.2 # weekdays 9pm-9am, weeknights
        * 9-20 * * 1-5 1.5 # weekdays 9am-9pm, weekdays
        * * * * 6-0    0.8 # weekends, holidays
     }
    

### Positive

The `positive` flag can be used to ensure that the schedule does not contain any negative values: 
    
    
     schedule demand {
        positive; 
        * 21-8 * * 1-5 1.2 # weekdays 9pm-9am, weeknights
        * 9-20 * * 1-5 1.5 # weekdays 9am-9pm, weekdays
        * * * * 6-0    0.8 # weekends, holidays
     }
    

### Boolean

The `boolean` flag can be used to ensure that the schedule contains on 0 or 1 values: 
    
    
     schedule demand {
        boolean; 
        * 21-8 * * 1-5 1 # weekdays 9pm-9am, weeknights
        * 9-20 * * 1-5 1 # weekdays 9am-9pm, weekdays
        * * * * 6-0    0 # weekends, holidays
     }
    

### Absolute

The `absolute` flag can be used to ensure that the schedule normalization uses only positive magnitudes: 
    
    
     schedule demand {
        absolute; 
        * 21-8 * * 1-5 1.2 # weekdays 9pm-9am, weeknights
        * 9-20 * * 1-5 1.5 # weekdays 9am-9pm, weekdays
        * * * * 6-0    0.8 # weekends, holidays
     }
    
## Schedule transforms

Any _double_ property can be driven from a schedule using a _schedule transform_. Schedule transforms are linear functions that convert a schedule-driven value to a property of an object. A schedule transform can be define by simply describing the function when the property is set, as in 
    
    
     schedule occupancy_schedule {
       * 8-16 * * 1-5
     }
     object small_building {
       n_occupants 10*occupancy_schedule;
     }
     object large_building {
       n_occupancy 100*occupancy_schedule+1;
     }
    

The value of the property is updated before the _presync_ pass, immediately after the schedules are updated. 

As of [Grizzly (Version 2.3)]
    Any property with an underlying numeric value may be driven by a transform. This includes double, complex, bool, [int16], int32, [int64], set, enumeration, [timestamp], [loadshape], and [enduse]. In cases where the underlying data type is compatible, the value will be cast according to the C/C++ type-casting rules.

As of [Hassayampa (Version 3.0)]
    A transform may also link a variable to an external function defined using the [extern] directive.

## Caveats

There are some notable differences from the _cron_ syntax: 

  1. The alternate use of _day_ and _weekday_ is not supported. If both day and weekday are not *, they are considered as _day_ AND _weekday_ rather than OR.
  2. The _step by_ syntax (using /) is not supported.
  3. The special keywords (e.g., @hourly, @daily) are not supported.
  4. The weekday 7 refers to holidays, which can occur any day of the week. Holidays are not supported yet, but will be someday.

Because all times are considered in local time, there is a possibility that scheduled changes during on the daylight-savings/summer time (DST) shifts could result in a missing or duplicate value. For example, scheduling an event at 2am the night DST ends may result in a duplicate value. The solution is to schedule the event either before or after am so the ambiguity is resolved internally as appropriate. Similarly, scheduling an event at 2am the night DST starts could result in a gap in the schedule, a problem also resolved by scheduling the event either before or after 2am so the gap is automatically filled by the schedule compiler. 


# Load shapes

The **loadshape** property is a finite-state machine that takes on the value of a complex power when synchronized. A loadshape is associated with a schedule, and requires a number of parameters to define its behavior. There are 4 types of loadshape, and each has a difference set of parameters that define it. 

## Analog loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/5/59/Analog_loadshape.png/300px-Analog_loadshape.png) ](/wiki/File:Analog_loadshape.png)

[]

Figure 1 - Example of an analog loadshape

Analog loadshapes directly compute the power from the values in the schedule. An analog loadshape is defined using the following terms: 
    
    
     class example {
       loadshape myshape;
     }
     object fixed-energy {
       myshape "type: analog; schedule: _schedule-name_ ; energy: _value_ kWh";
     }
     object scaled-power {
       myshape "type: analog; schedule: _schedule-name_ ; power: _value_ kW";
     }
     object unscaled {
       myshape "type: analog; schedule: _schedule-name_ ";
     }
    

  
The _schedule_ parameter specifies which schedule is to be used. When the _energy_ is given, the schedule is used to create a shape that consumes the specified energy in each schedule block. The power required is based on the fraction of energy allocated to each time interval. 

When the _power_ scale is given, the scheduled value is multiplied by the _power_. 

When neither _energy_ nor _power_ is given, the schedule value is used directly as the power. 

A standard deviation on the _energy_ or _power_ value can be given, in which case each instance of the loadshape that is generated will use an error drawn from the triangle distribution from -3 to +3, such that 

_value_ ← _value_ \+ _stdev_ * Triangle[-3,3]

The _stdev_ term can be given units and it will be scaled accordingly, e.g., 
    
    
     object stdev-power {
       myshape "type: analog; schedule: _schedule-name_ ; power: _value_ kW; stdev _error_ W";
     }
    

## Pulsed loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/3/38/Pulsed_loadshape.png/300px-Pulsed_loadshape.png) ](/wiki/File:Pulsed_loadshape.png)

[]

Figure 2 - Example of a pulsed loadshape

Pulsed loadshapes emit 1 or more pulsed at random times such that the total energy is accumulated over the period of the loadshape. A pulsed loadshape is defined using the following terms: 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape "type: pulsed; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; duration: _value_ s";
     }
    

or 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape "type: pulsed; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; power: _value_ kW";
     }
    

The first form define a series of pulses with constant duration, and the second form defines a series of pulses with constant power. When the duration is constant, the power will vary in response to changes in voltage such that the amount of energy used during a loadshape block is as specified. When the power is constant, the duration will vary in response to changes in voltage such that the amount of energy used is constant. Only one of the two may be specified, and at least one must be specified. 

The **count** parameter determine how many pulses will be generated during a loadshape block. The value is optional and the default value is 1.0. 

A standard deviation on the _duration_ or _power_ value can be given, in which case each instance of the loadshape that is generated will use an error drawn from the triangle distribution from -3 to +3, such that 

_value_ ← _value_ \+ _stdev_ * Triangle[-3,3]

The _stdev_ term can be given units and it will be scaled accordingly, e.g., 
    
    
     object stdev-power {
       myshape "type: analog; schedule: _schedule-name_ ; power: _value_ kW; stdev _error_ W";
     }
    

## Modulated loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/1/14/Modulated_loadshape.png/300px-Modulated_loadshape.png) ](/wiki/File:Modulated_loadshape.png)

[]

Figure 2a - Example of a amplitude modulated loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/1/18/Pw_modulated_loadshape.png/300px-Pw_modulated_loadshape.png) ](/wiki/File:Pw_modulated_loadshape.png)

[]

Figure 2b - Example of a pulse-width modulated loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/4/48/F_modulated_loadshape.png/300px-F_modulated_loadshape.png) ](/wiki/File:F_modulated_loadshape.png)

[]

Figure 2c - Example of a frequency modulated loadshapes

Modulated loadshapes emit a continuous sequence of modulated pulses with either constant period and duty-cycle(**amplitude**), or constant power and off-time (**pulsewidth**), or constant power and on-time (**frequency**). A modulated loadshape is defined using the following terms: 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape "type: modulated; modulation: _modulation_ ; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; period: _value_ s";
     }
    

or 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape  "type: modulated; modulation: _modulation_ ; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; power: _value_ kW";
     }
    

A standard deviation on the _duration_ or _power_ value can be given, in which case each instance of the loadshape that is generated will use an error drawn from the triangle distribution from -3 to +3, such that 

_value_ ← _value_ \+ _stdev_ * Triangle[-3,3]

The _stdev_ term can be given units and it will be scaled accordingly, e.g., 
    
    
     object stdev-power {
       myshape "type: analog; schedule: _schedule-name_ ; power: _value_ kW; stdev _error_ W";
     }
    

## Queued loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/e/ed/Queued_loadshape.png/300px-Queued_loadshape.png) ](/wiki/File:Queued_loadshape.png)

[]

Figure 2 - Example of a queued loadshape

Queued loadshapes emit random pulses whenever a queue accrued from the loadshape reaches an on threshold and continues emitting pulses until the queue reaches an off threshold. A queued loadshape is defined using the following terms: 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape  "type: pulsed; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; duration: _value_ s; q_on: _value_ ; q_off: _value_ ";
     }
    

or 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape  "type: pulsed; schedule: _schedule-name_ ; energy: _value_ kWh; count: _value_ ; power: _value_ kW; q_on: _value_ ; q_off: _value_ ";
     }
    

The values of _q_on_ and _q_off_ are in the same units as the integrals of the normalized loadshape and _q_on_ must be greater than _q_off_. 

A standard deviation on the _duration_ or _power_ value can be given, in which case each instance of the loadshape that is generated will use an error drawn from the triangle distribution from -3 to +3, such that 

_value_ ← _value_ \+ _stdev_ * Triangle[-3,3]

The _stdev_ term can be given units and it will be scaled accordingly, e.g., 
    
    
     object stdev-power {
       myshape "type: analog; schedule: _schedule-name_ ; power: _value_ kW; stdev _error_ W";
     }
    

## Scheduled loadshapes

[![](//images.shoutwiki.com/gridlab-d/thumb/e/ec/Simple_schedule.png/300px-Simple_schedule.png) ](/wiki/File:Simple_schedule.png)

[]

A scheduled loadshape is defined for a population of objects and the aggregate value (heavy black) diversity is built in.

A schedule-based loadshape syntax available from Version 2.1 on has a number of important advantages. 

  1. The definition syntax of the schedule is simpler and more intuitive.
  2. The definition incorporates diversity characteristics that are common in populations of buildings.
For example 
    
    
     class example {
       loadshape myshape;
     }
     object sample {
       myshape  "type: scheduled; weekdays: MTWRF; on-time: 6<8~1<10; off-time: 15<16~1<18; on-ramp: 0.5<1~0.5<1.5; off-ramp: 1<2~1<3; low: 1<2~1<3high: 10<15~2<20 kW;
     }
    

will generate a randomized ramped 8-hour pulse at roughly 10 kW Monday through Friday. Weekdays are defined as 

  * U=sunday,
  * M=monday,
  * T=tuesday,
  * W=wednesday,
  * R=thursday,
  * F=friday,
  * S=saturday, and
  * H=holiday.
Values are provided in the format 
    
    
     _min_ <_mean_ ~_stdev_ <_max_
    

If the min or the max are omitted, then 3 σ is used. If the stdev is omitted, then 0 is used (meaning the value is invariant). 

The syntax for varying values (mean~stdev) allows the same definition to be used for multiple objects, e.g., 
    
    
     #define SCHEDULE_1="weekdays: MTWRF; on-time: 8~1; off-time: 16~1; on-ramp: 1~0.5; off-ramp: 2~1;"
     object sample {
       myshape  "type: scheduled; SCHEDULE_1; power: 15~2 kW;
     }
    

# Reading loads

Loadshapes publish their characteristics when output through a string (say to XML, GLM, or a stream) so to read the actual load at any given time it is necessary to use an alias to access the end-use load. Currently, aliases are not supported in runtime classes, but a module may publish the load alias using the following form 
    
    
     ..., 
     PT_loadshape, "plugshape", &plugs, 
       PT_double, "plugload[kW]", &(plugs.load), 
     ...
    

# Enduse data

Objects should use load data as a part of **enduse** properties. The **enduse** property encapsulates information about how a load is converted to a power, accumulated to an energy and heat gain. The structure is 
    
    
    typedef struct s_enduse {
           loadshape *shape; /* reference to the loadshape from which _load_ is read
           set config; /* enduse configuration (i.e., IS220) */    
           complex total; /* total power (sum of 
           complex admittance; /* constant impedance portion of load in kW) */
           complex current; /* constant current portion of load in kW) */
           complex power; /* value of power in kW */
           complex energy; /* accumulated energy in kWh */
           complex demand; /* peak power observed (can be periodically reset) */
           double impedance_fraction; /* fraction of load that is constant impedance */
           double current_fraction; /* fraction of load that is constant current */
           double power_fraction; /* fraction of load that is constant power */
           double power_factor; /* power factor of load */
           complex voltage_factor; /* voltage in pu */
           double heatgain; /* internal heat from load */
           double heatgain_fraction; /* fraction of power that goes to internal heat */
    } enduse;
    

End-use power fractions are update differently depending on whether the end-use is driven from a loadshape or driven externally. 

When driven from a loadshape, the power, current, and impedance fraction are updated first, along with the total power and they are used to calculate the constant power, current, and admittance portion of the total power. 

When driven externally, the constant power, current, and admittance portion of the total power are update first, and the fractions and total power are calculated from them. 

When _enduse_recalc_ is called, the following calculations are made, depending on the update requested. When a _presync_ update is requested the energy is calculated as a function of the power and time given, i.e., 
    
    
     enduse_recalc(int PC_PRESYNC, double dt)
    

$$energy = power \times dt$$

Important
    The _presync_ update is done automatically by the main executive and should not be called explicitly by the object's _presync_ implementation, if any.

When a sync update is requested the power is calculated as a function load, and the heat is updated as a function of power, i.e., 
    
    
    enduse_recalc(int PC_SYNC)
    

$$power = set\_power\_factor(shape_{load},power\_factor) \left ( power\_fraction + \frac{current\_fraction}{voltage} + \frac{impedance\_fraction}{voltage^2} \right ) $$

$$heatgain = power_{real} \times heatgain\_fraction$$

Important
    The **enduse** _sync_ update is not performed automatically and must be called by the _sync_ operation of the object that implements the **enduse**. This is done using the _gl_enduse_sync_ call:
    
    
    TIMESTAMP myclass::sync(TIMESTAMP t0, TIMESTAMP t1)
    {
        TIMESTAMP t2 = TS_NEVER;
    
        /* ... my calcs before enduse sync */
    
        /* synchronize my enduse property */
        TIMESTAMP t3 = gl_enduse_sync(myenduse,t1);
        if (t3<t2) t2=t3;
        /* power and heatgain are now set based on loadshape */
    
        /* continue with calcs ... */
    
        return t2;
    }
    

The limitation is for performance reasons — there is at present no efficient way to determine which **enduse** blocks belong to which objects and consequently the _sync_ operation in the core cannot know when each block needs to be updated. Furthermore, depending on the implementation of the object's _sync_ function, the synchronization of the **enduse** block may need to occur in the middle of the object _sync_. For these reasons, the **enduse** _sync_ must be called explicitly during the object _sync_ at the appropriate time. 

# Module integration

There are two ways to use schedules, loadshapes, and enduses in modules. The first is to directly drive the enduse from the loadshape. This is done by including the statement 
    
    
     load.shape = &shape;
    

which in the case of residential enduses is done in `residential_enduse::create()`. In this case, the total power is calculated based on the load and the enduse fraction, e.g., 

_total r_ = _load_ ( _f V_ ( _f V Zf_ \+ _I f_ ) + _P f_ )   
_total i_ = Sgn(_pf_) _total r_ (_pf_ -2 -1)1/2

The second way separates the loadshape from the enduse so that a model of how the load converts to power can be explicitly given. In this case, the above statement is omitted, leaving `load.shape` null. 

Now you must provide the ZIP components as actual real and reactive quantities and the total power is computed, e.g., 

_total r_ = _f V_ ( _f V Zr_ \+ _I r_ ) + _P r_ )   
_total i_ = _f V_ ( _f V Zi_ \+ _I i_ ) + _P i_ ) 

In addition, the enduse load is named using a statement like, 
    
    
     load.name = oclass->name;
    

so that the convention of naming the enduse the same as the class that implements it is observed. 

# Caveats

Modules that implement defaults in the style commonly used in Version 1 of GridLAB-D will find that this is no longer possible. The following changes are required to use loadshapes and enduses. 

  1. You can rely on _object_create_single_ to both clear memory and properly create and initialize both _loadshape_ properties and _enduse_ properties.


  2. All other object property defaults must be done in _create_.
