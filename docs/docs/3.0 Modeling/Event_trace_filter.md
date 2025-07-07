# Event trace filter - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Event_trace_filter
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 See also
event_trace_filter \- Event trace control variable [Template:NEW30]

## Synopsis

Command line
    
    
    host% gridlabd -D|--define event_trace_filter=_prefix_
    

GLM
    
    
    #set event_trace=_prefix_
    

## Description

The event_trace_filter limits the [event_trace] output to only those objects whose prefix matches _prefix_. An empty prefix matches all objects. 

## See also

  * [event_trace]
  * [Global variables]
  * [Theory of operation]

