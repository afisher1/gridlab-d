# Gld property - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/PROPERTY

---
 
 
gld_property \- C++ Module API for properties [Template:NEW30]

## Synopsis
    
    
    class gld_property {
    
    private: // data
    	property *prop;
    	object *obj;
    
    public: // constructors/casts
    	gld_property(object *o, property *p);
    	gld_property(object *o, char *n);
    	gld_property([GLOBALVAR] *v);
    	gld_property(char *n);
    	operator PROPERTY*(void);
    
    public: // read accessors
    	int from_string(char *string);
    	PROPERTYACCESS get_access(void);
    	void* get_addr(void);
    	[gld_class]* get_class(void);
    	char* get_description(void);
    	[gld_keyword]* get_first_keyword(void);
    	PROPERTYFLAGS get_flags(void);
    	char *get_name(void);
    	size_t get_size(void);
    	[gld_type] get_type(void);
    	size_t get_width(void);
    	[gld_unit]* get_unit(void);
    	int to_string(char *buffer, int size);
    
    public: // special operations
    	template <class T> void getp(T &value);
    	template <class T> void getp(T &value, [gld_rlock] &lock);
    	template <class T> void getp(T &value, [gld_wlock] &lock);
    	template <class T> void setp(T &value);
    	template <class T> void setp(T &value, [gld_wlock] &lock);
    
    public: // keyword operations
    	[gld_keyword]* find_keyword(unsigned long value);
    	[gld_keyword]* find_keyword(char *name);
    
    public: // compare operations
    	bool compare(char *op, char *a, char *b=NULL, char *p=NULL);
    
    public: // iterators
    	property* get_next(void);
    	bool is_last(void);
    };
    

## Description

The gld_property class provides access to the properties of both [objects] and [global variables]. 

### compare

`bool [[#compare|compare[[(char *[op], char *[a], char *[b]=NULL, char *[part]=NULL)`
    This function compares the property using the operator _op_ to the values _a_ (and _b_ if the operator in _inside_ or _outside_). If the value _part_ is given and the property supports parts (e.g., complex, enduse), then the part indicated is compared.
    The values _a_ and _b_ will be parsed using the property type.
    Supported operators are ==, <=, >=, !=, <, >, _inside_ , and _outside_.
    Supported parts depend on the type of the property: 

  * complex: [real] (double), [imag] (double), [mag] (double), [arg] (double), [ang] (double).
  * [enduse]: [total] (complex), [energy] (complex), [demand] (complex), [breaker_amps] (double), [admittance] (complex), [current] (complex), [power] (complex), [impedance_fraction] (double), [current_fraction] (double), [power_fraction] (double), [power_factor] (double), [voltage_factor] (double), [heatgain] (double), [heatgain_fraction] (double).
  * object: [id] (double), [rng_state] (double), [tp_affinity] (double), [latitude] (double), [longitude] (double), clock ([timestamp]), [valid_to] ([timestamp]), [schedule_skew] (double), [in_svc] ([timestamp]), [out_svc] ([timestamp])
  * [timestamp]: [seconds] (double), [minutes] (double), [hours] (double), [days] (double), [second] (double), [minute] (double), [hour] (double), [day] (double), [month] (double), [year] (double), [weekday] (double), [yearday] (double), [isdst] (double).
### find_keyword

`[gld_keyword]* find_keyword(unsigned long _value_);`
`[gld_keyword]* find_keyword(char *_name_);`
    This function will find a keyword given a value or given a name.

### from_string

`int [from_string](char *buffer)`
    This function is used to read the string to the value associated with this property and object.

### get_access

`[PROPERTYACCESS] [get_access](void)`
    This function is used to determine the [PROPERTYACCESS] flags associated with this property.

### get_addr

`void* [get_addr](void)`
    This function is used to obtain the memory address of the data associated with this property.

### get_class

`[gld_class]* [get_class](void)`
    This function is used to obtain a pointer to the container of the class associated with this property.

### get_description

`char* [get_description](void)`
    This function is used to obtain the description of the property, if any. A NULL pointer is returned if no description is associated with the property.

### get_first_keyword

`[gld_keyword]* [get_keyword](void)`
    This function is used to obtain a pointer to container of the first [KEYWORD] associated with this property, if any. Keywords are only associated with enumeration and set properties.

### get_flags

`[PROPERTYFLAGS] [get_flags](void)`
    This function is used to obtain the [PROPERTYFLAGS] associated with this property.

### get_name

`char* [get_name](void)`
    This function is used to get the name of the property.

### get_next

`property* [get_next](void)`
    This is used to find the next property associated with this object.

### get_size

`size_t [get_size](void)`
    This function is used to determine the size of the property (in units of size of the primitive type).

### get_type

`[gld_type]* [get_type](void)`
    This function is used to obtain a pointer to the container of the [PROPERTYTYPE] associated with this property.

### get_unit

`[gld_unit]* [get_unit](void)`
    This function is used to obtain a pointer to container of the [UNIT] associated with this property.

### getp

`void [getp]([PROPERTYTYPE] &value)`
    This template function is used to get the value associated with this property and object.

### gld_property

There are four available constructors. 

`gld_property(object *obj, property *prop)`
    This constructor is used to access a property of an object when the core property structure is already available.

`gld_property(object *obj, char *name)`
    This constructor is used to access a property of an object when only the name of the property is available.

`gld_property([GLOBALVAR] *var)`
    This constructor is used to access a global variable's property information when the [GLOBALVAR] structure is already available.

`gld_property(char *name)`
    This constructor is used to access a global variable's property information when only the name of the variable is available.

### is_last

`bool [is_last](void)`
    This is used to determine whether this property is the last in list of properties associated with this object.

### PROPERTY

(property)
    This cast is used to gain access to the core property structure used by the property.

### to_string

`int [to_string](char *buffer, int size)`
    This function is used to write the value associated with this property and object to a string.

### setp

`void [setp]([PROPERTYTYPE] &value)`
    This template is used to set the value associated with this property and object.

## Examples

To obtain access a property in another object: 
    
    
    gld_property myvar(my_obj,"varname");
    

To get the value of the property (assuming it's a double): 
    
    
    double value;
    myvar.getp(value);
    

To set the value of the property (assuming it's a double): 
    
    
    myvar.setp(12.4);
    

To write the property to a string: 
    
    
    char buffer[256];
    myvar.to_string(buffer,sizeof(buffer));
    

To read the value from a string: 
    
    
    myvar.from_string("18.2");
    

To compare the value to another value: 
    
    
    if ( myvar.compare("<","0") )
      output_warning("myvar is negative");
    

## Version

The C++ [Module API] was introduced in [Hassayampa (Version 3.0)] to support multithreaded modules. 

## See also

  * [Source documentation]

