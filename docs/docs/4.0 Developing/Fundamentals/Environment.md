# Environment

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Environment
# Environment

environment \- Control the operating environment for GridLAB-D 

# 

Synopsis

Command line
    
    
    gridlabd -e {batch|gui|html|matlab|server|X11}
    gridlabd --environment {batch|gui|html|matlab|server|X11}
    

GLM file
    
    
    #set environment={batch|gui|html|matlab|server|X11}
    

# 

Description

When GridLAB-D starts it evaluates the environment [global variable] to determine the simulation's operating environment. Depending on the value of the variable, different start-up procedures are followed. 

## Batch

The [batch environment] is the default operating environment for GridLAB-D. This is a stand-alone command-line based operating mode when no [GUI directives] are found in the loaded GLM files. In batch mode, GridLAB-D simple loads the GLM files and run the simulation to steady state or stoptime with output directed by default to the [stdout] and [stderr] output streams. 

If [GUI directives] are found and the operating environment is batch, the operating is automatically switched to [GUI]. 

## GUI

The [GUI environment] is used when [GUI directives] are present in any of the loaded GLM files. When operating in GUI model, GridLAB-d start a [server] instance and a [browser] instance that connects to it. 

If there are not [GUI directives] present or either the [server] or [browser] processes fail to start properly, then GridLAB-D will fail to start properly. 

[GUI environment] is in development as of [Navajo (trunk)]. 

## HTML

The [HTML environment] is used to force GridLAB-D to parse any [GUI directives] in the loaded GLM files and output them to [stdout]. When this process is completed, GridLAB-D exits without starting the simulation. 

[HTML environment] is in development as of [Navajo (trunk)]. 

## Matlab

The [Matlab environment] is used to start GridLAB-D as a Matlab CMEX module. Note that the CMEX module is not part of the standard GridLAB-D build. Any system using Matlab must have a license copy of Matlab installed. 

[Matlab environment] is not fully supported as of [Navajo (Version 4.3)]. 

## Server

The [Server environment] is used to start GridLAB-D as a HTTP/1.1 server. 

## X11

The [X11 environment] is used to start GridLAB-D as an X11 server. 

[X11 environment] is stub code as [Navajo (trunk)] and is not included in [Navajo (Version 4.3)] or [Ostrander (Version 5.0)]. 

# 

See also

  * Environment
    * [Batch]
    * [GUI]
    * [HTML]
    * [Matlab]
    * [Server]
    * [X11]
  * [Global variables]
