# Connection:mode

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:mode
PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Description
    * 2.1 CLIENT
    * 2.2 SERVER
  * 3 Version
  * 4 See also
## Synopsis
    
    
    module connection;
    class [xml]|[json] {
      mode CLIENT|SERVER;
    }
    

## Description

### CLIENT

    The connection is originated with GridLAB-D as client. The remote host is considered the server and is expected to accept multiple incoming connections from GridLAB-D.

### SERVER

    The connection is originated with the remote host(s) as client. GridLAB-D is considered the server and is expected to accept multiple incoming connections from remote host(s).

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

