# Dev:Subsecond

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Subsecond
Approval item:  TECHNICAL MANUAL [Grizzly (Version 2.3)]

## Contents

  * 1 Coding Guide
    * 1.1 Module Initialization
      * 1.1.1 Global variables
      * 1.1.2 Export functions
    * 1.2 Class implementations
    * 1.3 Multithreading Support
    * 1.4 See also
[![](//images.shoutwiki.com/gridlab-d/thumb/8/88/Deltamode.png/300px-Deltamode.png)](/wiki/File:Deltamode.png)

[]

Delta mode UML flow diagram [Edit this diagram](http://www.websequencediagrams.com/?lz=bWFpbi0-ZXhlYzogc3RhcnQKYWN0aXZhdGUgZXhlYwoKbG9vcCBtYWluIGV2ZW50IGxvb3AKCmV4ZWMtPmRlbHRhOiBkZXNpcmVkPwAxCgAUBQoAGgUAVQgAJwUgcnVudGltZQpkZQAdDwBiBgBdBnNvbHZlciBpdGVyYXRpb24AZwhvYmplY3Q6IHBhc3MgcHJlL3Bvc3Qgc3luYy9jb21taQCBMQsAJQYKACwGAIFYCG5leHQAgT8HAHUQACkHCmVuZAoKb3B0AIEoB21vZGUAgVcIAIFmD3VwZGF0ZQCBYxAAgWwIbW9kdWxlOiBwcmUAGRAAFQYKABwGAII7CXN0YXR1cwCCDAwAIQcAgnsGAII4BnRpbWVzdGVwAAgNAIIcCwCCagcAgiAIAIETEACCBg8AYRkAggUIAIE9D2ludGVyAIEoJmNvbnRpbnUAg1sNAIFICACCXgUAgmMFAIIcEG9zdACBbj8AhGUNAIJMBwCEWRMAhHMLAIVfBgCBCgoAhVMGbWFpbjogZG9uZQoKCgo&s=default)

Delta mode is a special mode of simulation that allow models to be advanced at very small fixed time increments. Modules may use delta mode by implementing the feature described in this page. 

The principle differences between the conventional event driven mode and the finite time difference (delta) mode are as follows: 

Event mode

* * *

| Delta mode

* * *

|   
---|---|---  
Variable time step | Fixed time step |   
1 second minimum time step | 1 nanosecond minimum time step |   
No maximum time step | Modules request maximum time step |   
No maximum run time | Modules request maximum run time |   
  
# Coding Guide

To support delta mode processing, modules must be modified as follows. 

## Module Initialization

The following changes to `init.c` or `init.cpp` are required to enable delta mode operation for a module. 

### Global variables

Add the following global variables 
    
    
    bool enable_subsecond_models = false; /* normally not operating in delta mode */
    unsigned long deltamode_timestep = 10000000; /* in ns, e.g., 10 ms timestep */
    

and add the following global registrations to init() 
    
    
    gl_global_create("_modulename_ ::enable_subsecond_models", PT_bool, &enable_subsecond_models,NULL);
    gl_global_create("_modulename_ ::deltamode_timestep", PT_int32, &deltamode_timestep,NULL);
    

enable_subsecond_models
    This boolean value should be used to indicate whether delta mode operation is desired. It can be set by the GLM modeler or it can be set by internal module code, depending on the modeling approach chosen.

deltamode_timestep
    This value indicates how long the timestep is desired when operating in delta mode. The value is given in nanoseconds, so be carefully when combining it with clock values given in seconds. You can use the DT_SECOND macro which specifies how many delta ticks there are in a clock tick.

### Export functions

Add the following exported functions after init() 
    
    
    EXPORT int deltamode_desired(int *flags)
    {
    	/* indicate whether delta mode is desired */
    	flags |= DMF_SOFTEVENT; /* set flag if not required, but only desired */
    
    	/* **TODO** Add your deltamode_desired code */
    	
    	return dt_val;	/* Returns time in seconds to when the next deltamode transition is desired */
    	
    	/* Return DT_INFINITY if this module doesn't have any specific triggering times */
    }
    
    EXPORT int preupdate(MODULE *module, TIMESTAMP t0, unsigned int64 dt)
    {
    	/* **TODO** Add preupdate code */
    	/* t0 is the current global clock time (event-based) */
    	/* dt is the current deltaclock nanosecond offset from t0 */
    	
    	return deltamode_timestep;	/* Return minimum delta timestep module requires */
    	
    	/* return DT_INFINITY if this module doesn't have any minimum timstep requirements */
    	/* DT_INFINITY can also be returned if no deltamode updates are desired as part of this module */
    }
    
    EXPORT int interupdate(MODULE *module, TIMESTAMP t0, unsigned int64 dt, unsigned int iteration_count_val)
    {
    	/* **TODO** Add your interupdate code here */
    	/* Module-level call at each deltatimestep, including iterations */
    	/* t0 is the current global clock time (event-based) */
    	/* dt is the current deltaclock nanosecond offset from t0 */
    	/* interation_count_val is the current iteration on deltatime dt */
    	
    	return SM_EVENT; /* return SM_DELTA, SM_DELTA_TER, SM_EVENT, or SM_ERROR */
    	
    	/* SM_DELTA - ready to proceed to next delta timestep */
    	/* SM_DELTA_ITER - stay at this delta timestep and reiterate */
    	/* SM_EVENT - deltamode is no longer needed, this module is ready to return to event mode */
    	/* SM_ERROR - an error has occurred */
    }
    
    EXPORT int postupdate(MODULE *module, TIMESTAMP t0, unsigned int64 dt)
    {
    	/* **TODO** Add your postupdate code here */
    	/* t0 is the current global clock time (event-based) */
    	/* dt is the current deltaclock nanosecond offset from t0 */
    	
    	return SUCCESS;	/* return FAILURE (0) or SUCCESS (1) */
    }
    

int deltamode_desired()
    This function is used by the core to find out whether the module wants to use deltamode and if so, how long before delta mode must begin (in seconds).

int preupdate()
    This function is used by the core to notify modules that it is about to start a series of delta mode updates. This happens only when the simulation is switching from event mode to delta mode. The return value is the delta timestep this module requires.

int interupdate()
    This function is used by the core to notify modules that is has completed a single timestep in delta mode, or is reiterating the current delta mode timestep.

int postupdate()
    This function is used by the core to notify modules that it has completed a series of delta mode updates. This happens only when the simulation is switching from delta mode to event mode.

## Class implementations

To each class that needs to process object-level updates add the following at the end of implementation file (e.g., _class_.cpp) 
    
    
    int _class_ ::deltaupdate(unsigned long dt)
    {
      /* **TODO** add your object update code here */
      return 1;
    }
    EXPORT int update__class_(OBJECT *obj, unsigned long dt, unsigned int iteration_count_val)
    {
      _class_ *my = OBJECTDATA(obj,_class_);
      int status = 0;
      try
      {
        status = my->deltaupdate(dt);
        return status;
      }
      catch (char *msg)
      {
        gl_error("update__class_(obj=%d;%s): %s", obj->id, obj->name?obj->name:"unnamed", msg);
        return status;
      }
    }
    

and add the declaration of the update function to the class definition itself 
    
    
    class _class_
    {
      // ...
    public:
      int _class_ ::deltaupdate(unsigned long dt);
      // ...
    }
    

Classes should only implement this function when it is absolutely necessary that objects be notified of a delta mode timestep. Objects should only enable the call using OF_DELTAMODE when it is absolutely necessary because this call can have very significant performance impacts on the speed of delta mode. Careful consideration should be given to the code design of the module in order to reduce the necessity and complexity of this function. 

## Multithreading Support

Multithreaded operation is supported at two levels, module level and object level. 

Module updates
    Each module update shall be assumed to be independent of all others. The main loop shall implement multithreading when enabled by the user by running each module update as a separate thread.

Object updates
    Each object update shall be assumed to be independent of all others within the same rank. The object update loop shall implement multithreaded when enabled by the user by running each object update as a separate thread.

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
      * Subsecond processing
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
  * [Subsecond]
    * Development 
      * [Requirements]
      * [Specifications]
      * Programmer's Manual
    * [Global variables]
      * [simulation_mode]
      * [deltamode_maximumtime]
      * [deltamode_timestep]
      * [deltamode_preferred_module_order]
      * [deltamode_updateorder]
      * [deltamode_iteration_limit]
      * [deltamode_forced_extra_timesteps]
      * [deltamode_forced_always]
