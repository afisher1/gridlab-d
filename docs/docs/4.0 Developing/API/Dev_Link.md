# Dev:Link - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Link
link \- Developer's guide to creating an application link DLL [Template:NEW30]

GridLAB-D developers can create application link modules by coding the interface in C++. 

Three functions must be exported to support the interface: 

`EXPORT bool init(link *_lnk_)`
    This function is called during the initialization process and must return **true** for GridLAB-D to continue.

`EXPORT [TIMESTAMP] sync(link *_lnk_ ,[TIMESTAMP] _t0_)`
    This function is called each time the global clock changes or GridLAB-D iterates (in which case the clock does not change). The function must return **TS_ERROR** to halt GridLAB-D, **TS_NEVER** to allow GridLAB-D to halt at steady state, a future time _t1_ > _t0_ to request a synchronization event at the time _t1_ , or the time _t0_ to force GridLAB-D to iterate.

`EXPORT bool term(link *_lnk_)`
    This function is called when the simulation is completed, either because of an error, because of steady state, or because the global clock has reached the stoptime.

## C++ API

The following are defined in _core/link.h_. 

### link

The link class provides the complete specification and interface elements for an external application link. 

`LINKLIST *get_globals(void)`
    This function retrieves the first item in the list of globals.

`GLOBALVAR *get_globalvar(LINKLIST *_item_)`
    Retrieves the item as a global variable.

`LINKLIST *get_objects(void)`
    This function retrieves the first item in the list of objects.

`OBJECT *get_object(LINKLIST *_item_)`
    Retrieves the item as an object.

`LINKLIST *get_exports(void)`
    This function retrieves the first item in the list of variables to export.

`OBJECTPROPERTY *get_export(LINKLIST *_item_)`
    Retrieves the item as an object property reference.

`LINKLIST *get_imports(void)`
    This function retrieves the first item in the list of variables to import.

`OBJECTPROPERTY *get_import(LINKLIST *_item_)`
    Retrieves the item as an object property reference.

`char *get_name(LINKLIST *_item_)`
    This function is used to retrieve the name of an item.

`char *get_next(LINKLIST *_item_)`
    This function is used to retrieve the next item in an item list.

`char *get_data(LINKLIST *_item_)`
    This function is used to retrieve the GridLAB-D data associated with an item.

`char *get_addr(LINKLIST *_item_)`
    This function is used to retrieve the address of the application data associated with an item.

### LINKLIST
    
    
    typedef struct s_linklist {
    	char *name; // spec for link
    	void *data; // local data
    	void *addr; // remote data
    	size_t size; // size of data
    	size_t index; // index to data
    	struct s_linklist *next;
    } LINKLIST;
    

### OBJECTPROPERTY
    
    
    typedef struct {
    	object *obj; // object reference
    	property *prop; // property reference
    } OBJECTPROPERTY;
    [gld_property](object*,property*);
    

This structure is used to pair objects and properties for import and export items. 

The [gld_property] class is used to obtain values for the variable. 

## Includes

You will need to include the target application's API definition headers and the GridLAB-D API header: 
    
    
    #include <core/gridlabd.h>
    

## Libraries

You will typically need to include the target application's runtime interface link libraries. 

## See also

  * [Link (directive)]
    * [Matlab link]
    * [JSON link] [Template:NEW30]
    * Technical manual
  * **[How To]**
    * [How to plot data using Matlab]
    * [How to create a movie]

