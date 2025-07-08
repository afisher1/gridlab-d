# Mt profile

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Mt_profile
# Mt profile

The mt_profile option produces [profiler] output from GridLAB-D for multithreaded operation. It is managed by the [global variable] **mt_profile** , which is by default 0. The value of the mt_profile determine the maximum number of thread to analyse. 

# 

GLM

To enable profile mode use the directive 
    
    
    #set [mt_analysis]=64
    

To disable profile mode use the directive 
    
    
    #set [mt_analysis]=0
    

# 

Command line

To toggle profile mode use the option 
    
    
    host% gridlabd --mt_profile 64
    

To enable profile mode use the option 
    
    
    host% gridlabd -D [mt_analysis]=64
    

To disable profile mode use the option 
    
    
    host% gridlabd -D [mt_analysis]=0
    

# 

See also

  * [Command options]
  * [Global variables]
  * [Profiler]
