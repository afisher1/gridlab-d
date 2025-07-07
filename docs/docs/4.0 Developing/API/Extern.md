# Extern - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Extern
extern \- Define or load external function references [Template:NEW30]

## Synopsis
    
    
    extern "C" _library_name_ :_function_name_1_[,_function_name_2_[,...]];
    extern "C" _library_name_ :_function_name_1_[,_function_name_2_[,...]] {
      /* C code */
    }
    

## Description

A GLM file may reference functions in an external DLL or shared library using the extern directive or define a function using in-line "C" code. 

## Transform functions

A transform must use an external function conforming to the C prototype 
    
    
    typedef struct s_gldvar { void *data; void *info; } GLXDATA;
    typedef int (*TRANSFORMFUNCTION)(int nlhs, GLXDATA plhs[], int nrhs, GLXDATA prhs[]);
    

The function must return a 32-bit integer to specify the duration for which the result of the transform remains valid (in seconds). When a non-zero value is returned, the simulation will not call the function again until the indicated number of simulation seconds have elasped on the clock. A value of 0xfffffff (negative 1 signed int32) indicates an error has occurred and the simulation cannot continue. A value of zero indicates that the function must be called at the next opportunity. 

The value _nlhs_ indicates how many left-hand-side values are referenced by the _plhs_ vector. The value _nrhs_ indicates how many right-hand-side values are referenced by the _prhs_ vector. It is the function programmer's responsibility to ensure that the left-hand-side and right-hand-side values are interpreted correctly and it is the modeler's responsibility to ensure that the GLM file passes the correct data types to the transform function when it is used. No type-checking is possible at compile time and unless the programmer implements type-checking in the transform function using _info_ , none is performed at simulation time. 

## Compiler Options

The following environment variables may be used to control compiler, debugger, and linker options: 

### CCFLAGS

Use [CCFLAGS] to set compiler flags. These are often platform specific flags, but can also be used to control compiler macros. For example: 
    
    
    #setenv CCFLAGS=-Dvar=5
    extern "C" test : function {
      int function(int nlhs, GLXDATA plhs[], int nrhs, GLXDATA prhs[])
      {
         GLXdouble(plhs[0]) = GLXdouble(prhs[0]) + var;
      }
    }
    

### DBFLAGS

Use [DBFLAGS] to set debugging options when debugging is enabled. 

### LDFLAGS

Use [LDFLAGS] to set linker options when generating the dynamic link library of the module. 

### Module Compiler Flags

The [module_compiler_flags] [global variable] may be used to control compiler features such as 

  * Keep working files ([KEEPWORK]),
  * Generate a new library every time instead of keeping unchanged code from previous builds ([CLEAN]),
  * Produce a debugging version of the library ([DEBUG]), and
  * Enable verbose output of compiler commands issued to the system ([VERBOSE]).
## Example

Referenced or defined functions may be used by transforms as illustrated by the following GLM example. 
    
    
     extern "C" mytest : myfunction {
      int myfunction(int nlhs, GLXDATA *plhs, int nrhs, GLXDATA *prhs)
      {
       if ( nlhs==1 && nrhs==2 )
       {
         GLXdouble(plhs[0]) = GLXdouble(prhs[0]) * GLXdouble(prhs[0]) + GLXdouble(prhs[0]) - 5;
         return 0;
       }
       else
         return -1;
     }
     class myclass {
       double x;
       double y;
       double z;
     }
     object myclass {
       x 12.5;
       y 2.5;
       z myfunction(x,y);
     }
    

The effect of this code is to define a function $z = x^2 + y - 5$ and apply it to the object instantiation of _myclass_. 

The exact same result can be obtained by compiling the "C" source file `myfile.c` into the DLL file `myfile.dll`
    
    
    int myfunction(int nlhs, GLXDATA *plhs, int nrhs, GLXDATA *prhs)
    {
      if ( nlhs==1 && nrhs==2 )
      {
         GLXdouble(plhs[0]) = GLXdouble(prhs[0]) * GLXdouble(prhs[0]) + GLXdouble(prhs[0]) - 5;
         return 0;
      }
      else
         return -1;
    }
    

and using the following GLM code to access the function: 
    
    
    extern "C" myfile:myfunction;  
    class myclass {
       double x;
       double y;
       double z;
     }
     object myclass {
       x 12.5;
       y 2.5;
       z myfunction(x,y);
     }
    

The advantage of the second method is the "C" code is completely external from and independent of the GLM model code. 

## Remarks

Matlab MEX programmers may note the similarity with the calling convention for CMEX. The calling convention for transform functions is indeed similar, but not identical to Matlab's CMEX calling convention. Specific difference are as follows: 

  1. Matlab CMEX functions are declared void (no return value). GridLAB-D's are int because the timestamp offset is returned to the transform calling routine to allow clock control, if desired.
  2. Matlab CMEX functions use `mxArray*`, which allows programmers to access to the different data types supported by Matlab. GridLAB-D uses `GLDVAR*`, which allows functions to access both the data and the property info.
## Version

This capability is targeted for [Hassayampa (Version 3.0)] but is not yet fully implemented. 

As of <https://sourceforge.net/p/gridlab-d/code//2993> single source and simple target is supported. This means that a one-variable function can target a single item property but multiple input variables are not yet supported and complex targets are not yet supported. 

As of <https://sourceforge.net/p/gridlab-d/code//2998> inline code is supported on Windows 32-bit using MinGW. Same as of <https://sourceforge.net/p/gridlab-d/code//2999> for MacOSX. 

**TODO**: 

  * Support for multiple inputs.
  * Support for complex outputs.
  * Support for schedule inputs.
## Bugs

The macro interpreter for GridLAB-D's loader is very likely to catch macros before the C-compiler does. If you are using #include, #define, #ifdef, etc. you should not use inline code and use externally compiled code instead. 

When using inline code on Windows the MinGW compiler does not always create the DLL properly and the following warning may be displayed: 
    
    
    WARNING [INIT] : external function 'name' not found in library 'file'
    

If you attempt to use the function in a transform, the following error is displayed 
    
    
    ERROR [INIT] : transform_add_external(source='object:num:variable',function='name',target='object:num:variable'): function is not defined (probably a missing or invalid extern directive)
    

The work-around is to provide the ordinal of the function in the extern declaration as follows: 
    
    
    extern "C" mylibrary : functionA@1, functionB@2 {
      int functionA(...) { return 0; }
      int functionB(...) { return 0; }
    }
    

This gives the Windows DLL load routines a hint about where to find the function. The ordinal should simply be the ordinal position of the function in the inline code starting with position 1. 

## See also

  * [Directives]
  * [Transform]
  * Runtime classes 
    * [Runtime compiler support]
    * [Runtime Class User Guide]
    * Runtime compiler environment variables 
      * [CXX]
      * [CXXFLAGS]
      * [GRIDLABD_DEBUG]
      * [LDFLAGS]
      * [LDPOSTLINK]
  * External function compiler flags
    * [CCFLAGS]
    * [DBFLAGS]
    * [LDFLAGS]
  * [Module compiler flags]
    * [KEEPWORK]
    * [CLEAN]
    * [DEBUG]
    * [VERBOSE]

