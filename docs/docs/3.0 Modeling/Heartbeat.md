# Heartbeat

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Heartbeat
Heartbeat \- Object heartbeat control [Template:NEW30]

## Synopsis
    
    
    object _class_ {
      heartbeat _time-modulo_ ;
    }
    

## Description

The object heartbeat determines the number of seconds that elapse between calls to the **heartbeat__class_()** export function. If the object heartbeat is zero or if the object's class does not export the heartbeat function, the heartbeat is not called. By default the object heartbeat is zero. 

When the heartbeat function is called, the object has the opportunity to set the time of the next object update, which is by default [NEVER]. 

The purpose of the heartbeat is to allow objects to preschedule sync times without relying on the sync call loop to ensure that the next sync time gets scheduled. The prototypical object with this problem is the recorder, which sometimes must schedule syncs to occur every _n_ seconds. Prior to [Hassayampa (Version 3.0)] this could not be done and it was necessary for sync calls to anticipate the next event, which would cause all objects to be updated with sync calls even when it's obvious that nothing needs to be or should be changed. See [GridLAB-D Forum Topic 5227211](https://sourceforge.net/projects/gridlab-d/forums/forum/842561/topic/5227211) for more information. 

## Version

The heartbeat property was introduced in [Hassayampa (Version 3.0)]. 

## See also

  * [Object (directive)]

