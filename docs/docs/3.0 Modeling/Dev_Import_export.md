# Dev:Import/export

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Import/export
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Example
  * 4 See also
[Import/Export] \- Module import/export functions 

## Synopsis
    
    
    EXPORT int import_file(const char *file);
    EXPORT int export_file(const char *file);
    

## Description

The [import] and [export] directives are used to allow module to load non-GLM data. When [import] is referenced in a GLM file, the appropriate module's **import** function is called. When [export] is references in a GLM file, the module's **export** function will be called when the simulation terminates. 

## Example
    
    
    #include <string.h>
    EXPORT int import_file(char *file)
    {
      char *ext = strrchr(file,'.');
      if (ext!=NULL && stricmp(ext,".cdf")==0 )
        return _my_read_cdf_(file) ? 1 : 0;
        errno = ENOENT;
      return 0;
    }
    EXPORT int export_file(char *file)
    {
      if (file==NULL) file="_default_._ext_ ";
      char *ext = strrchr(file,'.');
      if (ext!=NULL && stricmp(ext,".cdf")==0)
        return _my_write_cdf_(file) ? 1 : 0;
      errno = ENOENT;
      return 0;
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
      * Import/export
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

