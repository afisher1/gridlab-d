# Class

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Class
class \- Class definition directives 

## Synopsis
    
    
    class _class-name_ {
      [public] _[GLM-type]_ _property-name_[_[Units]_];
      protected _[GLM-type]_ _reference-name_[_[Units]_];
      private _C-type_ _private-name_ ;
      intrinsic create (object _parent_) {
        // ...
        return SUCCESS; // or FAILED
      };
      intrinsic init (object _parent_) {
        // ...
        return SUCCESS; // or FAILED
      };
      intrinsic isa (char* _class-name_) {
        // [Template:NEW30] ...
        return true; // or false
      };
      intrinsic precommit (void) {
        // [Template:NEW30] ...
      };
      intrinsic presync ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {
        // ...
      };
      intrinsic sync ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {
        // ...
      };
      intrinsic postsync ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {
        // ...
      };
      intrinsic plc ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {
        // ...
      };
      intrinsic commit ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {
        // [Template:NEW30] ...
      };
      intrinsic notify (char* _property-name_ , int _event_ , void* _value_) {
        // [Template:NEW30] ...
      };
      function _function-name_ (_call arguments..._) {
        // code
      };   
    }
    

## Description

In GridLAB-D, a class is a named structure associated with a particular module or a [runtime] [GLM] file that is registered with the GridLAB-D core. A class can be defined or extended within an [input file], publish [properties], and export callback functions. At minimum, a class must define how it is constructed, and what it does during each synchronization time step. 

The [class directive] can be used to alter the structure of an existing class. 

### commit

Prior to [Hassayampa (Version 3.0)]

`intrinsic commit (void) {...}`
    The [commit] function is run after all [synchronization] passes have been completed. The commit function must return [SUCCESS] or [FAILED] to indicate the result. The simulation will stop if the return values is not [SUCCESS].

As of [Hassayampa (Version 3.0)]

`intrinsic commit ([TIMESTAMP] _from_ , [TIMESTAMP] _to_) {...}`
    The [commit] function is run after all [synchronization] passes have been completed. The commit function must return a [TIMESTAMP] later than _to_ (or TS_NEVER) to indicate the success. The simulation will stop if the return values is not later than _to_.

### create

`intrinsic create (object _parent_) {...}`
    The [create] function is run before user-defined values are loaded and [init] is performed. This event allows you to set up each object prior to user values being set by the GLM file. This is the time to set default values and values used to detect whether the user has defined required values (e.g., negative or zero values that are not valid). The [create] function must return [SUCCESS] or [FAILED] to indicate the result. The simulation will stop if the return values is not [SUCCESS].

### init

**TODO**: 

### isa

**TODO**: 

### notify

**TODO**: 

### plc

**TODO**: 

### postsync

**TODO**: 

### precommit

**TODO**: 

### presync

**TODO**: 

### sync

**TODO**: 

## Caveat

You should not define classes that are already defined in modules. There's nothing to prevent users from doing this, but the behavior of GridLAB-D under such conditions is not defined. 

Windows users must have [MinGW] installed on their system for the class directive to load properly for anything other than adding public properties. 

# Examples

Verifying class structures

To verify the structure of an existing class use the syntax: 
    
    
    module climate;
    class climate {
      double [temperature][[degF]$]$;
    }
    

If the variable [temperature] is defined but is not a double with [units] [degF], then the load of the GLM file will fail. This allows modelers to be certain that the variable and units remain as expected when a module is loaded. 

Altering class structures

To alter the structure of an existing class by adding new variables, use the syntax: 
    
    
    module climate;
    class climate {
      double elevation[[ft]];
    }
    

If the variable `elevation` is not defined, then it will be added with the [units] specified. 

Creating new classes

To create a new [runtime class] with a double called `elevation` measured in [[ft]] use the syntax: 
    
    
    class my_class {
       double elevation[[ft]];
    }
    

**TODO**:  Add an example of a full-fledged class with runtime components. 

## See also

  * [Built-in types]
  * [Directives]
  * [MinGW]
  * [Module (directive)]
  * [Runtime classes]
  * [Units]



# Class functions

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Class_functions
## Contents

  * 1 Synopsis
  * 2 Required functions
    * 2.1 constructor
    * 2.2 create
    * 2.3 loadmethod
    * 2.4 init
  * 3 Optional functions
    * 3.1 precommit
    * 3.2 sync
    * 3.3 commit
    * 3.4 finalize
    * 3.5 notify
    * 3.6 notify__property_
    * 3.7 isa
    * 3.8 plc
  * 4 User-defined functions
  * 5 See also
Class functions \- Required and optional class export functions 

## Synopsis

Required functions
    
    
    
    EXPORT_CREATE(_class_);
    EXPORT_LOADMETHOD(_class_ ,_method_);  PROPOSED FOR 3.1  See [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797)
    EXPORT_INIT(_class_);
    

Optional functions
    
    
    
    EXPORT_PRECOMMIT(_class_);
    EXPORT_SYNC(_class_);
    EXPORT_COMMIT(_class_);
    EXPORT_FINALIZE(_class_);
    EXPORT_NOTIFY(_class_);
    EXPORT_NOTIFY_PROP(_class_ ,_property_);
    EXPORT_ISA(_class_);
    EXPORT_PLC(_class_);
    

User defined functions
    
    
    
    EXPORT int64 (*call-address)(OBJECT *_obj_ , ...);
    gl_publish_function(CLASS *_class_ , const char *_function_name_ , FUNCTIONADDR _call-address_);
    

## Required functions

### constructor

The constructor for a class is actually only called once to register the class with the core. Usually the _new_ call is done in the **init.cpp** right after the global variables are declared, i.e., 
    
    
    new _myclass_(module);
    

The constructor template is as follows 
    
    
    _myclass_ ::_myclass_(MODULE *module)
    {
       if ( oclass==NULL )
       {
          oclass = gld_class::create(module,"_myclass_ ", sizeof(_myclass_), _options_);
          if ( oclass==NULL ) throw "unable to construct _myclass_ ";
          else oclass->trl = TRL_UNKNOWN;
          defaults = this;
          if ( gl_publish_variable(oclass,
            // **TODO** add variables to publish here
            NULL)<1 ) throw "unable to publish _myclass_ variables";
          memset(this,0,sizeof(_myclass_)); // this should not be done if virtual functions are used
       }
    }
    

### create

The create function is called whenever an object for a class is instantiated. The create process is typically used to construct the object in memory and set the pre-initialization default values, if any. Every GridLAB-D class must export the create function and define it in that class as follows: 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      int create(void);
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_CREATE(_class-name_);
    int _class_ ::create(void)
    {
       memcpy(this,defaults,sizeof(*this));
       // **TODO** add other pre-initialization defaults here
       return 1; // return 0 on failure
    }
    

### loadmethod

PROPOSED FOR 3.1  See [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797)

The loadmethod functions are called whenever the main GLM loader encounters a registered term while parsing a GLM file. 
    
    
    [[#loadmethod|EXPORT_LOADMETHOD(_class_ ,_method_);
    int _class_ ::_method_(char* _value_)
    {
       // parse _value_
       // return 0 on failure, 1 on success
       return 1;
    }
    

### init

The init function is called whenever an object is initialized. The initialization process is typically used to setup the object in memory and adjust default values, if any. Every GridLAB-D class must export the init function and define it in that class as follows: 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      int init(void);
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_INIT(_class-name_);
    int _class_ ::init(void)
    {
       // **TODO** add initialization process here
       return 1; // return 0 on failure, return 2 to defer
    }
    

The initialization sequence can be controlled by the user or by a module by setting the [init_sequence] global variable. When deferred initialization is allowed, certain objects defer their initialization until after other objects have been initialized. See [Initialization] for details. 

## Optional functions

### precommit

The precommit function is called whenever the clock is has just advanced and the internal states of properties have just been synchronized. The precommit process is called only once per timestep and is typically used to prepare objects for the upcoming sequence of synchronization iterations. The function is implemented as follows: 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      int precommit([TIMESTAMP] t0);
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_PRECOMMIT(_class-name_);
    int _class_ ::precommit([TIMESTAMP] t0)
    {
       // **TODO** add precommit process here
       return 1; // return 0 on failure
    }
    

### sync

The presync, sync, and postsync functions are called whenever the main synchronization loop is iterating. The sync processes are called as many times as necessary to resolve a distinct time to which the global clock should advance. As long as at least one object returns the current time, the clock will not advance. The functions are implemented as follows: 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      [TIMESTAMP] presync([TIMESTAMP] t0);
      [TIMESTAMP] sync([TIMESTAMP] t0);
      [TIMESTAMP] postsync([TIMESTAMP] t0);
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_SYNC(_class-name_);
    int _class_ ::presync([TIMESTAMP] t0)
    {
       [TIMESTAMP] t1 = TS_NEVER;
       // **TODO** add pre-topdown sync process here
       // **TODO** set t1 to time of next event
       return t1; // return TS_INVALID on failure
    }
    int _class_ ::sync([TIMESTAMP] t0)
    {
       TIMESTAMP t1 = TS_NEVER;
       // **TODO** add bottom-up sync process here
       // **TODO** set t1 to time of next event
       return t1; // return TS_INVALID on failure
    }
    int _class_ ::postsync([TIMESTAMP] t0)
    {
       TIMESTAMP t1 = TS_NEVER;
       // **TODO** add post-topdown sync process here
       // **TODO** set t1 to time of next event
       return t1; // return TS_INVALID on failure
    }
    

Note
    Not all sync functions are used by every class. The determination of which sync functions are called is made by the [pass control flags] when the class is registered.

### commit

The commit function is called after the main synchronization process is completed and the next clock timestep has been determined. The commit process is called only once per timestep and is typically used to wrap up objects after sequence of synchronization iterations. The function is implemented as follows: 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      [TIMESTAMP] commit([TIMESTAMP] t0);
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_COMMIT(_class-name_);
    [TIMESTAMP] _class_ ::commit([TIMESTAMP] t0)
    {
       [TIMESTAMP] t1 = TS_NEVER;
       // **TODO** add commit process here
       // **TODO** set t1 if soft or hard event is pending
       return t1; // return TS_INVALID on failure
    }
    

Certain objects may be given _observer_ status and allowed to defer their commit processing until after all non-observer objects have completed their commits. See [creating observer classes] for details. 

### finalize

The optional finalize function is called after the simulation loop has completed. It is called for all objects after leaving the loop, whether the simulation was successful or not. It takes no arguments and should assume that other objects have already been finalized and disposed of. The objects themselves should not be deconstructed or free'd, which is the responsibility of the core itself. 

_class_.h
    
    
    #include "gridlabd.h"
    class _class_ {
      // other declarations ...
    public:
      [int64] finalize();
    }
    

_class_.cpp
    
    
    #include "_class_.h"
    EXPORT_FINALIZE(_class-name_);
    [int64] _class_ ::finalize()
    {
       // **TODO** add finalize process here
       return 1; // 1 or SUCCESS on completion.
    }
    

Finalize() is the last call that the exec loop will make to an object. If not present, the block will simply be free()'d without fanfare. 

### notify

**TODO**: 

### notify__property_

**TODO**: 

### isa

**TODO**: 

### plc

**TODO**: 

## User-defined functions

**TODO**: 

## See also

  * [Guide to Programming GridLAB-D]
    * Introduction 
      * [Developer prerequisites]
      * [Programming conventions]
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
      * Class functions
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

