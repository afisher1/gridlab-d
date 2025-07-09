# Engine link

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Engine_link
# Engine link

**_Proposed for review_**

## Contents

  * 1 Synopsis
  * 2 Description
    * 2.1 timeout
    * 2.2 interface
    * 2.3 cachesize
  * 3 Version
  * 4 See also
engine link \- GridLAB-D engine link target control file 

## Synopsis

GLM file
    
    
    
    [link] target-control-file;
    

Target control file
    
    
    
    [target] engine
    timeout _seconds_
    interface _type_
    cachesize _megabytes_
    

## Description

The engine link control file is used to establish a connection to an external application implemented using the [glengine] class library. It is always referenced by a [link directive] in a GLM file, i.e., 
    
    
    [link] control-file;
    

The link control file may contain one or more of the following commands in addition to those supported by the [link directive]. 

### timeout

    Specified the maximum number of seconds to wait before a message send or recv times out. A negative value is treated as a very large number. The default is 10 seconds.

### interface

    Specifies the type of interface to used to transfer information between GridLAB-D and the application.

### cachesize

    Specifies the size of the memory cache used by the engine. Note that is specified on the GridLAB-D side of the connection even though the cache is actually implemented on the application side. This is because the amount of data being transferred is known to GridLAB-D and the default value is automatically calculated by GridLAB-D based on the size of the global, import, and export data.

## Version

The engine link was introduced in [Navajo (trunk)]. 

## See also

  * [glengine]
  * engine link
  * [engine protocol]
