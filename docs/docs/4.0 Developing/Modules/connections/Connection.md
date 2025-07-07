# Connection - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection
PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Classes
  * 4 Example
  * 5 Version
  * 6 See also
connection \- Client/server connection module 

## Synopsis
    
    
    module connection {
      [security] NONE|[LOW]|[NORMAL]|[HIGH]|[EXTREME];
      [lockout] 10 s;
    }
    

## Description

The connection module implements a common set of data exchange and co-simulation links for both clients and servers. Various underlying transport protocols are supported and various data format protocols are implemented by the classes of objects. A common security model is shared by all the interfaces ensuring that all external interaction are managed consistently across all the various connection types. 

## Classes

Each class joins three basic characteristics of a connection: 

  * whether the connection [mode] is [SERVER] or [CLIENT],
  * whether the connection [transport] is [UDP] or [TCP], and
  * the data exchange protocol supported by the connection.
The data exchange protocol determines the name of the class. The follow classes (hence protocols) are proposed (red) or supported (blue): 

[xml]
    implements XML links
[json]
    implements JSON links
[fncs_msg ]
    implements FNCS library for FNCS co-simulations.
[helics_msg ]
    implements the HELICS library for HELICS co-simulations.
gridlabd
    implements gridlabd links
[volttron]
    implements volttron links
[matlab]
    implements matlab links

The connection [mode] and [transport] are properties of the underlying class from which all connection classes are derived. 

## Example

The following example sets up a simple XML server 
    
    
    module connection 
    {
       [security] [STANDARD];
    }
    class [xml] 
    {
       [mode] [SERVER]; // enable server mode
       [transport] [TCP]; // use TCP for data transport
       [from] 127.0.0.1; // only accept incoming connections from the local host
       [port] 8080; // use port 8080 
       [version] 1.0; // use XML version 1.0
       [encoding] [UTF8]; // use 8bit character encoding
       [schema] "<http://www.gridlabd.org/gridlabd_3.0.xsd>"; // use 3.0 schema
       [stylesheet] "<http://www.gridlabd.org/gridlabd_3.0.xsl>"; // use 3.0 stylesheet
       [security] [HIGH]; // override default module security
       [link] "file:datamap.txt"; // datalink map from file (allows external link command lists);
       [link] "[allow]:local_object.var1 -> remote_1"; // permit data send
       [link] "[forbid]:local_object.var2 <- remote_2"; // forbid data receive
       [link] "[presync]:local_object.var3 -> remote_3"; // force data send on precommit, presync, sync, postsync, commit, or finalize
    }
    

## Version

The connection module is proposed for [Hatwai (Version 3.1)]. Its development is tracked under [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797). 

## See also

  * Connection classes 
    * [xml]
    * [json]
    * [native]
    * [volttron]
    * [matlab]
  * Connection variables 
    * [security]
    * [lockout]
  * Connection documentation 
    * [Requirements]
    * [Specifications]
    * [Validation]
    * [Developer docs]

