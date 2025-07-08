# Dev:KML

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:KML
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Example
  * 4 See also
kmldump \- KML dump function 

## Synopsis
    
    
    #include "gridlabd.h"
    EXPORT int kmldump(int (*stream)(const char *,...), OBJECT *obj);
    

## Description

The KML dump function is used to output a KML data file for Google Earth when the --[kml] [command option] is used. 

## Example

_module_ /main.cpp
    
    
    #include "gridlabd.h"
    EXPORT int kmldump(int (*stream)(const char *,...), OBJECT *obj)
    {
      if ( gl_object_isa(obj,"_class_ ") )
        return OBJECTDATA(obj,_class_)->kmldump(stream);
      // **TODO** add other classes
      else
        return 0;
    }
    

_module_ /_class_.h
    
    
    class _class_ : public gld_object {
      // ... other declarations
      int kmldump(int (*stream)(const char *,...));
    };
    

_module_ /_class_.cpp
    
    
    int _class_ ::kmldump(int (*stream)(const char*, ...))
    {
      if ( isnan(get_latitude()) || isnan(get_longitude()) ) return 0;
      stream("<Placemark>\n");
      stream("  <name>%s</name>\n", get_name());
      stream("  <description>\n<![CDATA[\n");
      // **TODO** add popup data here
      stream("    ]]>\n");
      stream("  </description>\n");
      stream("  <Point>\n");
      stream("    <coordinates>%f,%f</coordinates>\n", get_longitude(), get_latitude());
      stream("  </Point>\n");
      stream("</Placemark>");
    }
    

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
      * KML output
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
