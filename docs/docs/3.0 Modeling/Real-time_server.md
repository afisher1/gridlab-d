# Real-time server - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Real-time_server
# Real-time server

**TODO**:  Update for [Hassayampa (Version 3.0)]

Important node
    This capability is partially implemented. See the [Realtime_server] page for details. Striken text indicates the capability is not implemented at this time.

A real-time server (RTserver) module that include synchronous HTTP services has been implemented. The realtime server component has two main features. 

  1. It locks the simulator clock to the system clock. Although the simulator clock may be started before the real-time clock, the simulator will advanced as quickly as possible until the real-time clock time is encountered. From then on the simulation will run in real-time. This allow stable initialization to be performed if necessary.   
If the simulation clock is initialized to TS_INIT, the simulation will start immediately with the real-time clock. This is enabled by setting the global variable `run_realtime` to a non-zero value. ~~If the value is greater than 1, then the clock will run at a rate that many times greater than the system clock.~~
  2. Incoming HTTP traffic on port 80 is handled during the sync process of the server object. ~~As many servers can be created as desired. The name of the server is used for virtual host naming but if the server is unnamed, it takes the name of the local host.~~ Messages are handled synchronously in the order they are received and responses are always delivered in XML.
The following messages are handled: 

`http://_servername_ /_globalname_`
    This query obtains the value of a global variable. The XML result is usually as follows

` `

`
    
    
     <global>
        <_variable-name_ >
           <unit>_unitname_ </unit>
           _value_
        </_variable-name_ >
     </global>
    

```

``

~~~~

~~

`http://_servername_ /_objectname_`
    This message queries the server for an object. The XML result is usually as follows
~~~~~~

``

~~`
    
    
    <_objectname_ >
       <_propertyname_ >
         <unit>_unitname_ </unit>
         _value_
       </_propertyname_ >
       etc...
     </_objectname_ >
    

`~~~~``~~

~~``~~

`http://_servername_ /_objectname_ /_propertyname_`
    This message queries the server for property of an object. The XML result is usually as follows

` `

`
    
    
     <_objectname_ >
       <_propertyname_ >
         <unit>_unitname_ </unit>
         _value_
       </_propertyname_ >
     </_objectname_ >
    

```

``

`http://_servername_ /_objectname_ /_propertyname_ =_value_`
    This message queries the server for property of an object. The XML result is usually as follows

` `

`
    
    
     <_objectname_ >
       <_propertyname_ >
         <unit>_unitname_ </unit>
         _value_
       </_propertyname_ >
     </_objectname_ >
    

```

``

Optionally, units may be appended, e.g., `http://_servername_ /_objectname_ /_propertyname_?='value unit __`

~~~~

~~

`http://_servername_ /_objectname_ /_functionname_?_arg1_ &_arg2_ &_..._ &_argN_`
    This message calls the function on the object. The XML result is usually as follows
~~~~~~

``

~~`
    
    
    <_objectname_ >
       <_functionname_ >
         _int64-value_
       </_functionname_ >
     </_objectname_ >
    

`~~~~``~~

~~``~~


  
