---
title: GLM Models
excerpt: '101'
deprecated: false
hidden: false
metadata:
  robots: index
---

# Introduction

GridLAB-D™ is a power system simulation tool that provides valuable information to users who design and operate electric power transmission and distribution systems, and to utilities that wish to take advantage of the latest smart grid technology. It incorporates advanced modeling techniques with high-performance algorithms to deliver the latest in end-use load modeling technology integrated with three-phase unbalanced power flow, and retail market systems. Historically, the inability to effectively model and evaluate smart grid technologies has been a barrier to adoption; GridLAB-D is designed to address this problem. 

This guide to using GridLAB-D™ is intended to help those who are at least slightly familiar with distribution systems to establish a foundation that will allow them to use GridLAB-D™ in their work. It is not intended to be comprehensive as GridLAB-D contains many models with many parameters, but rather to address some of the more important and popular features. The guide will not only address practical issues such as how certain models function but also more general topics exploring the architecture of GridLAB-D™. 

# What is a GridLAB-D™ model?

A GridLAB-D™ model can be stored in either GLM (GridLAB-D™ Model) or XML (Extensible Markup Language) files, often with other files around to help define simulation boundary conditions and provide needed supporting data.  The names GLM and XML come from the extension used in the file name, .glm and .xml, respectively.

GLM files are not exactly like XML files, though. An XML file is a faithful representation of the exact model being simulated, while a GLM file can provide underlying information such a parametric values and statistical distributions for properties, as well as specifications for synthesizing populations of objects so that large models can be generated from relatively simple descriptions. GLM files also allow you to describe the behavior of objects in the simulation, whereas XML files cannot. A .glm file is used to synthesize a population of objects. This is in contrast to an .xml file, which is used to represent a specific population that has been synthesized. 

In general we will describe models using the GLM file format and reserve XML for two main functions: 1) viewing output results, and 2) exchanging data with other tools.

Consider a simple example.  We need to define a [house](docs:house_e), each instance of which has a \["/wiki/Floor\_area">floor\_area], drawn from a normal distribution about a mean of 2500 sf with a standard deviation of 250 sf. Compare the GLM object definition

```
module residential;
object house {
  floor_area random.normal(2500,250);
};
```

with the XML code that might be generated after loading it

```
<module>
  <residential/>
<module>
<house>
  <floor_area>2534.2 sf</floor_area>
</house>
```

The GLM file described the property \["/wiki/Floor\_area">floor\_area] as having a random value with a normal distribution.  If you were to run GridLAB-D™ multiple times using this object description, then depending on how the random number generator is set up you might see a different value for the floor area each time. But overall the values would have a a mean 2500 sf with a standard deviation of 250 sf.  In contrast the XML file simply describes one realization of that object and each time you load the XML you will get the same value for the floor area.  The information about randomness is lost in the XML file, but repeatability of the results is gained.  Therefore, you should exercise care and use the appropriate type of files depending on the task you need to accomplish.  In general, model exchange will require XML files and stochastic (or Monte Carlo) studies will require GLM files.

## Comments
Comments begin with a `//` sign. All text between the `//` and the end of the line is ignored by the parser.

It is important to note that the parser in GridLAB-D™ is quite primitive and may in certain circumstance be confused by `//` appearing in other contexts, such as a URL not enclosed in a quotes.

##  Macros
The .glm loader allows the use of macros to control the behavior of the parser and to a limited extent also the behavior of GridLAB-D™. Macros are lines that begin with a '#' sign. The following macros are available

- `#define` name=value is used to define a global variable. This allows the creation of a new global variable, in contrast to the #set macro which requires the global variable already exist.
- `#set` name=value is used to set a global variable. For a list of defined global variables, see the [Doxygen Documentation].
- `#undef` is used to remove the definition of a global variable.
``` 
#ifdef|ifndef <expression>
...
[#else
... ] 
```
`#endif` is used to conditionally process a block of text in the .glm. For `#ifdef` and `#ifndef` the expression is simply the name of a global variable. When #if is used, the expression is a conditional test in the form of name op value where the operator op is one of <, >, &lt=, >=, ==, or !=.
- `#include` file is used to include text from file in the loaded text.
- `#print` name is used to display the value of the global variable <name> at the moment it is encountered by the loader.
- `#ifexist` file is used to determine whether a file exists
- `#include` file is used to include another file during the parser load
- `#setenv` name=string is used to set an environment variable
- `#binpath` path sets the path for binary searches (compiler PATH environment)
- `#libpath` path sets the path for library searches (compiler GLPATH environment)
- `#incpath` path sets the path for include file searches (compiler INCLUDE environment)
- `#error` message triggers a parser error condition
- `#warning` message triggers a parser warning condition
- `#option` command-option applied the command line option
- `#system` command makes an operating system call and waits for completion before continuing processing the GLM file
- `#start` command makes an operating system call and continues processing the GLM file

# Model terminology

In the previous examples, we introduced two very important concepts.  The first is notion of an \["/wiki/Object">object].  Objects describe specific instances of a thing that can respond to and/or act on other objects.  A simulation can contain very many objects.  It is not uncommon to see a model that contains thousands of objects.  Some of the large GridLAB-D™ models have been built that contain well over 100,000 objects.

However, describing so many objects in detail can be an overwhelmingly difficult challenge.  This is where the second notion comes in: a \["/wiki/Class">class].  Classes are used to group object according to similar properties and behavior.  In general we define a class of objects that are similar in structure, but we instantiate objects that exhibit specific properties.  For example, according to the Department of Motor Vehicles a person has a name, birth date, height, weight, age, eye and hair color all of which are associated with their driver's license number.  This collection of information is the same for all drivers.  So a person is a class.  John Q Smith, born on May, 1968, who is 6'2", 200 lbs, 32 years old, brown hair and brown eyes and has driver's license number "SMITHJQ325KE" is an instance of that class.

Accordingly, the class definition for the house used above would look something like this

```
// class example
class house { 
  double floor_area[sf];
};
```

which tell GridLAB-D™ that the variable \["/wiki/Floor\_area">floor\_area] is a property of all \["/wiki/House">houses] and it is stored in \["/wiki/Units">units] of \["/wiki/Units">sf] (i.e., square feet).  Note that it is not necessary to define the \["/wiki/House">house] class this way because \["/wiki/Modules">modules] this for us in a way that ensures everybody uses the same properties and behaviors (see below).

XML files do not really support classes, at least not the way GLM files do.  In a GLM files, the class can be defined fully from nothing, but in an XML file, it can only be described in a limited fashion.  This is because the definition is not easily transportable from one system to another, but the description is something that other systems often require in order to know how to interpret the model.

For this reason, XML files often use a description file (called an XSD file) to know how to interpret the XML file.  The \["/w/index.php?title=XSD\&action=edit\&redlink=1" >XSD] contains all the class information, but XSD files omit many aspects that are specified in GLM files.  GLM files provide instructions called directives on what GridLAB-D™ is supposed to do when it tries to update an object, which is something that is largely irrelevant to (or incompatible with) other tools that might look at a GridLAB-D™ XML file.

From here on, we will discuss modeling in the context of GLM files, and reserve the special consideration for XML files when they are discussed in later chapters.

## Modules

Static modules must be loaded before the classes they define can be used or modified. To load a static module, use module name;. The module loader will search the GLPATH environment variable to locate a file named `name.dll` (on linux the search will be for `libname.so.`) If a particular version is desired, the version must be appended to the name using the format module name-major; or module name-major_minor; depending on whether you wish to specify on the major version, or both the major and minor version.

Module blocks may include additional information, such as assignments of the values for module globals and specification of the version number. To set a module global variable, simply include the name and value in the module block, such as
```
 module MyModule {
   MyStringGlobal "value";
   MyEnumGlobal A;
   MyDoubleGlobal 1.2 ft/s;
 }
```
To enforce verification of the module's version information, simply include the desired version in the module block:
```
 module MyModule {
   major 2;
   minor 1;
 }
```
If the version loaded does not match the version specified, an error will be displayed and the loader will stop.

## Classes
Class blocks are used to create, modify, or verify class definitions. If a class is already defined in a static module, then a class block either modifies or verifies the definition provided by the module. Consider the following example
```

 module MyModule;
 
 class MyClass {
   char32 svalue;
   enum {A=0,B=1,C=2} evalue;
   double dvalue[W];
 }
```
If the properties svalue, evalue, and dvalue are already defined as specified, the class block will load successfully. However, if there are any differences between the class block and the module's definition of the class, then the loader will attempt to address the discrepancy as follows:

1. If the class defines a property differently than the module, then the loader will fail.
2. If the class defines a property that the module does not define, then the loader will extend the module's definition of the class to include this new property.
3. If the class is not defined by any loaded module, then the class is defined as a new class, and the properties are added to that new class. In this case, you may also include C++ code for the behaviors that static modules normally provide. See below for more information on runtime classes.

## Runtime Classes

If a class block is defined, and that class is not already implemented by an existing module, then you may define class behavior for each of the supported behaviors in GridLAB-D™. Such classes are called runtime classes and are only be supported if you have installed MinGW on your system. Use of the compiler is automatic and does not need to be explicitly configured. However, if you have installed GridLAB-D™in an unusually location, you may have to set the global variable INCLUDE to indicate where the rt/gridlabd.h file is located. You may also want to add the location of any files that the #include macros are expected to find.

The supported behaviors for runtime classes include:

- `create (object <parent>) { ... return [SUCCESS|FAILED];};` is used to define the object creation behavior other than the default behavior (which is to set the entire object's memory buffer to 0). Usually parent is not defined at this point. Object creation functions are called when the object's memory is allocated, while initialization isn't performed until after all objects have been created and the parent/child hierarchy established.
- `init (object <parent>) { ... return [SUCCESS|FAILED];};` is used to define object initialization behavior, which is delayed until all objects have been created and their properties set as defined in the model file. Usually initialization is a good time to adjust dependent properties and/or check for inconsistencies in the values provided.
- `[presync|sync|postsync] (TIMESTAMP t0, TIMESTAMP t1) { TIMESTAMP t2 = TS_NEVER; ... return t2;};` is used to implement presync, sync, and postsync behavior. Presyncs are called on the first top-down pass, syncs are called on the bottom-up pass, and postsyncs are called on the last top-down pass. The top-down/bottom-up order in which objects are evaluated is based on their ranks. Rank is determined primarily by the parent-child relationship, however calls to gl_set_rank() can be used to promote the rank of an object arbitrarily with respect to another object. (Objects cannot be demoted.)
- `plc (TIMESTAMP t0, TIMESTAMP t1) {TIMESTAMP t2 = TS_NEVER; ... return t2;};` is used to define the default programmable logic controller (PLC) behavior. This behavior is overridden if a plc refers to this object as its parent.

Over time other behaviors, such as check, import, export, kmldump, etc. will be added, as needed.


For more information on the compilers for runtime classes, see the page on [Runtime compiler support](/wiki/Runtime_compiler_support).


## Exporting functions from runtime classes
You can publish a function from a runtime class by using the following syntax.
```
class example1 {
  export myfunction(void *arg1, ...)
  {
     // your code goes here
     return result; // int64 value is returned
  }
}
```
and access that function from another class using the syntax
```
class example2 {
   function example1::myfunction;
   intrinsic sync(TIMESTAMP t0, TIMESTAMP t1)
   {
      // your code goes here
      int64 result = myfunction(arg1, arg2);
      return t2;
   }
```

## Debugging Runtime Classes
To debug runtime class behavior, you must install gdb on your system. The debugger does not appear to be fully functional at this time. As more is learned about its limitations and workarounds, tip and tricks will be posted at Runtime debugging.

Windows users can debug with MS Visual Studio 2005TM. To enable this debugger the following environment variables should be set
```

// set this to your MSVC installation folder
#setenv MSVC=C:/Program Files/Microsoft Visual Studio 8

// enable use of MSVC instead of GNUtools
#define use_msvc=1 

// this disables automatic rebuild suppression of runtime classes 
// compilation based on modification time
#set force_compile=1

// customize to your local setup
#setenv path=${MSVC}/Common7/IDE;${MSVC}/VC/bin
#setenv include=${MSVC}/VC/include
#setenv lib=${SystemRoot}/system32;${MSVC}/VC/lib;${MSVC}/VC/PlatformSDK/Lib
```
You can get all these settings by simply including the debugger configuration file:
```
#include "debugger.conf"
```
You should set the debug program to the debug version of gridlabd.exe which is usually placed in the $GRIDLABD/VS2005/win32/debug folder. Once you have done this, you can set breakpoints in the C++ code block of your .glm files and the debugger will stop and offer most of the usual debugging features needed to debug your runtime class.


## Objects
Object blocks are used to define one or more instances of a class. The simplest form is a singleton object definition, such as
```
 module residential;
 object house {
   floorarea 2500 sf;
 }
```
which will define an anonymous house of 2500 square feet.

If however, you wish to define 10 such houses, then you can specify a count, such as
```
 module residential;
 object house:..10 {
   floorarea 2500 sf;
 }
```
If a functional value is used, then the functional value is evaluated separately for each instance created, so that
```
 module residential;
 object house {
   floorarea random.normal(2500,100) sf;
 }
```
defines 10 separate house, each with a different floor area, but with the floor area normally distributed about a mean of 2500 sf with a standard deviation of 100 sf.

If a mathematical expression is written within a set of parentheses, it will be processed at load time. Addition, subtraction, multiplication, division, exponential, and modulo operators are supported, as well as parenthetical nesting and unary math.h functions. The code
```
 module residential;
 object house {
   floorarea random.normal(2500,100) sf;
   ceilingheight random.normal(8,1) f;
   airvolume (this.floorarea * this.ceilingheight);
 }
```
will generate a random value for the floor area and the ceiling height of a house and calculate the contained air volume.


## Expanded values
An expression written in back quotes will be parsed in such a way that expressions in curly-braces are expanded in the context of the object being loaded. The following values are expanded

- `{file}` embeds the current file (full path,name,extension)
- `{filename}` embeds the name of the file (no path, no extension)
- `{fileext}` embeds the extension of the file (no path, no name)
- `{filepath}` embeds the path of the file (no name, no extension)
- `{line}` embeds the current line number
- `{namespace}` embeds the name of the current namespace
- `{class}` embeds the classname of the current object
- `{id}` embeds the id of the current object
- `{var}` embeds the current value of the current object's variable var

For example,
```
namespace space1 {
	object mytest {
		name `{namespace}::{class}:{id}`;
	};
}
```
will result in the object having a name like space1::mytest:0.


## Object properties
The valid properties for are determined by the class definition. This includes the units specification. When units are specified in the class, then the value is always stored in memory in those units. If the object definition provide the value with different (but compatible) units, then the value is automatically converted to the units specified by the class so that the behavior needn't consider units when doing calculation using the value. Consider the following example

```
 class house {
   double floorarea[sf];
   init (object parent) {
     printf("floor area is %f sf\n", floorarea);
   };
 }
 object house {
   floorarea 100 m^2;
 }
```
When init is executed, the floor area will be displayed in sf and not <Latex>{`$m^2$`}</Latex> because the loader automatically converts the value to the units specified by the class definition.


## Parameter expansions
String, double, set and enumeration variables can be manipulated during expansion using parameter expansions. For example
```
#define AREA=1000
class house {
  double floorarea[sf];
}
object house:..5 {
  floorarea ${AREA+=100};
}
```
will create 5 houses with values of floorarea ranging from 1000 to 1400 by increments of 100.

# Loading a model

The machine where GridLAB-D™is installed is called the host computer.  To use GridLAB-D™you have several options.  First, you can be logged directly on the host computer.  Alternatively, you can use a client application on your computer to connect to a server computer that hosts the simulation.  Finally, you can run a simple web browser that serves a web-based client application that in turn connects to the simulation either on the web-server or to a third computer that hosts the simulation.

When the GridLAB-D™is not directly available from your computer, you will need to connect to a machine where it is available.  Often, this is simply a matter of opening a telnet or ssh client to connect to the host computer.  Certain client applications may connect using other means, which will certainly be described elsewhere as appropriate.  In any case you will need to know the name of the host computer and your login id and password on that computer.  These are available from your system administrator, and you should ask him or her for help, if necessary.

Once you’ve connected to the machine that hosts GridLAB-D™, you should check to see what version of GridLAB-D™is installed:

```
host% gridlabd --version
GridLAB-D™ 2.2.0 (Four Corners)
host% 
```

The *host%* refers the command shell prompt, which indicates that you are logged in on the host computer where GridLAB-D™ is installed.  It will probably look different on your computer—on Windows machines it’s usually *C:>* and sometimes it’s simply *$* on Unix machines.

The bold *gridlabd* text is what you must type in, followed by the Enter or Return key.  Sometimes you will have to enter what are called \["/wiki/Command\_options">command options] to get GridLAB-D™ to do something different than usual.  To get a list of commonly used \["/wiki/Command\_options">command options], you can enter the following:

```
host% gridlabd –-help
Syntax: gridlabd [OPTIONS ...] <file> [<file2> ...
(...lots of helpful output...)
host%
```

To get detailed help on a particular topic, you can try the command (NOTE, "--info" is only available in versions 3.0 and newer)

```
host% gridlabd --info house
host%
```

which will open a web browser window on the page (or list of pages) related to the topic indicated.  If your topic is multiworded you should use underscores (\_) or plus signs (+) instead of spaces:

```
host% gridlabd --info implicit_enduses
host%
```

The \["/wiki/Command\_options">command options] are used to alter the mode of operation of GridLAB-D™.  The normal setting for a mode of operation is called the default, and \["/wiki/Command\_options">command options] are one way to override those defaults.  For example, GridLAB-D™ can be instructed to describing everything it is doing using the \["/wiki/Verbose">verbose] mode:

```
host% gridlabd --verbose
  ... load time: 0 sec
  ... starting up batch environment
  ... shutdown complete
  ... elapsed runtime 0 seconds
host%
```

* *Tip* - Sometimes the system administrator doesn’t properly install GridLAB-D™ and forgets to add the *gridlabd* command to the system command search path.  When this happens and you enter `gridlabd` at the command prompt, you will see some error message to the effect that the command cannot be found or is not recognized.  If this occurs, contact the system administrator and ask to have the GridLAB-D™ installation fixed so that it works from your command line.  Alternatively, you will need to provide the full path name to the GridLAB-D™ executable at the command prompt.
* *Note* - Some operating systems are not case sensitive, but GridLAB-D™ is always case sensitive.  Therefore, even though it may be possible to type *GRIDLABD* as well as *gridlabd* at the command line, the command line options may still be case sensitive.

# Creating your first model

The simplest \["/w/index.php?title=GLM\&action=edit\&redlink=1" >GLM] model you can create will contain only one object, will not process any time at all, and will record the state of only one property:

```
// Examples:1a.glm
module residential;
module tape;
object house {
  object recorder {
    property air_temperature;
    file "temperature.csv";
  };
}
```

* *Important Note* - Before \["/wiki/Hassayampa">Hassayampa (Version 3.0)] GridLAB-D™ required a clock block as described in \["/wiki/Examples:1bExample 1b] below.  If you get an error indicated that the local clock is not initialized, you will need to add the following to the beginning of this example:

```
clock {
  starttime '2000-01-01 00:00:00';
  stoptime '2001-01-01 00:00:00';
}
```

* However, the result is different because then the simulation will run for the range of time specified instead of simply giving you the state of the system at the default initial time.

This example loads the \["/wiki/Residential" >residential] \["/wiki/Module" >module] and the \["/wiki/Tape" >tape] \["/wiki/Module" >module].  It then defines a single \["/wiki/House">house] with all the default properties.  Within that house it defines a single \["/wiki/Recorder">recorder] which records the \["/wiki/Air\_temperature">air\_temperature] property of the \["/wiki/House">house] and stores that value in the \["/w/index.php?title=File\&action=edit\&redlink=1" >file] <i>temperature.csv</i>.

* *If you’ve just installed GridLAB-D*\
  You may need to set up the system’s runtime environment.  There are several files that you may have to adjust to accommodate the compilers and runtime libraries that are appropriate to your system.  These should have been set automatically, but if you are experiencing trouble with the runtime environment, see the \["/wiki/Installation\_Guide">Installation Guide] for details on how to make GridLAB-D™ work with your system.

* *If you’re a system administrator*\
  If there is more than one user for GridLAB-D™, you will probably have to set up the multiuser runtime support.  You can skip this step, but this will require all users to have the same runtime environment, which may be undesirable in certain cases. When there are multiple users, it is quite likely they will need to have the ability to customize their individual GridLAB-D™ configurations.  This is automatically supported by the `gridlabd.conf` runtime file, but several considerations must be observed when operating GridLAB-D™ this way.

* By default, `gridlabd.conf`is located using the `GLPATH` environment variable and the search is always done by first looking in the current folder.  If any users have a copy of this file in their search path or in the current directory, then they will not be consulting the host system’s standard configuration file.  This can result in unpredictable and erratic behavior for different users.

* Users may create a file `gridlabd-<i>user</i>.conf`, where `<i>user</i>` is the user’s login name as specified by the `USER` environment variable.  This file will be located and consulted when found.  You may opt to create a static one for each user in the GridLAB-D™ directory that the user cannot edit. Or you can create configuration files for each user in directories they own and can edit.  In the latter case you should be certain that their private directories are included in the `GLPATH` environment.  On Linux systems, this can be done by adding\
  \*`#["/wiki/Setenv">setenv] GLPATH=${GLPATH}:${HOME}/gridlabd`

* to the `gridlabd.conf` file.  In Windows, the correct syntax uses a semicolon instead of a colon:\
  \*`#["/wiki/Setenv">setenv] GLPATH=${GLPATH};${HOME}/gridlabd`

# Running your model

Using an editor, type this code into the file <i>house1.glm</i>.  Then run the following command:

```
host% gridlabd house1.glm
host%
```

* *Note*\
  you may some warnings about the clock and voltages, but you can ignore those for now.

Now look at the output file:

```
host% more temperature.csv
1970-01-01 00:02:00 UTC,+69
```

As you can see, the temperature was read once at (actually shortly after) the default initial simulation time.

If you don’t get the expected result, make sure your GridLAB-D™ system is properly installed and make sure you didn’t make any typographic errors if you entered the code by hand.

On most systems, you will see extra information about the recording output before the data.  Each piece of information is preceded by a ‘#’ character to indicate that it is not part of the recording itself but simply meta-information to describe the recording.

## GLM files

Let’s take a quick tour of the *house1.glm* model file to understand how this was result was obtained.

The three commands in this file are called directives.  \["/wiki/Directives">Directives] are either simple one-liners like the \["/wiki/Module" >module] directives, or complex multi-line ones like the \["/wiki/Object">object] directives.

### Loading modules

The first two directives in *house1.glm* instructs GridLAB-D™to load modules. Modules define many classes in a single quick and convenient directive.  Modules are not like class definitions that you give as a directive because they are not compiled when GridLAB-D™is run.  Instead, developers package a collection of classes and distribute a dynamic link library (as a `.dll` file in Windows or `.so` file in Linux).  The process for building and disseminated a module is described in later chapters.

The directives given in *house1.glm* load the \["/wiki/Residential" >residential] and \["/wiki/Tape" >tape] modules, which define classes for houses and input/output data processing.  In this case, we will be using a \["/wiki/House">house] object in the residential module and data \["/wiki/Recorder">recorder] object in the tape module.

### Creating new objects

The third and last directive in *house1.glm* is an object definition.  Object directives must provide that class type so that GridLAB-D™ knows what kind of information you’d like to associate with object.  An object definition usually includes property values (see below), but sometimes it can also include nested objects.

The third entry includes a nested object.  This nested object is the recorder.  By nesting the recorder, it is associated with the house.  This association is called a parent-child relationship.  In this case, the house is the parent object and the recorder is the child object.  Parent-child relationships are extremely important in GridLAB-D™ because they determine a hidden property of objects called the \["/w/index.php?title=Rank\&action=edit\&redlink=1" >rank].  Rank is used to establish things like the order in which time synchronization behaviors are run and whether they can be run simultaneously. This is critical to getting the correct results and enabling fast simulations running on high-performance computers.

### Recording information about an object

The recorder class is one of two classes in the tape module that can gather information about objects as they change over time.  Recorders can output data in a variety of formats, and the properties of a recorder object what is gathered, how it is gathered, and where it is delivered to.

The recorder object gathers information from its parent object, in this case the \["/wiki/House">house] object.  By specifying `["/wiki/Property" >property] ["/wiki/Air_temperature">air_temperature];`, the recorder object is instructed to gather only the value of the \["/wiki/Air\_temperature">air\_temperature] property of the house object.  The \["/w/index.php?title=File\&action=edit\&redlink=1" >file] property instructs the recorder to deliver the observed values of side to a file called `temperature.csv`.  Normally, a recorder object only delivers an observation to the file when the property changes, which is what we want in this case.

The output of a recorder object is normally in the form of comma-separate value file (CSV).  This is the format that many programs and applications use to exchange simple tables of column-row oriented data.  GridLAB-D™will always outputs the date and time of the observation (in simulation time), followed by the observed value(s).  Each observation event is placed in a separate row, so that they file is a complete time-series record of the changes in the observed value as the simulation advanced the clock.

# Improving your model

The follow section describes various ways that you can use to extend or improve your models.  These include how to use date/time, how to disable and enable objects, referencing other objects, generating populations of objects, embedding context information in objects, using functional properties, performing calculation while loading and running models, and handling units.

## Using date and time

Date and time specifications are used for a lot of things in GridLAB-D™.  Most importantly they are used to specify the starting and ending time of the simulation:

```
// Examples:1b.glm
clock {
  starttime '2000-01-01 00:00:00 UTC';
  stoptime '2001-01-01 00:00:00 UTC';
}
module residential;
module tape;
object house {
  object recorder {
    property air_temperature;
    file temperature.csv;
  };
}
```

which produces the following output:

```
2000-01-01 00:00:00 UTC,+69
2000-01-01 01:00:00 UTC,+69.9947
2000-01-01 02:00:00 UTC,+70.6269
2000-01-01 03:00:00 UTC,+71.1764
2000-01-01 04:00:00 UTC,+71.6754
2000-01-01 05:00:00 UTC,+72.1189
2000-01-01 06:00:00 UTC,+72.5266
2000-01-01 07:00:00 UTC,+72.927
...
```

Dates and times are usually specified using the ISO (International Standard Organization) standard format, which is "YYYY-MM-DD HH:MM:SS ZZZ".   (You can change the format of date/time values using the \["/wiki/Dateformat">dateformat] \["/wiki/Global\_variable" >global variable].)

If you omit the time zone specification, then the time zone indicated by the \["/wiki/TZ">TZ] environment variable will be used.  If you wish to specify the timezone to use in the simulation, use the \["/wiki/Timezone">timezone] directive:

```
// Examples:1c.glm
clock {
  timezone EST+5EDT;
  starttime '2000-01-01 00:00:00 UTC';
  stoptime '2001-01-01 00:00:00 UTC';
}
module residential;
module tape;
object house {
  object recorder {
    property air_temperature;
    file temperature.csv;
  };
}
```

which results in the following output:

```
2000-01-01 00:00:00 EST,+69
2000-01-01 01:00:00 EST,+69.9947
2000-01-01 02:00:00 EST,+70.6269
2000-01-01 03:00:00 EST,+71.1764
2000-01-01 04:00:00 EST,+71.6754
2000-01-01 05:00:00 EST,+72.1189
2000-01-01 06:00:00 EST,+72.5266
2000-01-01 07:00:00 EST,+72.927
2000-01-01 08:00:00 EST,+73.3376
2000-01-01 09:00:00 EST,+73.7618
```

Time zones are specified in the time zone file `tzinfo.txt` file that is installed with the system in the gridlabd folder. GridLAB-D™ does not use the operating system’s time zone specifications for several reasons.  First, some operating systems don’t recognize time zone that are historically relevant but no longer used.  Second, the simulation often needs to run the simulation in a different time zone than that used by the host computer. Finally, the ability to use alternate time zone rules is essential to understanding the energy use implication of altering the time zone rules, something which policymakers have an interest in and sometimes ask.  GridLAB-D™ can address these questions only if the rules for the simulation are different from the rules on the computer on which the simulation is running.

## Disabling and enabling objects

It is possible to have an object activate at a pre-determined date and time, and have deactivate to exist at some time later.  Each object has a pair of built-in properties called \["/wiki/Object\_(property)#in">in] and \["/wiki/Object\_(property)#out">out] that determine when the object enters service and when to go out of service.

In the following example, we modify *house1.glm* to illustrate how the \["/wiki/Object\_(property)#in">in] and \["/wiki/Object\_(property)#out">out] properties function.

```
// Examples:1d.glm
clock {
  timezone EST+5EDT;
  starttime '2000-01-01 00:00:00';
  stoptime '2001-01-01 00:00:00';
}
module residential;
module tape;
object house {
  object recorder {
    property air_temperature;
    file temperature.csv;
    in '2000-04-01 00:00:00';
    out '2000-04-02 00:00:00';
  };
}
```

Running *house1.glm* give the following result:

```
2000-04-01 00:00:00 EST,+76.0806
2000-04-01 00:05:02 EST,+73.9952
2000-04-01 01:00:00 EST,+75.6225
...
2000-04-01 22:00:00 EST,+75.5875
2000-04-01 23:00:00 EST,+75.8156
2000-04-02 00:00:00 EST,+75.9208
```

The first major difference is that although the simulation started on January 1, 2000 at midnight, the data was collected starting April 1, 2000 as specified by the \["/wiki/Object\_(property)#in">in] property of the recorder.  The second difference is that the recorder stopped collecting data after midnight of the April 2, 2000 as specified by the \["/wiki/Object\_(property)#out">out] property of the recorder.

## Referencing other objects

In the above examples, the recorder object is nested within the house object.  This is a convenient way to indicate that the recorder depends on the house object for the property it records. This implied relationship is the parent-child relation.

Another way to specify such a relationship is the give the house a name and reference it from the recorder, as in

```
// Examples:1e.glm
clock {
  timezone EST+5EDT;
  starttime '2000-01-01 00:00:00';
  stoptime '2001-01-01 00:00:00';
}
module residential;
module tape;
object house {
  name MyHouse;
}
object recorder {
  parent MyHouse;
  property air_temperature;
  file temperature.csv;
  in '2000-04-01 00:00:00';
  out '2000-04-02 00:00:00';
};
}
```

In this case, the recorder is explicitly referring to the house as its parent by name.  It is using this mechanism that you can link objects together across multiple model files.

The parent relationship has an impact on how behaviors like initialization and time synchronization are performed. So often it’s not appropriate to use it.  Some objects reference others objects without using the parent relationship. This is done using an explicitly declared property of object of the object:

```
// Examples:1f.glm  **TODO**:  add necessary properties so this actually loads ok
module powerflow;
class link {
  object from;
  object to;
}
object node {
  name Node1;
}
object node {
  name Node2;
}
object link {
  from Node1;
  to Node1;
}
```

## Generating populations

One of the most important capabilities in GridLAB-D™is the ability to study large populations of objects as they interact with each other.  The challenge for modelers is quickly defining what that population looks like without having to explicitly define each and every object.  This is made easy by using a multi-object definition, such as

```
object house:..5 {
  floor_area 2500 sf;
};
```

In this case, GridLAB-D™will create 5 identical objects and will all run together.

## Embedding context information

Object populations can create complications for other objects, such as recorders.  Because the recorder is nested in the house definition, there are in fact 5 recorders created all of which write to the same file.  This will clearly not have the desired outcome.  To remedy the problem, context information can be embedded in the name of the output file, such as is shown with the bolded changes:

```
// Examples:1g.glm
clock {
  timezone EST+5EDT;
  starttime '2000-01-01 00:00:00';
  stoptime '2001-01-01 00:00:00';
}
module residential;
module tape;
object house:..5 {
  object recorder {
    property air_temperature;
    file `temperature{id}.csv`;
    in '2000-04-01 00:00:00';
    out '2000-04-02 00:00:00';
  };
}
```

The result is that the a different recorder file is used for each recorder.  The recorders' internal \["/wiki/Object\_(property)#id">id] is used for each file name thus allowing a separate file for each. You can enable embedded information from the context of an object by surrounding the value in back-quotes.  When a value is defined this way, any part of the value which is in curly-braces is expanded from the context of the object. See \["/wiki/Expansion\_variables">Expansion variables] for details.

## Functional Values

Sometimes it is necessary to examine the behavior of multiple objects that are not quite the same. When we want to create different objects with different properties we use a functional property, such as

```
object house:..5 {
  floor_area random.uniform(1500,2500);
};
```

Functionals are values that are determined when the object is created.  There are a number of supported functionals, most of which are random number generators of different types.  In this case, the floor area property will be randomly sampled 5 times from uniform distribution of values between 1500 and 2500. See \["/wiki/Functional\_values">Functional values] for details.

## Performing calculations

Often objects have values that are correlated with each other, rather than independently distributed.  For example, by default the air volume of a house ceiling height times the floor area.  However, if you want to explicitly calculate a different volume based on floor area you can use an equation, as shown in the following example:

```
object house:..5 {
  floor_area random.uniform(1500,2500);
  air_volume (10*$floor_area);
};
```

Calculated values are always enclosed in parenthese and are an important way to established correlated relationship when large populations of objects are defined. See \["/wiki/Property\_calculations">Property calculations] for details.

## Handling units

Every real valued or complex valued property should have \["/wiki/Units">units] associated with it.  All recognized units are defined in the unit file `unitfile.txt`.  For example, the house’s \["/wiki/Floor\_area">floor area] property could be defined as

```
class house {
  double floor_area[sf];
}
```

which indicates that the property has a square-foot unit.  When a property has a unit associated with it, any use of the property can include a compatible unit, and when it does, the value given will be converted automatically.  For example,

```
object house {
  floor_area 125 m^2;
}
```

would convert the value from 125 \["/wiki/Units">m^2] to square-feet when loading the model.

Unit conversion is performed automatically, but only when the units are compatible, meaning that they represent the same underlying fundamental quantity, but with different scales.  This means that GridLAB-D™ will automatically convert \["/wiki/Units">ft] to \["/wiki/Units">m], or \["/wiki/Units">ft^2] or \["/wiki/Units">sf] to \["/wiki/Units">m^2], but it cannot convert \["/wiki/Units">ft] to \["/wiki/Units">cm^2] because they are fundamentally incompatible.

When a unit is declared in class’s property, but not specified in object’s definition, it is assumed that the value defined uses the declared unit.  Therefore `floorarea 2500;` is the same as `floor_area 2500 sf;`. The only difference that if somebody were to come along and change the default unit, then the latter definition would continue to work as expected, while the former would behave differently.

* *Note*\
  Using units is a good way to prevent accidental changes in behavior, to ensure that your models always behave exactly as expected, and to let modelers who use your code later know what you intended.
* *Tip*\
  The unit file does not define every conceivable unit, but the unit parser understands compound units and ISO scales—even though \["/wiki/Units">kJ/h] is not listed, it can be parsed and converted to any units that is fundamentally compatible with it (e.g., \["/wiki/Units">W], \["/wiki/Units">kW], \["/wiki/Units">Btu/h]).

## Providing input

GridLAB-D™ provides a number of methods for inputting data into the system. ***TODO***

### Players

The first is part of the \["/wiki/Tape" >Tape] module, and is used to represent irregular time series data.  The \["/wiki/Player">Player] object provides the ability to update a single object variable at specified times. The values are read from a file formatted like comma-separated value (CSV) files or other sources. The source data must have \["/wiki/Timestamp">Timestamp] (or time changes) in the first column, and the values to be posted in the second column.  The \["/wiki/Player">player] must be "childed" to the object that you want to play information into.

In this example, the \["/wiki/Player">Player] is a child of the \["/wiki/House">house] object, and will play a stream of values from the CSV file "t\_cool" into the property \["/wiki/House#cooling\_setpoint">cooling\_setpoint] of the \["/wiki/House">house] object:

```
object house {
  object player {
    property cooling_setpoint;
    file t_cool.csv;
  };
}
```

The actual CSV file is a two-column format with a time value (either an absolute \["/wiki/Timestamp">Timestamp] or relative time).  An absolute timestamp version may look like:

```
2007-01-02 00:00:00, 72.0
2007-01-02 01:00:00, 73.0
2007-01-02 02:00:00, 72.0
2007-01-02 03:00:00, 68.0
2007-01-02 15:00:00, 69.0
2007-01-02 23:00:00, 72.0
```

Note the times can be completely irregular. A second method, using relative time would look like:

```
2007-01-01 23:00:00, 72.0
+1h, 72.0
+1h, 73.0
+1h, 72.0
+1h, 68.0
+12h, 69.0
+8h, 72.0
```

Note that the first time is absolute, but relative timestamps define the same schedule by using differentials in time.  This is useful in conjunction with the \["/wiki/Player#loop">loop] property in \["/wiki/Player">Player]:

```
object house {
  object player {
    property cooling_setpoint;
    file t_cool_relative.csv;
    loop 31;
  };
}
```

Where the relative times (summing to 24 hours) will be repeated 31 times in a cyclic manner.  When the allotted time has expired (i.e., the clock time runs beyond the time designated within the player file), the final value will be used indefinitely.

Schedules

***TODO***\
Transforms

***TODO***\
Loadshapes

***TODO***\
Links

***TODO***

### Generating output

GridLAB-D™ provides a number of ways to generate output from the simulation.  The basic concept is that of a data logger.  The recorder is hooked up to an object property and makes a copy of the value every so often and writes it to a file for you.  What object the recorder looks at and how often it writes is determined by how you set up the recorder.  For example

```
object recorder {
  parent MyHouse;
  property air_temperature;
  file "temperature.csv";
};
```

instructs the recorder to observe the `["/wiki/Air_temperature">air_temperature]` property of the `MyHouse` object and send any changed observations the file `temperature.csv`.

There are additional settings that you can provide to control the behavior of the recorder. To limit the length of the recording, for example:

```
object recorder {
  parent MyHouse;
  property air_temperature;
  file "temperature.csv";
  limit 1000;
};
```

prevent the recorder from making more than 1000 observations.

To change the sampling interval, for example:

```
  object recorder {
  parent MyHouse;
  property air_temperature;
  file "temperature.csv";
  limit 1000;
  interval 300;
};
```

additionally instructs the recorder to look at the internal air temperature of the house every 5 minutes.

* *Tip* - 
  Setting the interval to 0 causes the recorder to sample the value every internal iteration of the solver, which can help you debug modeling problems when the solvers do not converge.

# Summary

We have seen some of the basic features of GridLAB-D™ and how you can use it to model a simple system.  At this point you are ready to explore the various \["/wiki/Modules">modules] in GridLAB-D™ and get familiar with the classes and properties they implement as described in the \["/wiki/Beginner%27s\_Guide\_to\_GridLAB-D">Beginner's Guide to GridLAB-D] page.

# References

* \[1] RT Guttromson, DP Chassin, SE Widergren, "Residential energy resource models for distribution feeder simulation", IEEE PES GM, 2003, DOI: \["[http://dx.doi.org/10.1109/PES.2003.1267145">http://dx.doi.org/10.1109/PES.2003.1267145](http://dx.doi.org/10.1109/PES.2003.1267145">http://dx.doi.org/10.1109/PES.2003.1267145)]
# Mystery Intro to Modeling
As an example, open up [/Tutorial/Chapter 2 - Key Attributes/Sample Model/sample\_model.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%202%20-%20Key%20Attributes/Sample%20Model/sample_modelcourse) with a text editor. At the top of this model file (or simply "model") you'll see a number of module declarations such as

```
module tape;
module generators;

module powerflow{
  solver_method FBS;
  default_maximum_voltage_error 1e-9;
  line_limits FALSE;
};

module climate;

module residential {
  implicit_enduses NONE;
  ANSI_voltage_check FALSE;
};

module powerflow {
    solver_method FBS;
    NR_iteration_limit 100;
};
```

Each of these `module` statements links in a portion of the code base that enables specific functionality in this particular GridLAB-D™ model.

Looking further down the model you'll see that most of the statement groups begin with `object` such as

```
object node {     
      name R1-12-47-1_node_613;     
      parent R1-12-47-1_meter_21;     
      phases ABCN;     
      voltage_A 7216.88+0.0j;     
      voltage_B -3608.44-6250j;     
      voltage_C -3608.44+6250j;     
      nominal_voltage 7216.88;     
} 
.
.
.
object overhead_line {     
      groupid Distribution_Line;
      name R1-12-47-1_ol_299;     
      phases ABCN;     
      from R1-12-47-1_node_4;     
      to R1-12-47-1_node_25;     
      length 487.200;     
      configuration line_configuration_22;     
} 
.
.
.
object house {
  name myHouse;
  parent tpm2_R1-12-47-1_tm_21;
  floor_area random.normal(1750,400);
  heating_setpoint 70;
  cooling_setpoint 78;
  air_temperature 79;
}
```

Each one of these objects is a specific instance of a particular class (`node`, `overhead_line`, `house`). As a part of each class are a set of algorithms and equations that define how all objects of this class should behave. For example, the code in the `overhead_line` class will be used to define how much of a voltage drop should occur as a given amount of current flows through the line.

There can (and often will be) multiple instances of a class in a given model (model file, ".glm") though each one will likely be unique due to the specific values associated with each parameter in the object definitions. For example, look at two particular `overhead_line` objects in the model:

```
  object overhead_line {     
      groupid Distribution_Line;
      name R1-12-47-1_ol_298;     
      phases ABCN;     
      from R1-12-47-1_node_3;     
      to R1-12-47-1_node_21;     
      length 167.609;     
      configuration line_configuration_22;     
} 
.
.
.
object overhead_line {     
      groupid Distribution_Line;
      name R1-12-47-1_ol_290;     
      phases ABCN;     
      from R1-12-47-1_node_11;     
      to R1-12-47-1_node_12;     
      length 283.154;     
      configuration line_configuration_22;     
} 
```

These are both `overhead_line` objects and though they use the same equations to define their operation in the simulation, you can see that each has different lengths (among other differences). Because of this difference, the equations of the `overhead_line` class will produce a different voltage drop across each `overhead_line` instance, that is, each of the objects instantiated from that class.

## Multi-Domain Models

GridLAB-D™ is commonly represented as a distribution system and/or smart grid simulator with incredibly useful built-in classes that are more than what you might find in a traditional distribution system simulator. GridLAB-D™contains classes for all may common distribution components (transformers, lines, voltage regulators, capacitors, ZIP loads,...) it also contains classes for solar PV, energy storage, residences with air-conditioners and water heaters, and weather. Though each of these has impacts on the operation of the distribution system, it is their behavior outside the electrical system as it is modeled by GridLAB-D™that is most compelling. A house with an air-conditioner is modeled to not only represent how much energy the air-conditioner uses when it is running but also how often it runs and for how long based the indoor temperature which in turn is driven by the thermodynamic properties of the house (how large it is, how much insulation, how many windows...) and the weather. These multi-domain interactions provide a capability not often seen in other distribution system simulators.

While GridLAB-D™is commonly used to simulate distribution systems and smart grids in particular but there is no reason that it needs to be limited to this in scope. It is entirely possible to use the existing GridLAB-D™framework to simulate vegetation growth, weather, or vehicle traffic patterns while simultaneously simulating an electrical distribution system. Given that very few, if any, of the GridLAB-D™classes would be usable to add these features, a lot of coding would be required to define a new classes with the appropriate equations, algorithms, and parameter declarations so that, say, the grass and trees in the simulation behaved in a realistic way.

## Discrete Time

GridLAB-D™ is a discrete time simulator meaning it does not simulate a continuous flow of time as you might see in something like a SPICE circuit simulator or Simulink model. Truth be told, these simulators are not actually continuous but the time steps used by the simulator are small enough and regularly spaced so as to closely and reasonably approximate continuous time. GridLAB-D™makes no such effort and operates in a fundamentally different way.

GridLAB-D™ simulates time in discrete blocks that can vary in size; that is, the step size of the simulation is constant. The GridLAB-D™ core jumps from one discrete time to the next, asking all objects in the model (“.glm”) to update themselves based on how much time has elapsed since the last update. The algorithms and equations for each class define how objects of that class evolve over time and using these relationships, all of the objects can determine their new state at this new point in time.

Since many of the objects in a model interact with each other, having outputs that affect other objects’ inputs, the GridLAB-D™ core may re-iterate a time step asking all the models to re-update themselves using any new changes to their inputs to recalculate their state. This process may repeat many times until all of the objects have reached a new steady-state.

As a last step in the update process, all of the objects register with GridLAB-D™the next time they need to be updated. Since by this point all the objects have reached a steady-state, most of them will indicate that they never need to be updated. Unless there is some external inciting event, their state will not change. There are only a few special objects that end up driving the simulation forward by requiring they be updated at particular times.

As an example, open up [Tutorial/Chapter 2 - Key Attributes/Discrete Time/discrete\_time\_demo.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%202%20-%20Key%20Attributes/Discrete%20Time/discrete_time_democourse/) with a text editor.  Even not knowing any of the details of GridLAB-D™, you can see this is a very small model. With not too much effort, you could probably guess that this model will run from July 1st, 2001 to July 8th, 2001 (lines 10 and 11) and uses the weather from Spokane, WA (lines 14-16). You can also see on line 21 that a house is included in the model with a number of specific parameters defined.

Also attached to the house is an object called a recorder (lines 34-38). This object is one of the primary means of collecting data from a simulation and it is one of those special objects which regularly tells the GridLAB-D™core when it next needs to be updated. One of the parameters of the recorder is its `interval` which is the number of seconds between data collection times. The recorder interval is currently 3600 seconds, indicating it will log a value once an hour. Running this simulation with the profiler turned on...

```
gridlabd discrete_time_demo.glm --profile 
```

...shows the following...

```
Core profiler results
======================

Total objects                  4 objects
Parallelism                    1 thread
Total time                   1.0 seconds
  Core time                  0.9 seconds (86.1%)
    Compiler                 0.5 seconds (47.8%)
    Instances                0.0 seconds (0.0%)
    Random variables         0.0 seconds (0.0%)
    Schedules                0.0 seconds (0.1%)
    Loadshapes               0.0 seconds (0.0%)
    Enduses                  0.0 seconds (0.0%)
    Transforms               0.0 seconds (0.0%)
  Model time                 0.1 seconds/thread (13.9%)
Simulation time                7 days
Simulation speed             672 object.hours/second
Passes completed             552 passes
Time steps completed         552 timesteps
Convergence efficiency      1.00 passes/timestep
Read lock contention        0.0%
Write lock contention       0.0%
Average timestep           1096 seconds/timestep
Simulation rate          604800 x realtime


Model profiler results
======================

Class            Time (s) Time (%) msec/obj
---------------- -------- -------- --------
climate            0.128     92.1%    128.0
house              0.005      3.6%      5.0
recorder           0.003      2.2%      3.0
triplex_meter      0.003      2.2%      3.0
================ ======== ======== ========
Total              0.139    100.0%     34.8
```

(Your specific values will likely be different depending on the computer used for running these simulation.) For our purposes, note the total simulation time in the lower table.

Now go back to the model file, change the `interval` to `6` and re-run the simulation.

```
Core profiler results
======================

Total objects                  4 objects
Parallelism                    1 thread
Total time                   4.0 seconds
  Core time                  2.2 seconds (55.8%)
    Compiler                 0.5 seconds (12.2%)
    Instances                0.0 seconds (0.0%)
    Random variables         0.0 seconds (0.0%)
    Schedules                0.0 seconds (0.0%)
    Loadshapes               0.0 seconds (0.0%)
    Enduses                  0.0 seconds (0.3%)
    Transforms               0.0 seconds (0.5%)
  Model time                 1.8 seconds/thread (44.2%)
Simulation time                7 days
Simulation speed             168 object.hours/second
Passes completed          101355 passes
Time steps completed      101355 timesteps
Convergence efficiency      1.00 passes/timestep
Read lock contention        0.0%
Write lock contention       0.0%
Average timestep              6 seconds/timestep
Simulation rate          151200 x realtime


Model profiler results
======================

Class            Time (s) Time (%) msec/obj
---------------- -------- -------- --------
house              0.586     33.1%    586.0
triplex_meter      0.542     30.6%    542.0
recorder           0.325     18.4%    325.0
climate            0.317     17.9%    317.0
================ ======== ======== ========
Total              1.770    100.0%    442.5
```

Note the difference in total simulation time; it should be significant. By asking the recorder to collect data every 6 seconds instead of every 3600 seconds, we have forced GridLAB-D™to update all the objects much more often which requires significantly more calculation. Comparing the entries in the lower table individually, you can see that when we updated every 3600 seconds, the vast majority of the simulation time was spent in the climate class, updating the weather being experienced by the house. When we move the time step down to 6 seconds, now the calculations of the house are largest, followed closely by the power flow of the meter attached to the house.

This example should make it clear that when we change the interval on the recorder, we are not simply changing which results are written out to a file, as if the results were calculated at a very fine time step and sampled as needed by the recorder. By changing the interval on the recorder, we are changing how often the simulation stops to recalculate the state of the system. For many simulations, the effective timestep of the simulation ends up being defined by the recorder, due to its special status in driving the simulation forward.


---
What are we doing here?

Who is a “modeler” and what this guide intends to provide?

Reference and on-boarding for new modelers (people who write .glms)

Class hierarchy – In GLD, there is a class hierarchy which affects that parameter list for an object. This makes it good to know which objects are sub-classes of other objects. For example, there is a “node” object with, say, a “voltage\_A” property. The “meter” object is a subclass of “node” which means, even though “voltage\_A” won’t show up in its property list, “voltage\_A” is a valid property.

Device models – For each device the following

* Specification page – All the math; this may be something that we try to write into the source code and extract to build the webpage or we write the webpage (via Markdown) and then reference it in the code. We don’t want to have to describe these equations twice (once as code comments and once in the documentation).
* User page – Parameter list for the object and what each parameter does. Again, may be pulled from the existing source code documentation and created as a webpage; we just don’t want to write things down twice. This is likely to be the more popular page as it is what you need when you’re trying to put a .glm together.

Support objects

* Recorders
  * Output formats
  * Connecting to databases (if this is still a supported feature)
* Players
* Schedules
* Timing (assuming it exists and replaces “starttime” “min\_timeste”, etc)
* Message (may not be needed if we have libgld?)

Other objects

* Weather
* Market

Verifying/debugging a model

* What are the red flags?
* Common warnings you might be able to avoid
* Running validation scripts – Something that post-processes results and looks for modeling abnormalities

----------------