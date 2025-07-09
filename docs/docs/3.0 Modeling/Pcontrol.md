# Pcontrol

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Pcontrol
# Pcontrol

pcontrol \- Interactive process control [Template:NEW30]

## Synopsis
    
    
    gridlabd --pcontrol
    

## Remarks

The process control window is a continuous interactive screen that shows all the current GridLAB-D simulations active on the local host. The display is typically as follows: 
    
    
    GridLAB-D Process Control - Version 3.0.0-4595 (Hassayampa)
    
    PROC PID   RUNTIME    STATE   CLOCK                   MODEL
    ---- ----- ---------- ------- ----------------------- -----------------------------------------------------------------
    **0 72169        34s Running 2000-01-03 22:29:49 UTC /Users/david/gridlabd_3.0/core ... test_groupid/test_groupid.glm**
       1 72563         6s Running 2000-01-01 15:53:27 UTC /Users/david/gridlabd_3.0/core ... test_groupid/test_groupid.glm
       2 72698         4s Running 2000-01-21 06:57:28 UTC /Users/david/gridlabd_3.0/comm ... ial_Qi/test_commercial_Qi.glm
       3   -
       4   -
       5   -
       6   -
       7   -
    -----------------------------------------------------------------------------------------------------------------------
    
    2012/08/19 15:36:54: Ready.
    C to clear defunct, Up/Down to select, K to kill, Q to quit: 
    

The columns in the listing are relative self-explanatory: 

  * PROC is the GridLAB-D process map entry number. It is assumed that there can be no more than one entry per CPU available on the host.
  * PID is the host process id. This is used to send signals to the process.
  * RUNTIME is the elapsed wall clock time since the simulation started.
  * STATE is the state of the simulation. Possible states are
    

Init
    Initialization in progress.
Running
    Run in progress.
Paused
    Paused for user or server I/O.
Done
    Simulation completed.
Locked
    Simulation locked for synchronization or event.
Defunct
    Simulation no longer running but did not complete normally. These can be cleared using the 'C' key.

  * CLOCK is the current simulation clock time. All clock times are presented in UTC regardless of the timezone used by the simulation.
  * MODEL is the name of the model
The bolded entry is the selected entry. The up and down arrow keys allows the selection to be changed. Pressing the 'K' key kills the selected entry. 'C' will clear the process list of any defunct simulations that could be left from unexpected crashes. The 'Q' key will quit. The 'Ctrl-C' button will also quit. 

## Bugs

The screen refreshes every second and this cannot be changed. 

Because the process map is sampled once every second, it is quite likely that a quick simulation might never show up on the list. 

## Version

Process control was introduced in [Hassayampa (Version 3.0)]. 

## See also

[Xref:process]


  
