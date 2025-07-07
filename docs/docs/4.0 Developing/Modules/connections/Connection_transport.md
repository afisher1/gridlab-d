# Connection:transport - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:transport
PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Description
    * 2.1 MEM
    * 2.2 TCP
    * 2.3 UDP
  * 3 Version
  * 4 See also
## Synopsis
    
    
    module connection;
    class [xml]|[json] {
      transport #MEM|TCP|UDP;
    }
    

## Description

### MEM

    The transport is made using MMAP (windows) or SHMEM (*nix).

### TCP

    The transport is over a connection-based streaming socket.

### UDP

    The tranport is over a connectionless datagram socket.

## Version

The [connection] module is proposed for [Hatwai (Version 3.1)]. It's development is tracked under [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797). 

## See also

  * [Connection] classes 
    * [xml]
    * [json]
    * [native]
    * [volttron]
    * [matlab]
  * [Connection] variables 
    * [security]
    * [lockout]
  * [Connection] documentation 
    * [Requirements]
    * [Specifications]
    * [Validation]
    * [Developer docs]
