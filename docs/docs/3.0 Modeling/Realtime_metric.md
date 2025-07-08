# Realtime metric

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Realtime_metric
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 History
  * 4 See also
realtime_metric \-- Realtime performance metric 

## Synopsis
    
    
    host% wget http://_server_ :_portnum_ /_protocol_ /realtime_metric
    

## Description

The realtime_metric [global variable] is used to monitor the performance of the realtime simulation. The value of the metric is calculated using the infinite impulse response (IIR) filter $ realtime\_metric = 0.9 realtime\_metric + 0.1 (1 - t_{update}) $ where $t_{update}$ is the time required to make a single 1 second update of the simulation. This value is updated every second. The IIR filter has unit step response of about 30 seconds to reach 95% of the steady state value. 

A value near 1 indicates that the simulation has plenty of spare time to complete each update. A value near 0 indicates that the simulation is very little time available to complete each update. 

## History

The realtime_metric was introduced in <https://sourceforge.net/p/gridlab-d/code/5606>. 

## See also

  * [run_realtime]
  * [globals]

