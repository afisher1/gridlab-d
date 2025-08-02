# Connection:json

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Connection:json
Approval item: 

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Class members
    * 3.1 link
      * 3.1.1 allow
      * 3.1.2 forbid
    * 3.2 option
      * 3.2.1 init
      * 3.2.2 precommit
      * 3.2.3 presync
      * 3.2.4 sync
      * 3.2.5 postsync
      * 3.2.6 prenotify
      * 3.2.7 postnotify
      * 3.2.8 commit
      * 3.2.9 finalize
      * 3.2.10 term
      * 3.2.11 connection
      * 3.2.12 client
      * 3.2.13 server
      * 3.2.14 udp
      * 3.2.15 readcache
      * 3.2.16 writecache
      * 3.2.17 transport
      * 3.2.18 port
      * 3.2.19 header_version
      * 3.2.20 hostname
      * 3.2.21 debug_level
      * 3.2.22 on_error
      * 3.2.23 retry
      * 3.2.24 abort
      * 3.2.25 ignore
      * 3.2.26 maxretry
      * 3.2.27 none
  * 4 Example
  * 5 Version
  * 6 See also
json \- Implementation of JSON data exchange with external applications 

## Synopsis
    
    
    module [connection];
    class json {
       link "allow:_object_._property_ [<-|->] _remote_ ";
       link "forbid:_object_._property_ [<-|->] _remote_ ";
       link "[init|precommit|presync|sync|postsync|prenotify|postnotify|commit|finalize|term]:_object_._property_ [<-|->]_remote_ ";
       option "connection:[client|server], udp,readcache 256, writecache 256";
       option "transport:port _number_ , header_version _digit_ , hostname [_hostname_ |_ipv4addr_], debug_level _digit_ , on_error {retry|abort|ignore}, maxretry [_number_ |none]";
    }
    

## Description

The json class implements a connection to external software using the [JSON link protocol]. 

## Class members

### link

The link pseudo-member is used to control access using allow and forbid, and control the event mapping process. 

#### allow

The allow link specifier indicates that a normally prohibited exchange of data should be allowed. This specifier only has an effect for security modes that limit data exchanges. 

#### forbid

The forbid link specifier indicates that a normally allowed exchange of data should be forbidden. his specifier only has an effect for security modes that permit data exchanges. 

### option

The option pseudo-member is used to control connection layer and transport layer options. 

#### init

The init option indicates the data elements are to exchange during an [INIT] event. 

#### precommit

**TODO**: 

#### presync

**TODO**: 

#### sync

**TODO**: 

#### postsync

**TODO**: 

#### prenotify

**TODO**: 

#### postnotify

**TODO**: 

#### commit

**TODO**: 

#### finalize

**TODO**: 

#### term

**TODO**: 

#### connection

**TODO**: 

#### client

**TODO**: 

#### server

**TODO**: 

#### udp

**TODO**: 

#### readcache

**TODO**: 

#### writecache

**TODO**: 

#### transport

**TODO**: 

#### port

**TODO**: 

#### header_version

**TODO**: 

#### hostname

**TODO**: 

#### debug_level

**TODO**: 

#### on_error

**TODO**: 

#### retry

**TODO**: 

#### abort

**TODO**: 

#### ignore

**TODO**: 

#### maxretry

**TODO**: 

#### none

**TODO**: 

## Example
    
    
    module [connection];
    object json {
       link "init:my.x-> var1";
       link "init:my.y <- var2";
       link "sync:my.x-> var1";
       link "sync:my.y <- var2";
       option "connection:client,udp";
       option "transport:hostname localhost, timeout 1000, on_error retry, maxretry none";
    }
    

## Version

The [connection] module is proposed for [Hatwai (Version 3.1)]. Its development is tracked under [Ticket 797](http://sourceforge.net/p/gridlab-d/tickets/797). 

## See also

  * [Connection] classes 
    * [xml]
    * json
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
  * [JSON link protocol]

