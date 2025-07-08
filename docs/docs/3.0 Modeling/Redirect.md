# Redirect

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Redirect
redirect \- Stream redirection command line options 

## Synopsis
    
    
    host% **gridlabd --redirect _stream_ :_file_**
    

## Remarks

The --redirect [command option] is used to instruct GridLAB-D to redirect one of the output message stream to a file. The following stream may be redirected 

output
    The default stream is `gridlabd.out`

error
    The default stream is `gridlabd.err`

warning
    The default stream is `gridlabd.wrn`

debug
    The default stream is `gridlabd.dbg`

verbose
    The default stream is `gridlabd.inf`

profile
    The default stream is `gridlabd.pro`

progress
    The default stream is `gridlabd.prg`

The special term **all** may be specified, in which case all streams are directed to their default output files. 

The special term **none** may be specified to direct [validate] to not redirect any output to files. 

## Examples
    
    
    host% **gridlabd --redirect all**
    
    
    
    host% **gridlabd --redirect output:outfile.txt**
    

## See also

  * [Command options]

