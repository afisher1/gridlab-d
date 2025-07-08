# Building and Releasing GridLAB-D

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Building_and_Releasing_GridLAB-D
The following is an enumeration of the build and release process for the Gridlab-D project. This outline will be flushed out at a later date, and is the basis for the automated script currently being developed. 

  1. Update working directory to latest version of source.
  2. Perform a release build from within Visual Studio
  3. Build installer
  4. Install Gridlab-D using installer
  5. Run released models to ensure the build works
  6. Create a Linux release build in working directory
  7. Create tag using the build number from the linux release
  8. Subtract the bits that shouldn't be in the tag (proprietary, experimental, unreleased) directly from the repository
  9. Checkout tag to new working directory
  10. Create Linux build using cygwin (updates the version.h file in windows tree)
  11. Update the build numbers in the windows installer files
  12. Build a release build using Visual Studio
  13. Build windows installer
  14. Install from installer, run released models
  15. Check-in changes to the tag (version.h, installer files)
  16. Place installers on Development website for testing, update links
  17. After testing is complete, copy download directory on Development website to Production
Some things to note: 

  * The release working directory is created as described to ensure that history information and a baseline revision are carried over to the tagged source, allowing us to be able to merge from trunk if necessary (though, this shouldn't be needed the option is still open).
  * Subtract the items that aren't going to be released because this is fewer steps in a manual process. There is a downside to the subtractive approach, in that until you remove the unreleased items, it is possible for a third party to download them. That is why the subtraction step is immediately after the creation of the tag. For the script, we should use an additive approach to creating the tag which would eliminate this issue altogether.
  * Although it seems that a number of the steps are repetitive, we don't want to create a tag for something that doesn't at least pass basic tests, and the later testing validates not only the Gridlab-D executable, but ensures that a viable release can be made from the tagged source.

Table 1 - Windows installation structure  Environment variable | Description | Default path | Files installed   
---|---|---|---  
GRIDLABD | Main GridLAB-D installation | C:\Program Files\GridLAB-D |   
PATH | Executable and utility scripts | ${GRIDLABD}\bin | *.exe *.bat   
GLPATH | Libraries | ${GRIDLABD}\lib | *.dll *.py *.java   
GLPATH | Support files | ${GRIDLABD}\etc | *.txt *.shape   
GLPATH | Runtime files | ${GRIDLABD}\rt | *.conf *.h   
GLPATH | Climate files | ${GRIDLABD}\tmy | *.tmy2   
(na) | GridLAB-D sample models | ${GRIDLABD}\samples | *.glm   
PATH | MinGW executables | C:\MinGW\bin | *.exe   
INCLUDE | MinGW include files | C:\MinGW\include | *.h   
LIB | MinGW libraries | C:\MinGW\lib | *.lib   
PATH | Gnuplot executables | C:\GnuPlot\bin | *.exe   
  
  


Table 2 - Linux/Mac OS X installation structure  Environment variable | Description | Default path | Files installed   
---|---|---|---  
GRIDLABD | Main GridLAB-D installation | /usr/etc/gridlabd |   
PATH | Executable and utility scripts | /usr/bin | gridlabd *.py *.java   
GLPATH | Libraries | /usr/lib/gridlabd | *.{so,dylib}   
GLPATH | Support files | ${GRIDLABD} | *.txt *.shape   
GLPATH | Runtime files | ${GRIDLABD}/rt | *.conf *.h   
GLPATH | Climate files | ${GRIDLABD}/tmy | *.tmy2   
(na) | GridLAB-D sample models | ${GRIDLABD}/samples | *.glm   
  

