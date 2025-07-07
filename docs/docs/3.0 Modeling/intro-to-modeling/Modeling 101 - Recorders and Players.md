---
title: Recorders and Players
excerpt: >-

deprecated: false
hidden: false
metadata:
  robots: index
---
# Introduction to Recorders and Players

Data input and output is an essential part of any simulation, and simulations with GridLAB-D™ are no exception. If you’ve read through and run the simulations covered in the ["Basic_Distribution_System_Modeling" >previous section], you’ve seen that to be useful GridLAB-D™ must explicitly include objects in the model file that will record data of interest. If none of these recorder objects are included, there are effectively no results to the simulation. Similar to `recorders` is a parallel object called a `player` which allows externally defined data to be played into a simulation, affecting the behavior of that object and thus the simulation as a whole.

Both recorders and players create a mapping between internal parameters of GridLAB-D™ objects and external data files. So, to use these objects, an obvious hurdle must be cleared: how does a user find the names of the class parameters to which a recorder or player might be tied? There are two good answers to this: the GridLAB-D™wiki and the GridLAB-D™source code.

The ["/wiki/Main_Page">GridLAB-D™ wiki] (from which you are likely reading this tutorial) is a relatively user-friendly description of functionality of GridLAB-D™as a whole and contains many specific details on how many (but sadly not all) GridLAB-D™classes operate. As part of this description of the GridLAB-D™class is a listing of many of the parameters used by said class to perform its calculations. For example, going to the ["/wiki/Power_Flow_User_Guide">GridLAB-D™ Power Flow User Guide] page and clicking on the “Node” link on the right sidebar, we are provided not only with a brief description of how nodes are represented in GridLAB-D™but also an example of how node objects can be created in GridLAB-D™and a list of the parameters associated with the node class (and hence each node object). This parameter list generally defines the parameters with which recorders and players can interact.

If the GridLAB-D™wiki has a problem, it is that it doesn’t get updated as often as the GridLAB-D™ code itself and the code is what truly defines how the software operates. For classes that are well established (such as “Node”) it is likely that the wiki documentation is up-to-date. For newer classes, though, this may not entirely be the case and examining the source code itself may be the best route to finding details that may be needed. Examining other people’s code is never simple and GridLAB-D™has grown into relatively complex software over time; it is easy to feel overwhelmed and confused when first starting to examine the guts of GridLAB-D™. The first time you really must understand why a particular object is doing what it does, being able to jump into the source code and find the one or two lines that answer the question will be very helpful and satisfying.

So let's try it and see how to go about examining GridLAB-D™source code for these elusive parameters. To find out what parameters the node class has, we first have to find the file containing the source code that defines the node class. All of the code for GridLAB-D™is in the source code folder (hopefully downloaded as a part of the installation process you went through, see ["Installing_GridLAB-D" >Chapter 1 - Installing GridLAB-D]) and from there we can either try using the OS or an IDE like Eclipse or Xcode to search through those files for “node”.  Given how fundamental nodes are to GridLAB-D™, this is likely to return many files so, alternatively, we can try to intuitively navigate the source code tree and find the file ourselves. Trying the later route and looking through all the source code folders, we can see there is one called “powerflow” and inside that folder is a file called “node.cpp”. Opening this text file and reading the introductory comments readily confirms that this is the source code for node objects.

Now, to find those parameters. Scrolling down a bit we find a long list of statements that look something like this:

```
PT_INHERIT, "powerflow_object", 

PT_enumeration, "bustype", PADDR(bustype),PT_DESCRIPTION,"defines whether the node is a PQ, PV, or SWING node",

PT_KEYWORD, "PQ", (enumeration)PQ,

PT_KEYWORD, "PV", (enumeration)PV,

PT_KEYWORD, "SWING", (enumeration)SWING,

PT_set, "busflags", PADDR(busflags),PT_DESCRIPTION,"flag indicates node has a source for voltage, i.e. connects to the swing node",

PT_KEYWORD, "HASSOURCE", (set)NF_HASSOURCE,

PT_object, "reference_bus", PADDR(reference_bus),PT_DESCRIPTION,"reference bus from which frequency is defined",

PT_double,"maximum_voltage_error[V]",PADDR(maximum_voltage_error),PT_DESCRIPTION,"convergence voltage limit or convergence criteria",


PT_complex, "voltage_A[V]", PADDR(voltageA),PT_DESCRIPTION,"bus voltage, Phase A to ground",

PT_complex, "voltage_B[V]", PADDR(voltageB),PT_DESCRIPTION,"bus voltage, Phase B to ground",

PT_complex, "voltage_C[V]", PADDR(voltageC),PT_DESCRIPTION,"bus voltage, Phase C to ground",

PT_complex, "voltage_AB[V]", PADDR(voltageAB),PT_DESCRIPTION,"line voltages, Phase AB",

PT_complex, "voltage_BC[V]", PADDR(voltageBC),PT_DESCRIPTION,"line voltages, Phase BC",

PT_complex, "voltage_CA[V]", PADDR(voltageCA),PT_DESCRIPTION,"line voltages, Phase CA",

PT_complex, "current_A[A]", PADDR(currentA),PT_DESCRIPTION,"bus current injection (in = positive), this an accumulator only, not a output or input variable",

...
```

The quoted values are the class parameter names we are looking for; the value in “[ ]” is the default unit of the parameter. That is, `node`'s have a parameter called `voltage_A` and its default unit is volts. We can also see that the programmer of node class left a comment that the voltages are line-to-ground values and not the more traditional line-to-line values; good-to-know. And, as promised, looking at the source code has revealed a few parameters not listed in the wiki (at least at the time of this writing) such as `service_status` and `current_uptime`. When it comes to finding class parameter names, most of the GridLAB-D™modules are as straight-forward as this.


## Time in GridLAB-D

Before we dig further into the details of recording data out of or playing data into GridLAB-D™, we need to explain how time (as in time of day) in GridLAB-D™ is tracked. Internally, GridLAB-D™uses UTC as its clock but has support for presenting time in any timezone with or without daylight saving time. Near the top of every GridLAB-D™ model (“.glm”) is a `clock` statement which defines the local time in which the simulation will be run. Also included in that clock statement are the dates and times of the beginning and ending of the simulation period. GridLAB-D™represents hours of the day using a 24-hour clock with 00:00 being midnight, the beginning of the day, and 23:59 being the last minute of the day, 11:59 pm.

GridLAB-D™ also takes care of the time changes that occur when daylight saving time begins and ends. In the United States these times have changed over the years. To allow GridLAB-D™ to correctly track local time, the dates of the beginning and end daylight saving time are included in a specific file GridLAB-D™ references: “tzinfo.txt". This file is found in the source code “core” folder and is copied to the installation's "share" folder during installation process. As of the writing of this tutorial, there are three distinct daylight saving time translation rules as a function of the year of the simulation. 

The timezone for the simulation can be specified as an offset from UTC (with daylight saving time as necessary): `PST+8DST`. GridLAB-D™also allows users to specify this offset by defining the geographic location in which the simulation is set: `US/WA/Seattle`. The list of supported location is relatively limited and can be found in “tzinfo.txt”.

As a demonstration, open up “distribution_system_base.glm” from the ["Basic_Distribution_System_Modeling" >previous chapter] and make a few small changes:

* Change the `clock` statement so that the simulation starts on March 1st and ends on April 1st.

* Change the two recorder intervals from minutely (60) to hourly (3600).

Run the model and take a look at one of the output files generated by the recorder. In 2009, daylight saving time began on March 8th at 2am. At that time, the local clocks were all advanced an hour, effectively jumping from 1:59am to 3:00am. Looking at the recorder output file, we can see this reflected in the data that was collected, as expressed in local time. “tzinfo.txt” indicates that this latest definition of daylight saving time began in 2007; if we re-run the simulation setting the `starttime` and `stoptime` to be in the year 2000, we see that no hour was skipped on March 8th or any day in March as daylight saving time that year began in April.

## Recorders

As you’ve already seen in the ["Basic_Distribution_System_Modeling" >previous chapter] where we built a very basic distribution system model, recorders are the fundamental means of getting simulation results out of GridLAB-D™. They can either be declared as their own object explicitly parented to the object with data of interest by adding a `parent ...;` statement to their definition or they can be implicitly parented by including their definition inside the definition of the object of interest. Recorders aren't typically complicated: all that is generally required is a definition for the file to write the data out to, how often the values should be measured, the parameter of interest, and (if making an explicit recorder) the parent object.

```
object recorder {
  parent branch_1_meter_1;
    file branch_1_meter_1.csv;
    interval 60;
    property measured_voltage_A, measured_current_A, measured_power_A;
};
```

There are a few other interesting parameters for the recorder object that weren't in this basic definition that are worth mentioning here. ["/wiki/Recorder#limit">`limit`] can be used to limit the number of measurements taken; this can be useful to capture one-second data for the first few minutes of a twenty-four hour simulation run when all other parameters are only measured once every five minutes. Similarly, a ["/wiki/Recorder#trigger">`trigger`] statement can be used to define an initial value or condition that will initiate recording.

GridLAB-D™ also supports a type of recorder that is not restricted to data from one object but can be used to measure data from multiple objects into one file: the ["/wiki/Multi-recorder" >multi-recorder]. A multi-recorder is useful when doing things like recording the voltages from a few separate nodes and have them written to a single file. And like a recorder, it can record data of different types as well; allowing for a large degree of flexibility. The only real cost is in typing; a multi-recorder requires that the parameters being measured be prepended by the objects name followed by a colon.

If the goal is to get a general sense of how a collection of objects as a whole is doing, though, there’s a better way than listing each object out one-by-one in a multi-recorder. GridLAB-D™contains an object called a `collector` which allows users to define a collection of objects (such as all nodes in the model) and then define a summarizing statistic (i.e. average, minimum) to be used as the value to be logged. GridLAB-D™even goes one step further and allows custom naming metadata to be associated with individual objects and this name to be used to form compound logic statements in the collector definition. For example, a collector might be defined to create a group of objects whose class type is `node` and whose `groupid` is “load_node”.

In the example collector below, all objects of class `house` and with the custom `groupid` of "Residential" will have their `cooling_setpoint`s collected every five simulated minutes and the average of all those values written out to a file named "residential_setpoints.csv".

```
object collector {
  group "class=house AND groupid=Residential";
  property avg(cooling_setpoint);
  interval 300;
  file residential_setpoints.csv;
}
```

### Example - Recorders

Let’s see how this functionality might look in an actual model. Open up (https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%204%20-%20Recorders%20and%20Players/Players/playersplayers.glm) and spend a few minutes looking over the model. You can download the entire Tutorial repository [here](https://github.com/gridlab-d/course/). In fact, as an exercise, make a quick sketch of how this model is constructed, showing how the objects are connected to each other. As you begin doing this you’ll quickly see that this model definition is arguably not structured as neatly as the previous examples we’ve looked at. For example, some objects are defined before their parents; GridLAB-D™completely allows for this, though the users of models constructed like this may not care for this arrangement. Here’s what the model looks like.

!["Players model diagram scaled](/images\Players_model_diagram_scaled.png)

The end of the model contains a list of the recorder objects using all three data recording types: recorders, multi_recorders, and collectors. Running the model generates files from all three and comparing the data from all three hopefully demonstrates the utility of each. For getting data from just a few objects, using simple recorders can be quite sufficient. If detailed data is needed from a number of objects it may be easier from a post-processing stand-point to have all that data contained in one file and a multi-recorder might be best. If only summary statistics are needed, rather than calculating those in a post-processing script it may be easiest to use a collector object to do the math for you.

A few other notes about collectors: First, there are a number of other operations collectors can do besides avg, min, max; you can find a listing on the ["/wiki/Collector">collector wiki page]. Secondly, collectors can only perform operations on scalars as doing things like taking the average of a complex number is not always meaningfully defined in power systems. To convert a complex value to a scalar,  append the parameter name with a “part”; the valid parts are:
* .real - real part of the complex value

* .imag - imaginary part of the complex value

* .mag - magnitude of the complex value

* .ang - angle in degrees of the complex value

* .arg - angle in radians of the complex value

## Group Recorders

The group recorder can collect a recording of **ONLY ONE** property from a class or a group of similar objects. For instance, it can be used to record the voltages of every meter in the system as a time series. The values of the measured_energy variable of every meter object in a GLM are recorded into a file, named as "meter.csv", at 3600-second intervals. 
    
    
    object group_recorder {
      name MeterCorder;
      parent meter;
      group "class=meter";
      property measured_real_energy;
      file meter.csv;
      interval 3600;
      limit 1000;
    }

The group_recorder places a [timestamp] in the first column of every row it emits. By default the [timestamp] is formatted using the [ISO] format (i.e., **yyyy-mm-dd HH:MM:SS TZ**). However, if the value of the [dateformat] [global variable] is not [ISO], then the alternative date/time formatting rules will apply as follows: 

#set [dateformat]=[US]
    The [timestamp] will be formatted as **mm-dd-yyyy HH:MM:SS[.SSSSSS]**.

#set [dateformat]=[EURO]
    The [timestamp] will be formatted as **dd-mm-yyyy HH:MM:SS[.SSSSSS]**.

### Group Recorder Examples

**Example 01:** Three group_recorder objects are used to record the Phase A, B, and C voltages of all loads into three files. A MATLAB script that parses and plots the voltages of all loads is available in this GitHub [repository](https://github.com/wsu-smartcity/tool-scripts/tree/master/gridlabd/group%20recorder). 
    
    
    object group_recorder {
    	name loads_voltages_A_recorder;
    	group "class=load";
    	property voltage_A;
    	file loads_volts_A.csv;
    	interval 1;
    	limit 10000000;
    }
           
    object group_recorder {
    	name loads_voltages_B_recorder;
    	group "class=load";
    	property voltage_B;
    	file loads_volts_B.csv;
    	interval 1;
    	limit 10000000;
    }  
    
    object group_recorder {
    	name loads_voltages_C_recorder;
    	group "class=load";
    	property voltage_C;
    	file loads_volts_C.csv;
    	interval 1;
    	limit 10000000;
    }
    

**Caution:** It is possible to have the voltages recorded as magnitude and angle (e.g., +7003.99-2.25958d) in the csv file. In this case, the MATLAB script in that GitHub repository needs to be modified to process the string "+7003.99-2.25958d". 

1) Note that this is not controllable by users through property settings. Users may have a single csv file, in which the recorded voltages have a mix of two formats (i.e., the magnitude and angle format: "+7003.99-2.25958d", and the real and imaginary format: "+2465.49-1430.37j"). This bug will be fixed in future. 

2) The property "complex_part" can be used to record the magnitude of a phase voltage. See Example 02, of which the output file can be parsed by the provided MATLAB script properly. 

**Example 02:** A group_recorder that will watch the 'A' voltage of all meters and record the magnitude every 60 seconds. 
    
    
    object group_recorder {
       parent ThatNode;
       group "class=meter";
       property voltage_A;
       interval 60;
       limit 1000;
       file ThatNode_kV.csv;
       complex_part MAG;
    }
    

**Example 03:** A group_recoder that measures real power through every transformer in a specific section of the circuit (through groupid) every 5 mins. 
    
    
     object group_recorder {
       parent ThatTransformer;
       group "class=transformer";
       property power_out;
       interval 300;
       limit 1000;
       file AllTransformers.csv;
       complex_part REAL;
    }
    

### Properties

* **parent object**
    Built-in property that specifies the object that the group_recorder childs to. Does not need to be specified.

* **property string**
    Single property from the class to be recorded. Properties with units may be converted to other relevant units. Complex properties may be modified via the complex_parts enumeration.

* **file string** 
    By default, the name of the file to write the recorder output to. If left empty, the recorder will generate a file name based on the target object class and internal ID number. The exact mode is dependent on the format of this string. A simple file name will write text output to the specified file. Other output modes are available with "mode:path", where mode may be "file", "odbc", "memory", or "plot". The path for file and plot refer to a file name, to a global variable name for memory, and to a server login string for odbc. See the [Tape Database Output], [Tape Memory Output], and [Plotting Output] sections for more details.

* **flush number**
    By default the output buffer is flushed to disk when it is full (the size of the buffer is system specific). This default corresponds to the flush value -1. If flush is set to 0, the buffer is flushed every time a record is written. If flush is set to a value greater than 0, the buffer is flushed whenever the condition `clock mod flush == 0` is satisfied.

* **interval integer**
    The frequency at which the recorder samples the specified properties, in seconds. A frequency of 0 indicates that they should be read & written every iteration (note, that each timestep often requires multiple iterations, so a frequency of zero may lead to multiple measurements in a timestep). A frequency of -1 indicates that they should be read every timestep, but only written if one or more values change. By default, this is TS_NEVER.

* **limit integer**
    The number of rows to write to the output stream. A non-positive value puts no limit on the file size (use at your own risk). By default, this is 0. The limit is only checked when output non-subsecond value.

* **group string**
    Group definition string. Defines which class of device (required) and other information to create a group. See [Finding_objects] for more details.

* **flush_interval integer**
    How often to "flush" the recorded material to the flat file - file flush interval (0 never, negative on samples)

* **strict boolean**
    Causes the group_recorder to stop the simulation should there be a problem opening or writing with the group_recorder

* **print_units boolean**
    flag to append units to each written value, if applicable

* **complex_parts enumeration**
    Which part of the complex to record if a complex property is specified. Available settings: "NONE", "REAL", "IMAG", "MAG", "ANG_DEG", "ANG_RAD"

### Group Recorder Caveats

The group_recorder attempts to read the value from the last iteration of a timestep, rather than the first iteration of a timestep. Normally the final value is more important than the initial or the intermediate values, for iterative solvers, but an interval of -1 can be used if necessary to record the value of a property with greater resolution. 

The group_recorder cannot currently record the final value into a file, even though that value will be present in the output or dump XML file. This is a known quirk that cannot be resolved with the current structure of GridLAB-D, but is being worked on. All values written by various recorders at the same timestamp will be consistent between each other, however.

## Alternative Data Output - XML

Using recorders, very specific properties of different objects in your model can be read out and saved to a file. There is an alternative technique to get all object data out formatted as XML to match the hierarchy of the .glm. Though the format is not as common as CSV, the generated XML file is a good one-stop solution to look at voltages at all nodes in one place and check if your model is working as expected. There are two ways to get XML output after running a GridLAB-D™model file. The first and simplest is to add an extra parameter to the command line call; this will generate the XML file at the end of simulation. 

```
gridlabd distribution_system_base.glm –o myfile.xml
```

Alternatively, you can add a special command inside your .glm to create the XML output file every time the model is run.

```
#set savefile="myfile.xml"
```

There are a large number of XML viewers/editors out there but some have a hard time with handling the "infourl" tag; setting it to null in the model file and re-running the simulation often solves this problem.

```
#set infourl=
```

## Players

The mirror to the recorder object is the player object; the player object allows external data to be used to define internal parameters of GridLAB-D™objects. They are useful for hard-coding known behavior in system (rather than trying to model it) or for playing in data external to the system. For example, it is common to think of the transmission system defining the voltage at the head of a distribution feeder, the substation. These values could be found by including a model of the transmission system in the same model of a distribution system and endogenously generating the appropriate voltages. Alternatively, recorded values of transmission system voltages (from real-world system or other simulations) could be played into the GridLAB-D™simulation at the substation of the distribution model. This obviously breaks the connection between distribution system load and transmission system voltage but it is more realistic than having static values for substation voltage.

The format for player files is very straight-forward: two columns with the first being a timestamp and the second being a value. There are three acceptable formats for the timestamp:

* Absolute number of seconds since midnight January 1, 1970 (the Unix epoch time)

* Absolute date and time: 2009-03-01 13:24

* Relative time since the last timestamp: +30s (acceptable units are seconds “s”, minutes “m”, hours “h”, and days “d”)

The last timestamp format is very handy when the value will be updated at regular intervals. The first timestamp entry in the file must be absolute but all the remaining times can be specified using a relative value relieving the user of ensuring a long list of absolute timestamps appropriately roll over at, say, month boundaries.

The format for player objects is very similar to recorder objects with statements that define the filename to get the recorded values from and a parent object and parameter name into which the recorded values will be applied. And like recorders, this parenting can be defined explicitly or implicitly. As compared to recorder files, though, finding which parameters can be written to has a slight wrinkle. The list of parameters that are exposed by each class is the same as in recorder objects and can be found in the same way: the GridLAB-D™wiki and/or the source code. The complication though, is that though GridLAB-D™may allow a player to write to all of those parameters, the internal workings and calculations of each class may effectively overwrite those played-in values.

Take the example we just worked when experimenting with recorders. When the powerflow solves the system, it defines the voltage at each of the nodes as a function of the voltage at the feeder head, the loads at each node, and the impedance of the distribution lines. Given this, it would make sense that trying to define the voltage at each node through a player file would not work; defining the output of the powerflow solution does not affect the solution itself and would result in those played-in values being overwritten each time the powerflow is solved. Using player files to define parameters that are outputs of the internal GridLAB-D™calculations is ineffective.

In using player files, then, the hard part is determining whether a particular parameters listed on the wiki or in the source code is an input parameter that can be played into or an output parameter which is only useful for recorders. Unfortunately, there is no GridLAB-D™reference other than the source code itself to definitively determine if a particular parameter is an input or output for a particular class. Depending on the parameter and the class, digging into the source code to find an answer may be non-trivial. 

A little advice on how to handle the problem:
* Be aware that this problem does exist and be looking for it if a player doesn't seem to be working.

* Think through generally how the class algorithms should work and do your best to guess which parameters are inputs and which are outputs.

* The most direct way of determining if a parameter can be played into is to just try it and see if it works.

### Example - Players

With that warning in mind, lets take a look at this model for recorders and see what happens when we play a voltage into the substation (which is a swing node and can be freely defined, unlike all the other nodes in the system). Run the "players" model from the Tutorial repository [here](https://github.com/gridlab-d/course/tree/master/Tutorial/Chapter%204%20-%20Recorders%20and%20Players/Players):

```
gridlabd players.glm
```

Examining any of the results files generated shows that the changing substation voltage is affecting the rest of this small distribution system. Rather than static values for the duration of the simulation, we see changing values every minute in step with the substation voltage.

Player objects have a `loop` parameter that allows the same data file to be repeated a specified number of times. We can see this affect by changing the node object so that is it defined as

```
object node {
  name feeder_head;
  bustype SWING;
  phases ABCN;
  nominal_voltage 132790;
  object player {
    property voltage_A;
    file sub_voltage.player;
  loop 1;
  };
}
```

The player file will now loop once, playing back two hours of data. Note that the first value in the file, the one with the timestamp of `2009-01-01 00:00:00 PST` will *not* be played on the second loop as this is an absolute value. To compensate for this, the voltage at that time has been added to the end of the file as the final entry with a relative timestamp.

Though we have set up the player to produce two hours of data, the `clock` statement at the beginning of the model has not been changed and if no modification is made, the simulation will still only run from midnight to 1am. Modify the `clock` and re-run the simulation. Examining the recorded substation data will show that the voltage does begin to repeat at 1am, as expected.

What if the converse scenario occurs, though? What if the simulation runs longer than the player file provides data? Let's find out; modify the `clock` statement again, this time to end the simulation at 3am and re-run it. The substation data will now show that, beginning at 2am, after the player file is done looping, the final player value repeats for the remainder of the hour. Until some other object comes along to change it, the substation voltage stays at its last defined value.
