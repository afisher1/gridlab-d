# Publishing properties

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Publishing_properties
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Declarations
    * 3.1 GL_ATOMIC
    * 3.2 GL_STRUCT
    * 3.3 GL_STRING
    * 3.4 GL_ARRAY
    * 3.5 GL_BITFLAGS
  * 4 Example
  * 5 See also
Publishing properties \- Procedure for publishing properties of GridLAB-D classes 

## Synopsis
    
    
    gl_publish_variable(CLASS *_oclass, PROPERTYTYPE_ type _, const char *'name_ , size_t _offset_ , ..., NULL);
    

Declarations
    
    
    #include "gridlabd.h"
    GL_ATOMIC(_type_ ,_name_);
    GL_STRUCT(_type_ ,_name_);
    GL_STRING(_type_ ,_name_);
    GL_ARRAY(_type_ ,_name_ , _size_);
    GL_BITFLAGS(_type_ ,_name_);
    

General accessors
    
    
    size_t get__name_ _offset();
    gld_property get__name_ _property();
    

Atomic accessors
    
    
    _type_ get__name_();
    _type_ get__name_(gld_rlock& _rlock_);
    _type_ get__name_(gld_wlock& _wlock_);
    void set__name_(_type_ _value_);
    void set__name_(_type_ _value_ , gld_wlock& _wlock_);
    

Struct accessors
    
    
    _type_ get__name_();
    _type_ get__name_(gld_rlock& _rlock_);
    _type_ get__name_(gld_wlock& _wlock_);
    void set__name_(_type_ _value_);
    void set__name_(_type_ _value_ , gld_wlock& _wlock_);
    

String accessors
    
    
    char* get__name_();
    char* get__name_(gld_rlock& _rlock_);
    char* get__name_(gld_wlock& _wlock_);
    void set__name_(char* _value_);
    void set__name_(char* _value_ , gld_wlock& _wlock_);
    char get__name_(size_t _n_);
    char get__name_(size_t _n_ , gld_rlock& _rlock_);
    void set__name_(char _value_ , size_t _n_);
    void set__name_(char _value_ , size_t _n_ , gld_wlock& _wlock_);
    

Array accessors
    
    
    _type_ * get__name_();
    _type_ * get__name_(gld_rlock& _rlock_);
    _type_ * get__name_(gld_wlock& _wlock_);
    void set__name_(_type_ * _value_);
    void set__name_(_type_ * _value_ , gld_wlock& _wlock_);
    _type_ get__name_(size_t _n_);
    _type_ get__name_(size_t _n_ , gld_rlock& _rlock_);
    void set__name_(_type_ _value_ , size_t _n_);
    void set__name_(_type_ _value_ , size_t _n_ , gld_wlock& _wlock_);
    

Bitflag accessors
    
    
    _type_ get__name_(_type_ mask=-1);
    _type_ get__name_(gld_rlock& _rlock_);
    _type_ get__name_(gld_wlock& _wlock_);
    void set__name_(_type_ _value_);
    void set__name_ _bits(_type_ _value_);
    void clr__name_(_type_ _value_);
    void set__name_(_type_ _value_ , gld_wlock& _wlock_);
    

## Description

The gl_publish_variable function is a variable argument list call used to publish the publicly accessible properties of a GridLAB-D class. The argument list must contain a least one property type, property name and property offset tuple. The property type must be one of the members of [PROPERTYTYPE]. The property name must be a `const char *` and the property offset must be `size_t`. The property list must be NULL terminated. 

Additional options may be added after each property definition tuple. These options include 

PT_INHERIT
    This will include the properties of the parent class (if any) in searches of this class's properties.

PT_ACCESS, _access_
    This allows you to set special access rights (see [PROPERTYACCESS] for details).

PT_FLAGS, _flags_
    This allows you to set special property flags (see [PROPERTYFLAGS] for details).

PT_UNIT, "_definition_ "
    This allows you to set the units for double and complex properties.

PT_SIZE, _size_
    This allows you to define an array of properties.

PT_EXTEND
    This allows you to enlarge the class by the size of the property.

PT_EXTENDBY, _bytes_
    This allows you to enlarge the class by the number of _bytes_ given.

PT_DESCRIPTION, "_description_ "
    This allows you to provide a text description of the property for use in given users help (see --[modhelp] for details)

PT_KEYWORD, "_name_ ", _value_
    This allows you to define one or more keyword values to associate with the property (see set and enumeration for details). Note that you must cast constants to appropriate [built-in types] to avoid argument alignment problems.

## Declarations

### GL_ATOMIC

GL_ATOMIC types are used strictly for data types that can set as an atomic operation on all platforms. 

### GL_STRUCT

If the data type cannot be set as an atomic operation, you must use the GL_STRUCT declaration. 

### GL_STRING

Any data type that is a character array can be declared as GL_STRING to enable string accessors. 

### GL_ARRAY

Data types that are general arrays can be declared as GL_ARRAY to enable array accessors. 

### GL_BITFLAGS

Bitmap and bitflags integers can be declared as GL_BITFLAGS to enable bit set and clear accessors. 

## Example

The following example is taken from the `[assert](https://github.com/gridlab-d/gridlab-d/trunk/assert)` module: 

Header file (`[assert.h](https://github.com/gridlab-d/gridlab-d/trunk/assert/assert.h)`)
    
    
    class g_assert : public gld_object {
    public:
      typedef enum {AS_INIT=0, AS_TRUE=1, AS_FALSE=2, AS_NONE=3} ASSERTSTATUS;
      GL_ATOMIC(ASSERTSTATUS,status)
      GL_STRING(char1024,target);
      GL_STRING(char32,part); 
      GL_ATOMIC(PROPERTYCOMPAREOP,relation);
      GL_STRING(char1024,value);
      GL_STRING(char1024,value2);
      // ...
    };
    

Implementation file (`[assert.cpp](https://github.com/gridlab-d/gridlab-d/trunk/assert/assert.cpp)`)
    
    
    g_assert::g_assert(MODULE *module)
    {
      // ...
      if (gl_publish_variable(oclass,
        PT_enumeration,"status",get_status_offset(),PT_DESCRIPTION,"desired outcome of assert test",
          PT_KEYWORD,"TRUE",(enumeration)AS_TRUE,
          PT_KEYWORD,"FALSE",(enumeration)AS_FALSE,
          PT_KEYWORD,"NONE",(enumeration)AS_NONE,
        PT_char1024, "target", get_target_offset(),PT_DESCRIPTION,"the target property to test",
        PT_char32, "part", get_part_offset(),PT_DESCRIPTION,"the target property part to test",
        PT_enumeration,"relation",get_relation_offset(),PT_DESCRIPTION,"the relation to use for the test",
          PT_KEYWORD,"==",(enumeration)TCOP_EQ,
          PT_KEYWORD,"<",(enumeration)TCOP_LT,
          PT_KEYWORD,"<=",(enumeration)TCOP_LE,
          PT_KEYWORD,">",(enumeration)TCOP_GT,
          PT_KEYWORD,">=",(enumeration)TCOP_GE,
          PT_KEYWORD,"!=",(enumeration)TCOP_NE,
          PT_KEYWORD,"inside",(enumeration)TCOP_IN,
          PT_KEYWORD,"outside",(enumeration)TCOP_NI,
        PT_char1024, "value", get_value_offset(),PT_DESCRIPTION,"the value to compare with for binary tests",
        PT_char1024, "within", get_value2_offset(),PT_DESCRIPTION,"the bounds within which the value must bed compared",
        PT_char1024, "lower", get_value_offset(),PT_DESCRIPTION,"the lower bound to compare with for interval tests",
        PT_char1024, "upper", get_value2_offset(),PT_DESCRIPTION,"the upper bound to compare with for interval tests",
      NULL)<1)
         throw "assert property publish failed";
      // ...
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
      * [KML output]
      * [Example 1]
    * [Creating a class]
      * [Class functions]
      * [Class globals]
      * Publishing properties
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

