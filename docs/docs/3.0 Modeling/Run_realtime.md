# Run realtime - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Run_realtime
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Examples
    * 3.1 GLM
    * 3.2 Command line
    * 3.3 Module API
  * 4 Bugs
  * 5 Version
  * 6 See also
## Synopsis

run_realtime \- Real-time clock control global variable 

## Description

The run_realtimeglobal variable controls whether and at what rate the simulation runs at a rate coupled to the wall clock. When non-zero, the simulation clock runs at a rate of run_realtime seconds per second (i.e., a value of 2 causes the simulation to run twice as fast as the realtime clock. 

## Examples

### GLM

To set the realtime simulation rate at 1 second/second 
    
    
    #set run_realtime=1
    

To set disable the realtime simulation rate 
    
    
    #set run_realtime=0
    

To display the realtime rate during GLM loading 
    
    
    #print ${run_realtime}
    

To test the realtime rate during GLM loading 
    
    
    #if ${run_realtime}>1
    #error cannot run faster than realtime
    #endif
    

### Command line

To set the realtime rate at the command line 
    
    
    host% gridlabd -D run_realtime=1 _myfile_.glm
    

### Module API

Prior to [Hassayampa (Version 3.0)]

To get the realtime rate 
    
    
    char1024 buffer;
    int32 value;
    [gl_global_getvar]("run_realtime",buffer,sizeof(buffer));
    value = atoi(buffer);
    

To set the realtime rate 
    
    
    char1024 buffer;
    int32 value = 1;
    sprintf(buffer,"%d",value);
    [gl_global_setvar]("run_realtime",buffer,sizeof(buffer));
    

As of [Hassayampa (Version 3.0)]

To get the realtime rate 
    
    
    [gld_global] run_realtime("run_realtime");
    int32 value = run_realtime.get_int32();
    

To write to a string 
    
    
    [gld_global] run_realtime("run_realtime");
    char1024 value;
    run_realtime.to_string(value,sizeof(value));
    

To set the realtime rate 
    
    
    [gld_global] run_realtime("run_realtime");
    int32 value = 1;
    run_realtime.set(value);
    

To read from a string 
    
    
    [gld_global] run_realtime("run_realtime");
    char1024 value = "1";
    run_realtime.from_string(value);
    

## Bugs

As of [Hassayampa (Version 3.0)] run_realtime cannot be used when [subsecond] update is active. 

## Version

The run_realtime variable was introduced in [Diablo (Version 2.0)]. 

## See also

  * [Global variables]
    * clock
    * [server]
  * [gld_global]

