# Using Global Variables - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Using_Global_Variables
# Using Global Variables

**TODO**:  Update this for [Hassayampa (Version 3.0)]

## Contents

  * 1 Using Global Variables
    * 1.1 GLOBALVAR *gl_global_create(char *name, …)
    * 1.2 STATUS gl_global_setvar(char *name, …)
    * 1.3 char *gl_global_getvar(char *name, char*value, int len)
    * 1.4 GLOBALVAR *gl_global_find(char *name)
  * 2 Using Module Variables
  * 3 Global Variable Access
GridLAB-D supports dynamic definition of global variables. All of these are accessible through the core by any module at any time, and are exported to the output file as a group. These variables control output verbosity and output files, how many threads to use, how to output the results, the strictness of the global variable creation, various module states, etc. 

# 

Using Global Variables

Global variables are created and accessed through GL_* functions. There is no particular constraint on what can be done with the global variables, but it is recommended to declare them in the module init functions, and explicitly link them to a static variable. 

The **GLOBALVAR** struct should be considered opaque. It is much simpler and much more reliable to only use the struct as a handle for getting and setting the value within the variable. 

## GLOBALVAR *gl_global_create(char *name, …)

Explicitly creates and defines a global variable. The first argument is the name of the variable, which must be unique. The subsequent arguments must specify a PT_type as a second argument, then any arguments for that type, such as keywords, key values, and access types. 

Example: 

` `

`
    
    
     gl_global_create("myglobalname",PT_double,&myglobalvar,PT_ACCESS,PA_REFERENCE,NULL)
    

```

``

This will create an entry named _myglobalname_ that is treated as a _double_ , and will point to _myglobalvar_. **PA_REFERENCE** declares that the value should only be read through the global implementation. The last argument must always be _NULL_ , or the function will behave aberrantly – so don’t skip it. 

## STATUS gl_global_setvar(char *name, …)

This function uses a character string to set the value of a global variable, then returns 0 if the value could not be set as specified, 1 if it could. The arguments either end up as one string in the form (“ _name_ =_val_ ”), or (“ _name_ ”, “ _value_ ”). In both cases, the value is written as a string. 

Example: ` `

`
    
    
     gl_global_setvar(“myglobal”, “4.360”);
    

```

``

This will set “myglobal” to 4.360. 

Example: 

` `

`
    
    
     gl_global_setvar(“myglobal=camera”);
    

```

``

This will fail, since the string input “camera” is nonsensical for a double value. 

## char *gl_global_getvar(char *name, char*value, int len)

This function will look for the most recently constructed variable published with name, and attempt to convert the contents into value (len chars long) with the value for the global variable. If value is null, a static buffer will be used. In either case, a pointer to the buffer holding the string representation of the global variable’s value will be returned on success, and NULL will be returned if the global variable could not be found, or if insufficient buffer space was available for the conversion. 

## GLOBALVAR *gl_global_find(char *name)

Looks for the global variable published as name and returns the first global variable with that name that was found, if any were. 

# 

Using Module Variables

Module-level variables use the global variable interface for construction and access. The significant difference is that the variables must be prefixed with the module name and two colons to associate them with a module within the code. Within model files, these variables can be set within the module property block. For example, 

` my_mod/init.cpp: `

`
    
    
     gl_global_create("my_mod::value1", PT_double, &value1, NULL);
    

```

`` ` my_mod_test.glm: `

`
    
    
     module my_mod{
        value1 42.0;
     }
    

```

``

These module variables will be grouped underneath their modules within model dump XML files. 

# 

Global Variable Access

Global Variable Access Rights  Variable name | Visible | Saved | Loaded | Access   
---|---|---|---|---  
version.major | √ | √ |  | REFERENCE   
version.minor | √ | √ |  | REFERENCE   
command_line | √ | √ |  | REFERENCE   
environment | √ | √ | √ | PUBLIC   
quiet | √ | √ | √ | PUBLIC   
warn | √ | √ | √ | PUBLIC   
debugger | √ | √ | √ | PUBLIC   
gdb | √ | √ | √ | PUBLIC   
debug | √ | √ | √ | PUBLIC   
test | √ | √ | √ | PUBLIC   
verbose | √ | √ | √ | PUBLIC   
iteration_limit | √ | √ | √ | PUBLIC   
workdir | √ | √ |  | REFERENCE   
dumpfile | √ | √ | √ | PUBLIC   
savefile | √ | √ | √ | PUBLIC   
dumpall | √ | √ | √ | PUBLIC   
runchecks | √ | √ | √ | PUBLIC   
threadcount | √ | √ | √ | PUBLIC   
profiler | √ | √ | √ | PUBLIC   
pauseatexit | √ | √ | √ | PUBLIC   
testoutputfile | √ | √ | √ | PUBLIC   
xml_encoding | √ | √ | √ | PUBLIC   
clock | √ | √ | √ | PUBLIC   
starttime | √ | √ | √ | PUBLIC   
stoptime | √ | √ | √ | PUBLIC   
double_format | √ | √ | √ | PUBLIC   
complex_format | √ | √ | √ | PUBLIC   
object_format | √ | √ | √ | PUBLIC   
object_scan | √ | √ | √ | PUBLIC   
object_tree_balance | √ | √ | √ | PUBLIC   
kmlfile | √ | √ | √ | PUBLIC   
modelname | √ | √ |  | REFERENCE   
execdir | √ | √ |  | REFERENCE   
strictnames | √ | √ | √ | PUBLIC   
website | √ | √ | √ | PUBLIC   
urlbase | √ | √ | √ | PUBLIC   
randomseed | √ | √ | √ | PUBLIC   
include | √ | √ |  | REFERENCE   
trace | √ | √ | √ | PUBLIC   
gdb_window | √ | √ | √ | PUBLIC   
tmp | √ | √ | √ | PUBLIC   
force_compile | √ | √ | √ | PUBLIC   
nolocks | √ | √ | √ | PUBLIC   
skipsafe | √ | √ | √ | PUBLIC   
dateformat | √ | √ | √ | PUBLIC   
minimum_timestep | √ | √ | √ | PUBLIC   
platform | √ | √ |  | REFERENCE   
suppress_repeat_messages | √ | √ | √ | PUBLIC   
maximum_synctime | √ | √ | √ | PUBLIC   
run_realtime | √ | √ | √ | PUBLIC   
no_deprecate | √ | √ | √ | PUBLIC   
sync_dumpfile | √ | √ | √ | PUBLIC   
streaming_io | √ |  |  | PROTECTED   
  

  
