# Dev:load method - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:load_method
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Return value
  * 4 History
  * 5 See also


Load methods \- Procedure to handle loader pseudo-properties 

## Synopsis

_module_ /_class_.h
    
    
    #include "gridlabd.h"
    
    
    
    class _class_ : public gld_object
    {
      // ...
    public:
      int _class_ ::_method_(PROPERTY *prop, char *value);
      // ...
    }
    

_module_ /_class_.cpp
    
    
    EXPORT_LOADMETHOD(_class_ ,_method_);
    
    
    
    _class_ ::_class_(MODULE *module)
    {
      // ...
      if ( !gl_publish_loadmethod(oclass,"_method_ ",loadmethod__class_ __method_) )
        throw "_module_ /_class_ ::_class_(MODULE*): unable to publish _method_ method";
      // ...
    }
    
    
    
    int _class_ ::_method_(char *value)
    {
      // parse _value_
      // return 0 on failure
      // return 1 on success
      return 0;
    }
    

## Description

Whenever the loader encounters a property name that is defined as a load method, it's value is dispatched to the load methods defined in the associated class. These properties are called _pseudo-properties_ and can occur more than once in any given object. 

## Return value

The notifier returns 0 if the pseudo-property value is not parsed successfully and 1 if it is parsed successfully. 

## History

Load methods were introduced in [ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797) and deployed in [Jojoba (Version 3.2)]. 

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
