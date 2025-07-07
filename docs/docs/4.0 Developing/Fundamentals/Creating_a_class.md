# Creating a class - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Creating_a_class
## Contents

  * 1 Synopsis
    * 1.1 Header file
    * 1.2 Source file
    * 1.3 Autoconf file
    * 1.4 Project file
  * 2 Pass control
  * 3 Forced naming
  * 4 Automatic locking
  * 5 Observers
  * 6 Parent classes
    * 6.1 Abstract classes
    * 6.2 Parent overrides
  * 7 See also
Creating a class \- Procedure to create a new class in a GridLAB-D module 

## Synopsis
    
    
    _module-name_ /_class-name_.h
    _module-name_ /_class-name_.cpp
    _module-name_ /Makefile.am
    _module-name_ /_module-name_.vcproj
    

### Header file

_module-name_ /_class-name_.h
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #ifndef _<<<CLASS>>>_H
    #define _<<<CLASS>>>_H
    #include "gridlabd.h"
    class <<<CLASS>>> : public gld_object {
    public: // published variables
      // **TODO** add public typedefs
      // **TODO** declare published variables using [GL_* macros]
    private: // unpublished variables
      // **TODO** add private typedefs
      // **TODO** add unpublished variables
    public: // required functions
      <<<CLASS>>>(module *module);
      int create(void);
      int init(OBJECT *parent);
      // **TODO** add optional [class functions]
    public: // optional/user-defined functions
      // **TODO** add published [class functions]
    private: // internal functions
      // **TODO** add desired internal functions
    public: // required members
      static class *oclass;
      static <<<CLASS>>> *defaults;
    };
    #endif // _<<<CLASS>>>_H
    

### Source file

_module-name_ /_class-name_.cpp
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #include "<<<CLASS>>>.h"
    EXPORT_CREATE(<<<CLASS>>>);
    EXPORT_INIT(<<<CLASS>>>);
    // **TODO** add [optional functions declarations]
    class <<<CLASS>>>::oclass = NULL;
    <<<CLASS>>> *<<<CLASS>>>::defaults = NULL;
    // **TODO** add declaration of [class globals]
    <<<CLASS>>>::<<<CLASS>>>(module *module)
    {
      if ( oclass!=NULL )
        exception("cannot register class more than once");
      oclass = gld_class::create(module,"<<<CLASS>>>",sizeof(<<<CLASS>>>),<<<OPTIONS>>>);
      if ( oclass==NULL )
        exception("class registration failed");
      oclass->trl = <<<TRL>>>;
      if ( gl_publish_variable(oclass, <<<VARIABLESPECS>>>, NULL)<1 )
        exception("unable to publish properties");
      memset(defaults=this,0,sizeof(*this));
      // **TODO** set defaults
    }
    <<<CLASS>>>::create(void)
    {
      memcpy(this,defaults,sizeof(*this));
      // **TODO** set defaults
      return SUCCESS; // return FAILED on create error
    }
    <<<CLASS>>>::init(OBJECT *parent)
    {
      // **TODO** initialize object
      return SUCCESS; // return FAILED on create error
    }
    // **TODO** add implementations of [optional class functions]
    
    

### Autoconf file

_module-name_ /Makefile.am
    
    
    pkglib_LTLIBRARIES = <<<MODULE>>>.la
    <<<MODULE>>>_la_SOURCES = main.cpp \
    	<<<CLASS>>>.cpp <<<CLASS>>>.h \
           # **TODO** add new classes before this line
    <<<MODULE>>>_la_LDFLAGS = -module -no-undefined -avoid-version -version-info 1:0:0
    
    uninstall-hook:
    	-rmdir $(DESTDIR)$(pkglibdir)
    

### Project file

Additional include directories
    ..\core
Debug information format
    Program Database (/Zi)

**TODO**:  add other VS project options 

## Pass control

**TODO**: 

## Forced naming

**TODO**: 

## Automatic locking

**TODO**: 

## Observers

**TODO**: 

## Parent classes

**TODO**: 

### Abstract classes

**TODO**: 

### Parent overrides

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
    * Creating a class
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

