# Gld object

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Gld_object
gld_object \- C++ API for OBJECT container [Template:NEW30]

## Synopsis
    
    
    class _my_class_ : public gld_object {
    
    protected: // data (internal use only)
    	object *my();
    
    public: // constructors
    
    public: // header read accessors (no locking)
    	[OBJECTNUM] get_id(void);
    	char* get_groupid(void);
    	[gld_class]* get_oclass(void);
    	gld_object *get_parent(void);
    	[OBJECTRANK] get_rank(void);
    	[TIMESTAMP] get_clock(void);
    	[TIMESTAMP] get_valid_to(void);
    	[TIMESTAMP] get_schedule_skew(void);
    	[FORECAST] *get_forecast(void);
    	double get_latitude(void);
    	double get_longitude(void);
    	[TIMESTAMP] get_in_svc(void);
    	[TIMESTAMP] get_out_svc(void);
    	const char *get_name(void);
    	int get_tp_affinity(void);
    	[NAMESPACE] *get_space(void);
    	unsigned int get_lock(void);
    	unsigned int get_rng_state(void);
    	unsigned long get_flags(unsigned long mask=0xffffffff);
    
    protected: // header write accessors (no locking)
    	void set_forecast([FORECAST] *fs);
    	void set_latitude(double x);
    	void set_longitude(double x);
    	void #set_flags(unsigned long flags);
    
    protected: // locking (self)
    	void rlock(void);
    	void runlock(void);
    	void wlock(void);
    	void wunlock(void);
    protected: // locking (others)
    	void rlock(OBJECT*);
    	void runlock(OBJECT*);
    	void wlock(OBJECT*);
    	void wunlock(OBJECT*);
     
    protected: // member lookup functions
    	property* get_property(char *name);
    	[FUNCTIONADDR]* get_function(char *name);
    
    public: // external accessors
    	template <class T> void getp(property *prop, T &value);
    	template <class T> void getp(property *prop, T &value, [gld_rlock]&);
    	template <class T> void getp(property *prop, T &value, [gld_wlock]&);
    	template <class T> void setp(property *prop, T &value);
    	template <class T> void setp(property *prop, T &value, [gld_wlock]&);
    
    public: // core interface
    	int set_dependent(object *obj);
    	int set_parent(object *obj);
    	int set_rank(unsigned int r);
    	bool isa(char *type);
    
    public: // iterators
    	bool is_last(void);
    	object *get_next(void);
    }
    

## Description

The gld_object base class provides the basic linkage to GridLAB-D's core. It supersedes the implementation of the core linkage provided by using the macros and callbacks included in `gridlabd.h`. As of [Hassayampa (Version 3.0)] new modules should be implemented using this method to support multithreading properly. 

**TODO**:  Describe class members 

### getp

`void gld_object::getp(property &_prop_ ,[TYPE] &_value_)`
    This template provides a general-purpose locked read accessor for all published properties of the class.
    
    
    // allocate local space for value
    double myvalue = 0;
    
    // get property information
    [gld_property] prop(my(),"value");
    
    // get value (using read lock)
    getp(prop,myvalue);
    

`void gld_object::getp(property &_prop_ ,[TYPE] &_value_ , [gld_rlock] &lock)`
`void gld_object::getp(property &_prop_ ,[TYPE] &_value_ , [gld_wlock] &lock)`
    This functions allow reading of data with an existing lock to avoid deadlocks.
    
    
    double myvalue;
    [gld_property] prop(my(),"value");
    if ( prop.is_valid() )
    {
      [gld_wlock] lock(my());
      getp((PROPERTY*)prop,myvalue,lock);
      myvalue*=3.14;
      setp((PROPERTY*)prop,myvalue,lock);
    }
    

    Although the above code works fine, it is often easier to use the [gld_property]'s get/set members:
    
    
    double myvalue;
    [gld_property] prop(my(),"value");
    if ( prop.is_valid() )
    {
      [gld_wlock] lock(my());
      prop.[getp](myvalue,lock);
      myvalue*=3.14;
      prop.[setp](myvalue,lock);
    }
    

### get_clock

`TIMESTAMP gld_object::get_clock(void)`
    This returns the object's clock.
    
    
    // get the object's clock and convert it to a string
    [gld_clock] t3([get_clock]());
    char buffer[64];
    t3.[to_string](buffer,sizeof(buffer));
    

### get_flags

### get_forecast

### get_id

### get_in_svc

### get_groupid

### get_latitude

### get_lock

### get_longitude

### get_name

### get_next

### get_oclass

### get_out_svc

### get_parent

### get_property

### get_rank

### get_rng_state

### get_schedule_skew

### get_space

### get_tp_affinity

### get_valid_to

### is_last

`bool is_last(object *obj)`
    This function is used to determine whether an object is the last object in the core's list of objects. This is used for iterators that wish to determine which objects come after the current object in the creation order. See the [init_sequence] [global variable] for details on the use of the creation order list.
    
    
    object *obj;
    for ( obj=my ; !is_last(obj) ; obj=get_next(obj) )
      // iterates through all object created after _my_
    

`bool is_last(void)`
    This function is used to determine whether the object my is the last object created in the core's list of objects.

### isa

### my

`property `object *my``
`     This is always defined when the base class gld_object is used. It is used to access the object header and object data. The property my is not defined until the end of the create() call. Consequently, many member functions are not supported until after create() returns.
``

### rlock

``

### runlock

``

### setp

``

`gld_object::setp(property &_prop_ ,[TYPE] &_value_)`
    This template provides a general-purpose locked write accessor for all published properties of the class.
``
    
    
    // allocate local space for value
    double myvalue = 0;
    
    // get property information
    [gld_property] prop(my,"value");
    
    // set value (using write lock)
    setp(prop,myvalue);
    

``

`void gld_object::setp(property &_prop_ ,[TYPE] &_value_ , [gld_wlock] &lock)`
    This function allows writing of data with an existing lock to avoid deadlocks.
``
    
    
    double myvalue;
    [gld_property] prop(my(),"value");
    if ( prop.is_valid() )
    {
      [gld_wlock] lock(my());
      getp((PROPERTY*)prop,myvalue,lock);
      myvalue*=3.14;
      setp((PROPERTY*)prop,myvalue,lock);
    }
    

``

    Although the above code works fine, it is often easier to use the [gld_property]'s get/set members:
``
    
    
    double myvalue;
    [gld_property] prop(my(),"value");
    if ( prop.is_valid() )
    {
      [gld_wlock] lock(my());
      prop.[getp](myvalue,lock);
      myvalue*=3.14;
      prop.[setp](myvalue,lock);
    }
    

``

### set_dependent

``

### set_forecast

``

### set_longitude

``

### set_latitude

``

### set_parent

``

### set_rank

``

### wlock

``

### wunlock

``

## Version

``

The C++ [Module API] was introduced in [Hassayampa (Version 3.0)] to ensure support for multithreading. 

``

## See also

``

  * [Module API source documentation](http://gridlab-d.sourceforge.net/doxygen/3.0/group__module__api.html)

`


