# Mainloop state

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Mainloop_state
# Mainloop state

The global variable **mainloop_state** is used to control the main time synchronization loop in GridLAB-D's core. The main loop state can take the follow values: 

## INIT

This state indicates that the main loop has not yet completed initialization. Sometimes, initialization can take a long time, particularly if the GLM file contains complex schedules that need to be compiled. While in this state, the clock is typically set to **INIT** also. 

## RUNNING

This state indicates that the main loop is running. The clock should be expected to change as it advanced. 

## PAUSED

This state indicates that the main loop is paused. This should only occur if a) [server] mode is active, b) the [pauseat] time has been specified, and c) the clock has reached the [pauseat] time. The simulation can only be resumed by setting a new [pauseat] value using either the appropriate [control] message or directly setting the [pauseat] value to a future date/time. 

## DONE

This state indicates that the main loop has completed. This occur when the simulation is either at steady state or the stoptime has been reached. 

## LOCKED

This state indicates that the main loop is stuck because of a lock. This should never happen. If it does, then it is more than likely caused by an error that results in a [deadlock](http://en.wikipedia.org/wiki/Deadlock). 

## See also

  * clock
  * [control]
  * [pauseat]
  * [deadlock](http://en.wikipedia.org/wiki/Deadlock)
