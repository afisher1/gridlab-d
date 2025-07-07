---
title: Installing GridLAB-D™
excerpt: ''
deprecated: false
hidden: false
metadata:
  robots: index
---
# Installing GridLAB-D™
So, we start at the beginning and for GridLAB-D™ the beginning is getting the software up and running on your computer. As with many open source projects, there are two basic options for doing so: using a traditional installer or compiling the software yourself. We’ll look at both methods.

**TODO**:

Installing pre-built binaries

* Validating installation

Building from source

* Describe why you might want to do this

## Using the Installer

Assuming you’re on one of the supported platforms for GridLAB-D™, using the installer of the latest version is by far the easiest method. Head to the [GridLAB-D™ code repository](https://sourceforge.net/projects/gridlab-d/) and right there at the top of the screen should be a green button which will download the installer for your platform. (If you are downloading the installer for a platform different than the one that you're currently using, click on the “Browse all files”, “Gridlab-D”, “last stable version” and choose the appropriate installer.)

![SF installer download](/images/SF_installer_download.png)

Once the installer has been downloaded, launch it and walk through the process of installing the software as you would for any other commercial software.

Even if you never plan on building or modifying the source code, it can be very helpful to have a copy readily available to consult if you need to understand the details of how a particular portion of GridLAB-D™ is operating. For this reason, it is suggested (perhaps even recommeneded) that you download a copy of the source code for the version of GridLAB-D™ just installed. Head to the [GridLAB-D™ code repository](https://sourceforge.net/p/gridlab-d/code/HEAD/tree/branch/) and click the link of the appropriate version. Towards the top of the page should be a link to “Download Snapshot”.

![Download snapshot](/images\Download_snapshot.png)

This will create a .zip file with the source code which you can save somewhere for reference. Of course, if you want to go the full-nerd route and build the software locally, read on.

## Building from Source

There are a couple reasons that building/compiling the software directly from the source code might be the route you need to go:

* You are running on a platform for which GridLAB-D™ doesn’t have an installer.

* You want a newer version than the latest stable release (maybe it has a new feature you want to try)

* You plan on modifying or extending the existing code and will need to build your custom version anyway.

If you head this route it is assumed you have an understanding and familiarity with how software is typically built and can handle things like version control and environmental variables. If these are new concepts to you, fear not; the internet is full of helpful information that will help you get up to speed, it will just take a bit more time and effort.
# Download

Browse [programs and course materials](http://sourceforge.net/projects/gridlab-d/files) available from sourceforge, or skip straight to [download the latest](http://sourceforge.net/project/platformdownload.php?group_id=233096) version of GridLAB-D. 

The latest source code is available [via SVN](http://sourceforge.net/scm/?type=svn&group_id=233096). For details on how to build the GridLAB-D, see [Builds]. 

### Windows

GridLAB-D has been prepared and tested for Windows XP, Vista, and 7. All daily build tests are performed on Windows 7. Although it hasn't been tested extensively, Windows 8 and 10 have shown no issues with backwards compatibility. 

Installation is from an executable, with a name of the form `gridlabd-_system_ -_major_ __minor_ __build_.exe`. The current values are: 

  * `_system_` represents the machine architecture, which can be 
    * **win32** for 32 bit Intel 686-based systems, or
    * **x64** for 64 bit Intel Itanium-based systems.
  * `_major_` represents the major release number, which is currently **2**.
  * `_minor_` represents the minor release number, which is currently **1**.
  * `_build_` represents the build release number, which changes with each build and usually in the 100s or 1000s.

#### File locations

The default directory tree for GridLAB-D on Windows is as follows: 

c:\Program Files\GridLAB-D
    This is the main GridLAB-D directory. It contains all the subdirectories as well as the _readme_ and _uninstall_ files.
c:\Program Files\GridLAB-D\bin
    This contains all the executables. The **PATH** environment variable should include this directory.
c:\Program Files\GridLAB-D\etc
    This contains all the runtime files. The **GLPATH** environment variable should include this directory.
c:\Program Files\GridLAB-D\lib
    This contains all the module files. Both the **PATH** and **GLPATH** environment variables should include this directory.
c:\Program Files\GridLAB-D\samples
    This contains sample data files.

#### Environment variables

Only two environment variables must be set for GridLAB-D to function properly. 

PATH
    The Windows **PATH** environment must include both the **bin** and **lib** directories. Users who want to include their own modules should add the directory that contains them as well, e.g., **C:\Documents and Settings\_user_ \My Documents\_path_** , where _**user**_ is the user's login name, and _**path**_ is the path to their own modules.
GRIDLABD
    This should contain the path where GridLAB-D is installed.
GLPATH
    GridLAB-D uses the **GLPATH** environment to find runtime files and module files. It should include at least the **etc** and **lib** directories. Users who want to include their own modules or runtime files should add the directory that contains them as well, e.g., **C:\Documents and Settings\_user_ \My Documents\_path_** , where _**user**_ is the user's login name, and _**path**_ is the path to their own modules.
GLTEMP
    (optional) Set to the path where temporary files will be stored. If **GLTEMP** is not set, it will be automatically set depending on the environment variables defined in the following order:

  1. _%HOMEDRIVE%%HOMEPATH%\Local Settings\Temp\gridlabd_ if both **HOMEDRIVE** and **HOMEPATH** are defined.
  2. _%TMP%\%USERNAME%\gridlabd_. If **TMP** is not defined, then **TEMP** will be tried in its place. If both **TMP** and **TEMP** are undefined, then _C:\Windows\Temp_ will be used for **%TMP%**.


Note: If the **GLTEMP** directory does not exist and it is required, it and all its parent directories will be created. 

#### Search Order

Searches for GridLAB-D files on **GLPATH** will be performed using the following order: 

  1. Current working directory
  2. Directories in **GLPATH** environment variable in the order listed
  3. GridLAB-D installation directory (usually _C:\Program Files\GridLAB-D_)
  4. etc subdirectory of GridLAB-D install directory (_C:\Program Files\GridLAB-D\etc_)
  5. lib subdirectory of GridLAB-D install directory (_C:\Program Files\GridLAB-D\lib_)
### Linux and OS-X

#### File locations

Prior to [Hassayampa (Version 3.0)] installation on Mac OS/X could only be performed using the build process. Since [Hassayampa (Version 3.0)] a DMG build is available, but the installation file structure is different when using the DMG. 

#### Build Installation

These folders are created by the build process. There is no official DMG installer before [Hassayampa (Version 3.0)]. 

/usr/bin/gridlabd
    A symbolic link to _/usr/lib/gridlabd/gridlabd_ , a bash script that sets up an appropriate environment to run gridlabd. _/usr/lib/gridlabd/gridlabd_ is itself a soft link to gridlab.bin, the actual GridLAB-D binary.
/usr/lib/gridlabd
    Contains all runtime files.
/usr/share/doc/gridlabd
    Contains copyright notice and other documentation.

#### DMG Installation

/usr/local/bin
    Executable folder contains the command line script and the main executable binary image.
/usr/local/lib/gridlabd
    Library folder the modules and basic support files.

Note that the DMG installer does not alter you profile, so you may need to add the path to /usr/local/bin if it is not already included in your command shell path. 

#### Environment variables

GRIDLABD
    This should contain the path where GridLAB-D is installed. The _gridlabd_ script sets this variable before calling _gridlabd.bin_.
GLPATH
    A set of colon-separated paths used for searching for GridLAB-D configuration files, modules, and runtime files. At a minimum, this variable should contain the path to the directory where GridLAB-D is installed (_/usr/lib/gridlabd_). It may also be used to add additional search paths.
GLTEMP
    (optional) Set to the path where temporary files will be stored. If **GLTEMP** is not set, it will be automatically set depending on the environment variables defined in the following order:

  1. _$HOME/.gridlabd/tmp_ if **HOME** is defined.
  2. _$TMP/$USER/gridlabd_. If **TMP** is not defined, then **TEMP** will be tried in its place. If both **TMP** and **TEMP** are undefined, then _/tmp_ will be used for **$TMP**.

Note: If the **GLTEMP** directory does not exist and it is required, it and all its parent directories will be created. 

#### Search Order

Searches for GridLAB-D files on **GLPATH** will be performed using the following order: 

  1. Current working directory
  2. Directories in **GLPATH** environment variable in the order listed
  3. /usr/lib/gridlabd or /usr/local/lib/gridlabd

Note: The last hard-coded path should probably not be hard-coded or at least should be set using the install prefix. 

## Installation Validation

Assuming you’ve been able to get the software installed and all appears well a good next step is to validate that this is the case. The easiest and most basic way is to ask GridLAB-D™ to tell you about itself. From your console/command-line/terminal type:

```
gridlabd --version
```

You should get a one-line response showing which version of the software you’ve installed including the secret version codename and platform on which it will run. If the response is anything else, it is likely an indication that GridLAB-D™ is either not installed or fully configured.

