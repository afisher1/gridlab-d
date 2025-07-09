# Skipsafe

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Skipsafe
# Skipsafe

Certain objects permit the clock to advance more quickly if possible and avoid calling unnecessary time synchronization events. The skipsafe [global variable] controls for how much time (in seconds) it is safe for the clock to skip when synchronizing objects. 

The default value of skipsafe is 0, meaning that it is not safe to skip any time when advancing the clock. 

# 

GLM

To set the safe clock skipping time to 1 minute, use the syntax 
    
    
    #set skipsafe=60
    

# 

Command line

To set the safe clock skipping time to 1 minute, use the syntax 
    
    
    host% gridlabd -D skipsafe=60
    

# 

See also

  * clock
  * [Global variables]
