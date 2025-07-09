# Publishing methods

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Publishing_methods
# Publishing methods

## Contents

  * 1 Synopsis
  * 2 Remarks
  * 3 Example
  * 4 Issues
  * 5 See also
Publishing methods \- Procedure to publish methods 

## Synopsis
    
    
    FUNCTIONADDR (*_call_)(object *_object_);
    gl_publish_function(class *_oclass_ , const char *_name_ , [FUNCTIONADDR] _call_);
    FUNCTIONADDR gl_get_function(object *_obj_ , const char *_name_);
    

## Remarks

Classes may expose methods to other classes using the **gl_publish_function** API call. The function address can then be looked up and called as needed. The argument list is not specified and thus cannot be verified when the method is called. 

## Example

Target class
    
    
    #include <stdarg.h>
    #include "gridlabd.h"
    EXPORT void* _my_function_(OBJECT *_obj_ , ...)
    {
      va_list ptr;
      va_start(ptr,obj);
      // **TODO** implement your function here
      va_end(ptr);
      return NULL; // **TODO** return data pointer
    }
    

Calling class
    
    
    #include "gridlabd.h"
    gld_object *target = get_object("_target_object_name_ ");
    FUNCTIONADDR _my_function_ = target->get_function("_my_function_ ");
    void *result = my_function(target->my()); // **TODO** add arguments to call
    

## Issues

As of [Hassayampa (Version 3.0)]
    Methods cannot be called from other instances of GridLAB-D.

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
      * [Class functions]
      * [Class globals]
      * [Publishing properties]
      * Publishing methods
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
