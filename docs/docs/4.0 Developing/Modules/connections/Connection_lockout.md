# Connection:lockout

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:lockout
PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Version
  * 4 See also
lockout \- Client/server connection module security lockout time control property 

## Synopsis
    
    
    module [connection] {
      lockout 10 s;
    }
    

## Description

The lockout property of the [connection] module determines the time that a server will lock out a client when it violates an [EXTREME] security constraint. Lockouts only occur when [security] is set to [PARANOID]. 

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
    * lockout
  * [Connection] documentation 
    * [Requirements]
    * [Specifications]
    * [Validation]
    * [Developer docs]
