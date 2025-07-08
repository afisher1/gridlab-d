# Engine protocol

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Engine_protocol
**_Proposed for review_**

## Contents

  * 1 UDP Protocol
    * 1.1 INIT
    * 1.2 SYNC
    * 1.3 TERM
  * 2 UDP_SHMEM or UDP_MMAP Protocol
    * 2.1 INIT
    * 2.2 SYNC
    * 2.3 TERM
  * 3 Version
  * 4 See also
engine protocol \- GridLAB-D engine link protocol 

The general engine link protocol is implemented using three general message types to maintain engine cache coherence. 

    INIT : messages to establish and populate the engine cache.
    SYNC : messages to update the engine cache and synchronize the application's clock with GridLAB-D's clock.
    TERM : messages to provide a final update of the engine cache before GridLAB-D is halted.

## UDP Protocol

### INIT

The initialization sequence is completed using the following sequence of messages in GridLAB-D's link implementation. Every sequence of messages must be responded to using a _status_ message as follows: 

OK
    The sequence was processed ok.
ERROR\n _messages_
    The sequence was processed but errors were encountered. The messages may be appended separated by newlines (up to the size of the message). Overflow errors are ignored.
TERM\n _messages_
    The engine link is about to shutdown. The reason may be included.

The first part of the sequence transfer basic information about the GridLAB-D environment and the engine link: 
    
    
    recv: INIT
    send: GRIDLABD _major_._minor_._patch_ -_build_ (_branch_)
    send: PROTOCOL UDP
    send: CACHESIZE _count_ _size_
    send: TIMEOUT _seconds_
    recv: _status_
    

The second part of the initialization sequence sets up the engine data cache: 
    
    
    send: GLOBAL _index_ _type_ _size_ _name_ _value_ (repeated)
    send: IMPORT _index_ _type_ _size_ _name_ _value_ (repeated)
    send: EXPORT _index_ _type_ _size_ _name_ _value_ (repeated)
    send: _status_
    recv: _index_ _value_ (repeated)
    recv: _status_
    

### SYNC

Synchronization messages update the engine data cache: 
    
    
    recv: SYNC _time_
    recv: _index_ _value_ (repeated 0-N times)
    recv: _status_
    send: _index_ _value_ (repeated 0-N times)
    send: _status_
    

### TERM

If termination is initiated by GridLAB-D 
    
    
    recv: SYNC _time_
    recv: _index_ _value_ (repeated 0-N times)
    recv: _status_
    send: _index_ _value_ (repeated 0-N times)
    send: TERM
    

or if termination is initiative by application 
    
    
    recv: TERM 
    recv: _index_ _value_ (repeated 0-N times)
    recv: _status_
    send: _index_ _value_ (repeated 0-N times)
    send: TERM
    

## UDP_SHMEM or UDP_MMAP Protocol

The follow protocol is used when SHMEM or MMAP is used. Note that UDP is still used to provide synchronization. But data transfer is completed using share memory to improve performance for larger models. 

### INIT
    
    
    recv: INIT
    send: GRIDLABD _major_._minor_._patch_ -_build_ (_branch_)
    send: PROTOCOL SHMEM -or- MMAP
    send: CACHESIZE _count_ _size_
    send: TIMEOUT _seconds_
    send: GLOBAL _index_ _type_ _size_ _name_ _value_ (repeated)
    send: IMPORT _index_ _type_ _size_ _name_ _value_ (repeated)
    send: EXPORT _index_ _type_ _size_ _name_ _value_ (repeated)
    send: _status_
    

### SYNC
    
    
    recv: SYNC _time_
    send: _status_
    

### TERM

If termination is initiated by GridLAB-D: 
    
    
    recv SYNC _time_
    send TERM
    

or if termination is initiated by application: 
    
    
    recv TERM
    send _status_
    

## Version

The [engine link] was introduced in [Navajo (trunk)]. 

## See also

  * [glengine]
  * [engine link]
  * engine protocol

