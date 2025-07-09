# Connection:xml

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:xml
# Connection:xml

PROPOSED FOR 3.1 

## Contents

  * 1 Synopsis
  * 2 Class members
    * 2.1 encoding
      * 2.1.1 UTF8
      * 2.1.2 UTF16
    * 2.2 schema
    * 2.3 stylesheet
    * 2.4 version
  * 3 Example
  * 4 Version
  * 5 See also
## Synopsis
    
    
    module [connection];
    class xml {
       version 1.0; 
       encoding UTF8|UTF16; 
       schema "_[xsd]-url_"; 
       stylesheet "_[xsl]-url_"; 
    }
    

## Class members

### encoding

    Determine the encoding of characters.

#### UTF8

    Character encoding uses 8 bits.

#### UTF16

    Character encoding uses 16 bits.

### schema

    Identifies the XML schema ([xsd]) to use.

### stylesheet

    Identifies the XML stylesheet ([xsl]) to use.

### version

    Identifies the XML version to use.

## Example
    
    
    module [connection];
    class xml 
    {
       [mode] [SERVER]; // enable server mode
       [transport] [TCP]; // use TCP for data transport
       [version] 1.0; // use XML version 1.0
       [encoding] [connection:xml#UTF8]; // use 8bit character encoding
       [schema] "<http://www.gridlabd.org/gridlabd_3.0.xsd>"; // use 3.0 schema
       [stylesheet] "<http://www.gridlabd.org/gridlabd_3.0.xsl>"; // use 3.0 stylesheet
    }
    

## Version

The [connection] module is proposed for [Hatwai (Version 3.1)]. It's development is tracked under [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797). 

## See also

  * [Connection] classes 
    * xml
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
