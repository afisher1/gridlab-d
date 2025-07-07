# Code templates - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Code_templates
# Code templates

## Contents

  * 1 Required code
    * 1.1 _module_ /main.cpp
    * 1.2 _module_ /_class_.h
    * 1.3 _module_ /_class_.cpp
    * 1.4 _module_ /Makefile.am
    * 1.5 _module_ /_module_.vcproj
  * 2 Optional code
  * 3 See also
Code templates \- Code templates for module developers 

The code templates may be downloaded to create a new module or class. In each template you will find a number of instances of <<<KEYWORD>>>, indicating that a substitution is required to properly implement your module or class. In addition, you may find a number of **TODO** comments indicating where you may have to write additional code to properly implemented the desired functionality. 

The following keywords are used: 

<<<MODULE>>>
    The name of the module
<<<YEAR>>>
    The year (or year range) of the copyright
<<<COMPANY>>>
    The name of copyright holder, i.e., the author or company to which the copyright is assigned
<<<CLASS>>>
    The class you are implementing
<<<GUID>>>
    A global unique identifier (see [GUID] for details)

## Required code

### _module_ /main.cpp
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #define DLMAIN // required to enable module main code in gridlabd.h
    #include "gridlabd.h"
    #include "<<<CLASS>>>.h"
    // **TODO** add class includes here
    // **TODO** define [module globals] here
    EXPORT [CLASS] *init([CALLBACKS] *fntable, [MODULE] *module, int argc, char *argv[])
    {
            if (set_callback(fntable)==NULL)
            {
                    errno = EINVAL;
                    return NULL;
            }
            // **TODO** publish [module globals] here
            new <<<CLASS>>>(module);
            // **TODO** add other classes here
            // always return the first class registered
            return <<<CLASS>>>::oclass;
    }
    EXPORT int do_kill(void*)
    {
            // **TODO** module cleanup if any
            return 0;
    }
    // **TODO** add [optional functions]
    

### _module_ /_class_.h
    
    
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
      <<<CLASS>>>([MODULE] *module);
      int create(void);
      int init(OBJECT *parent);
      // **TODO** add optional [class functions]
    public: // optional/user-defined functions
      // **TODO** add published [class functions]
    private: // internal functions
      // **TODO** add desired internal functions
    public: // required members
      static [CLASS] *oclass;
      static <<<CLASS>>> *defaults;
    };
    #endif // _<<<CLASS>>>_H
    

### _module_ /_class_.cpp
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #include "<<<CLASS>>>.h"
    EXPORT_CREATE(<<<CLASS>>>);
    EXPORT_INIT(<<<CLASS>>>);
    // **TODO** add [optional functions declarations]
    [CLASS] <<<CLASS>>>::oclass = NULL;
    <<<CLASS>>> *<<<CLASS>>>::defaults = NULL;
    // **TODO** add declaration of [class globals]
    <<<CLASS>>>::<<<CLASS>>>([MODULE] *module)
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
    