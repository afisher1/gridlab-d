# Connection:native - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:native
PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Class members
    * 2.1 version
  * 3 Example
  * 4 Version
  * 5 See also
## Synopsis
    
    
    module [connection];
    class native {
       version 3.0; 
    }
    

## Class members

### version

    Identifies the gridlabd version to use.

## Example
    
    
    module [connection];
    class native 
    {
       [mode] [SERVER]; // enable server mode
       [transport] [TCP]; // use TCP for data transport
       [version] 3.0; // use GridLAB-D Version 3.0 format
    }
    

## Version

The [connection] module is proposed for [Hatwai (Version 3.1)]. It's development is tracked under [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797). 

## See also

  * [Connection] classes 
    * [xml]
    * [json]
    * native
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

