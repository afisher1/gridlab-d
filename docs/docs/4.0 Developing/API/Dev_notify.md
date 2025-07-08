# Dev:notify

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:notify
# Dev:notify

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Return value
  * 4 Issues
  * 5 See also
Notifications \- Procedure to obtain object/property change notifications 

## Synopsis

_module_ /_class_.h
    
    
    #include "gridlabd.h"
    EXPORT_NOTIFY(_class_);
    EXPORT_NOTIFY_PROP(_class_ ,_property_);
    int _class_ ::prenotify(PROPERTY *prop, char *value);
    int _class_ ::postnotify(PROPERTY *prop, char *value);
    

## Description

Whenever a property is changed using the module API, a notification is sent to any class that has registered a notifier. If the property notification is used, the notification message will only be sent when the specified property is changed. 

## Return value

The notifier returns 0 is the notification is not handled and non-zero if it is handled. 

## Issues

GridLAB-D does not mandate using accessors to write properties of objects. As a result, the core cannot guarantee that all changes to object properties will result in notifications. 

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
      * Notifications
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
