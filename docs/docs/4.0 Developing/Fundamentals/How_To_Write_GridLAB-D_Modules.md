# How To Write GridLAB-D Modules - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/How_To_Write_GridLAB-D_Modules
NOTE
    As of [Hassayampa (Version 3.0)] developers should also read [Creating runtime modules] for details on the advanced API.

## Contents

  * 1 Definitions
    * 1.1 Initialization
    * 1.2 Parent Object
    * 1.3 Sibling Object
  * 2 Core, Module, Class Interactions
    * 2.1 Loading the Module
      * 2.1.1 Declaring the Module
        * 2.1.1.1 Setting the Callback Table
        * 2.1.1.2 Constructing Each Class
      * 2.1.2 Registering Classes
      * 2.1.3 Defining Class Properties
  * 3 Layout of the Module
    * 3.1 Required Module Interface Functions
  * 4 Layout of the Class
    * 4.1 Available Class Functions
      * 4.1.1 gl_register_class
      * 4.1.2 gl_publish_variable
      * 4.1.3 gl_publish_function
      * 4.1.4 gl_get_function
      * 4.1.5 gl_find_property
      * 4.1.6 gl_register_type
      * 4.1.7 gl_publish_delegate
      * 4.1.8 Non-wrapped class functions
    * 4.2 Class Properties
    * 4.3 Construction of the Running Class
      * 4.3.1 Create
      * 4.3.2 Loading
      * 4.3.3 Init
      * 4.3.4 Rank Determination
    * 4.4 Execution of the Running Class
      * 4.4.1 Precommit
      * 4.4.2 Presync
      * 4.4.3 Sync
      * 4.4.4 Postsync
      * 4.4.5 Commit
      * 4.4.6 Finalize
  * 5 Troubleshooting Common Problems
    * 5.1 Class Construction
      * 5.1.1 "sync_myclass() is not implemented in module mymodule"
    * 5.2 Class Behavior
      * 5.2.1 Periodic behavior is not occuring
    * 5.3 Memory errors
This section is intended to provide a combination of an explanation of the components of the GridLAB-D compiled modules and compiled classes, and give a walk through for how to take the piecewise understanding and turn it into a working whole. The required module DLL interface, the class interface, the available class functions, and the typical construction of a module will all be touched on. 

# Definitions

The following definitions are explicitly made for this page, though consistency is a primary goal throughout the wiki and the documentation. 

### Initialization

The phase of the main program execution where all objects defined in the loaded model are initialized by calling their init() method. Not to be confused with the program initialization. 

### Parent Object

A parent object is an intrinsic object reference that has been declared to establish an order of dependency to provide convenient access and to control the order of object execution. A parent object is always ranked higher than its children. 

### Sibling Object

A sibling object is one that is referenced by another object, but is not explicitly a child of. The 'left' and 'right' nodes of a link object are examples of such. In the core, sibling objects are defined with mapped object properties and do not have any expectations of relative rank. 

# Core, Module, Class Interactions

## Loading the Module

### Declaring the Module

There are two files that are used to create GridLAB-D modules: main.cpp and init.cpp. Main.cpp is a stamped file that does not significantly differ from one module to the next, apart from the MAJOR and MINOR preprocessor declarations. Init.cpp contains include directives for each files in the module, sets the callback table, declares any module-level global variables, and call the new operator for each class to be registered with the core. 

#### Setting the Callback Table

The callback table is set using the macro set_callback(fntable). The sets the local GL callbacks to use the CALLBACKS structure provided by the core when it called the module init function. 

#### Constructing Each Class

Calling the constructor for each class will in turn call gl_register_class, which will register the class's name with the core and map it to a module, an gl_define_map, which publishes properties for the class. Additional calls to gl_publish_function can be used to register function symbols with the core, so that other modules can make use of exposed functionality. 

### Registering Classes

### Defining Class Properties

# Layout of the Module

## Required Module Interface Functions

# Layout of the Class

## Available Class Functions

### gl_register_class

CLASS *gl_register_class(MODULE *mod, CLASSNAME name, unsigned int size, PASSCONFIG pcfg) 

Registers a class 'name' with module 'mod'. It is assumed to be 'size' bytes long, excluding the object header. During sync passes, it will be called for the passes in the set 'pcfg'. 

PASSCONFIG has seven elements in its bitwise set: PC_NOSYNC indicates that objects of this class should never be synchronized. PC_PRETOPDOWN indicates that objects of this class should be called for the first top-down pass. PC_BOTTOMUP indicates that objects of this class should be called for the bottom-up pass. PC_POSTTOPDOWN indicates that objects of this class should be called for the second top-down pass. 

Four of the elements are flags that control how the class behaves. PC_FORCE_NAME tells the core that all objects of this type must have names, and that the core should halt otherwise. PC_UNSAFE_OVERRIDE_OMIT controls a class's inheritance behavior, informing the core that it should report errors if another class inherits from it without overriding its `init()` call and sync pass settings. PC_PARENT_OVERRIDE_OMIT tells the core to ignore the above flag, indicating that 'we know this class is inheriting functions from other classes that we should be considerate of'. 

### gl_publish_variable

int gl_publish_variable(CLASS *oclass, ...) 

Describes the properties of the specified class. For a thorough explanation, see [Publishing Class Variables]. 

### gl_publish_function

FUNCTION *gl_publish_function(CLASS *oclass, FUNCTIONNAME fname, FUNCTIONADDR func) 

Publishes the name and address of an arbitrary function that other classes and modules can access. The function specified with at 'func' will be accessible through [ gl_get_function()]. 

Returns a pointer to the new GridLAB-D FUNCTION handle on success, NULL on failure. 

See [Publishing Class Functions] for more information. 

### gl_get_function

FUNCTIONADDR gl_get_function(char *cname, char *fname) 

Retrieves a function 'fname' from the class with name 'cname'. 

Returns the address 'f' of the function specified, and can be called with '(*f)(a, b, c)'. 

See [Publishing Class Functions] for more information. 

### gl_find_property

PROPERTY *gl_find_property(CLASS *oclass, PROPERTYNAME pname) 

Locates the property named 'pname' in class 'oclass'. 

Returns the specified property, if found, and NULL otherwise. 

### gl_register_type

DELEGATEDTYPE *gl_register_type(CLASS *oclass, char *type, int(*from_string)(void *, char*), int (*to_string)(void *, char *, int)) 

Used for delegated types. 

### gl_publish_delegate

int gl_publish_delegate(CLASS *, DELEGATEDTYPE *, ...) 

Used for delegated types. 

### Non-wrapped class functions

CLASS *callbacks->class_getname(char *) int callbacks->string_to_property(PROPERTY *prop, void *addr, char *value) int callbacks->property_to_string(PROPERTY *prop, void *addr, char *value, int size) 

  * these functions are not wrapped with a gl_* call at this time, but still available
class->getname is used to retrieve a CLASS pointer when given the name of its class. 

string_to_property will dispatch the string 'value' to the appropriate convert_to_* function for the property 'prop', and feeds the value into the block at 'addr'. property_to_string will print the value of the 'prop' at 'addr' into the buffer 'value' of 'size' length. Both functions will return nonzero values on success. 

## Class Properties

Property Name | Type | Description   
---|---|---  
name | char64 | A unique name to reference one object with. Can be referenced when defining object properties within a GLM file.   
parent | object | A reference to the object 'above' this object. Used for determining ranking. Frequently used as a convenient reference to operate on for the likes of value reading or writing.   
clock | timestamp | Read-only. The time that the object has synchronized to.   
valid_to | timestamp | Read-only. Internal use only.   
latitude | double | The latitude the object is at. Input is a formated double.   
longitude | double | The longitude the object is at. Input is a formated double.   
in_svc | timestamp | The time at which the core will start calling this object's `sync()` method.   
out_svc | timestamp | The time at which the core will stop calling this object's `sync()` method.   
flags | int64 | Internal use only.   
  
## Construction of the Running Class

### Create

Two things happen when the file loader recognizes a `object [classtype]` directive: first, the core allocates a block of memory large enough for both a `struct OBJECT`; second, the class's `create()` callback is located and executed for the class side of the allocated block. 

The `create()` method should only attempt to copy the default values over the class, and to set any default values that are independent of the contents of the object and of other objects. When `create()` is called, not all of the objects have been read out of the input file and the properties within the object block have not been read. 

After copying the default values onto the class, examples of what `create()` should do includes incrementing and setting counters or other UIDs for a given class. 

### Loading

The properties defined for a given object are read from the file and loaded into memory by the loader in core/load.c at this point. There is no way to affect execution programatically in this stage. 

For more information, please refer to [Creating GLM Files]. 

### Init

When the `init()` function is called for a class, the system is relatively 'complete', in that no more objects will be created, and that any load-time object references will have been resolved. During object initialization, any properties that are derived from parent or sibling objects should be set. Object rank has not yet been established, and object parents are still mutable. 

The majority of the initialization actions should be taken within `init()`. This generally involves looking for files, applying random numbers to uninitialized properties, bounds-checking, reference checking, and reading values from other objects. Any operations that derive from values read in from the file should occur here as well. 

### Rank Determination

Once all objects are initialized, all objects are ranked based upon the parent hierarchy and any explicit ranking commands made earlier. 

## Execution of the Running Class

### Precommit

Objects that need to prepare for the synchronization sequence _before_ any others objects start their syncs must implement a precommit function. Precommit is called only once per timestep and the return value is the time to the next update, if any. 

### Presync

Any object that is flagged with "PRETOPDOWN" will be called during the presync phase. This is most generally meant for clearing any accumulators for the coming iteration, determining 'hard' physical changes based on internal states, and writing any values that do not require any calculation. 

Specific algorithms may use this phase differently: powerflow takes this opportunity to propagate voltage outwards when using the radial system solver. 

Prior to [Hassayampa (Version 3.0)]
    The player and shaper classes write to their target objects during this pass. As of [Hassayampa (Version 3.0)] these should use precommit instead.

### Sync

The 'heavy lifting' phase of the sync loop. Any calculations that may be dependent on direct external values, or from values propagated up from their children, should occur here. Objects execute in order from the lowest child to the root of the object tree. 

### Postsync

This is the 'cleanup' phase of the sync loop when a great deal of after-effect calculation occurs. It proceeds from the root object "down" towards the child objects. Some file writing occurs (if iteration driven rather than clock driven), and accumulator values are analyzed and operated on. 

### Commit

The commit function is called at the end of every completed timestep, once all the objects have converged. Any actions independent of other objects should occur here, including writing to files or other buffers. 

This phase executes 'as fast as possible' and unordered (and highly parallel if possible). Because commit is run _after_ all the objects have converged, changing published object properties is theoretically prohibited (but not enforced). Although tempting, changing published properties in commit is a really bad idea. Such changes can cause other objects to become inconsistent, give rise to updating sequencing problems and leave the model in an erroneous state. Typically, the temptation to use commit stems from a more fundamental problem such as trying to model subsecond behaviors when the 1 second timestep is "too long". 

As of [Hassayampa (Version 3.0)] **_Proposed for review_** (see <http://sourceforge.net/p/gridlab-d/tickets/698>)

There is a category of objects called _observers_ (e.g., assert, recorder, collector, histogram) that clearly need to be process their commit operations after all other objects have processed theirs. For this reason, the new pass control flag **PC_OBSERVER** is used to force certain classes to be processed strictly after all non-observer objects. No object should depend on an observer. The global flag [restrict_observer_access] controls how attempts to get data from observers are addressed. 

In addition, although it is not usually necessary, if the [commit_order] global is set to [TOPDOWN] all commits will be processed in top-down rank order to allow children to assume that their parents have already been committed. 

### Finalize

When the simulation is done, any object that implement a finalize function will be called to allow them to complete any remaining calculation. 

# Troubleshooting Common Problems

## Class Construction

### "sync_myclass() is not implemented in module mymodule"

Error message: 

`FATAL [2000-01-01 00:00:00 EST] : object_sync(OBJECT *obj='myobject', TIMESTAMP ts='2000-01-01 00:00:00 EST', PASSCONFIG pass=PC_PRETOPDOWN): int64 sync_myclass(OBJECT*,TIMESTAMP,PASSCONFIG) is not implemented in module mymodule`

Solution: 

The core was unable to find the symbol for either the create or sync function at startup, and caught a null function pointer when it was trying to synchronize "myclass". All classes must include the EXPORT'ed functions "sync_myclass" and "create_myclass", with 'myclass' replaced with the contextually appropriate class name. 

## Class Behavior

### Periodic behavior is not occuring

Problem: 

A class has been designed to trigger behavior on a fixed period, whether to change state once an hour, write output, or to simply trigger a system convergence. This behavior is not occurring when it's expected to, either happening too fast or never at all. 

Solution: 

The most likely problem is that the object's timer is not correctly advancing in step with the clock. There is no guarantee that an object reports 'five minutes until state change' will have a five minute time step ~ other objects may be moving faster and may short-cycle improperly written timers. Verify that the clock has advanced to the desired time for the object's state change, and that the times for the last periodic action and the next periodic action are properly stored or otherwise discernible. 

## Memory errors

Two tools have been used with GridLAB-D to great effect. 

[Application Verifier](http://www.microsoft.com/en-us/download/details.aspx?id=20028)
    This tool works well with VS2005. The instructions for use are provided at the MS download website for [Application Verifier](http://www.microsoft.com/en-us/download/details.aspx?id=20028). It has the advantage of catching bounds errors immediately when they occur rather than waiting for a heap check to catch memory damage as VS2005 debugger normally does. Apparently it does this by allocating all memory so that it ends on a page boundary, which causes an exception whenever the bound is broken.

[valgrind](http://valgrind.org/)
    Although a bit chatty [valgrind](http://valgrind.org/) is very effective.


