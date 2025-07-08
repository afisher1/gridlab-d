# GLM Loading FAQ

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/GLM_Loading_FAQ
# Silent load failures

Gridlab-D can find the file I'm trying to load, it even says how large the file is with --[verbose]. However, it immediately fails without a line number, and says that the file cannot be found! What's going on?
    Check what [version] of Gridlab-D you're using. If the [version] is [Coulee (Version 1.2)] or older, C-style double-slash [comments] will not be supported.

# Can't find g++

On Windows, I'm trying to run a GLM file with inline classes, but Gridlab-D says that it can't find g++. What's g++ and how do I get Gridlab-D to find it?
    g++ is a component of MinGW that Gridlab-D uses to compile inline code prior to loading them. First make sure that MinGW is installed. Second, make sure that either the user [environment variables] (My Computer [right click] -> Properties -> Advanced -> Environment Variables) to include "PATH" with the value of the MinGW bin directory (likely c:\MinGW\bin ), or use #setenv path=c:\MinGW\bin within your GLM file.

# Can't find rt/gridlabd.h

I'm using inline classes in my GLM file, and it can't load the file because Gridlab-D can't find the file "rt/gridlabd.h", and then gives me a big string of errors about things not being found or not being defined. How do I fix this?
    You need to add the [GLM macro] `#define include="c:\Program Files\gridlab-d` (or whether path you installed Gridlab-D to) to your GLM files. This will set the correct `-I` options for `g++<t/tt> when it compiles your code. Make sure that there is an `rt` folder in the Gridlab-D install directory that contains `gridlabd.h`. If it's missing, manually add the `rt` folder and download `rt/gridlabd.h` from the SVN repository.`


