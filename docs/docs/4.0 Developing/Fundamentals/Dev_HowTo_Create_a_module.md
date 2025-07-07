# Dev:HowTo:Create a module - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:HowTo:Create_a_module
*** WORKING DRAFT ***   
**Please review, edit and[comment] as necessary**.

* * *

Dev:HowTo:Create a module \- Developer's guide to creating a new module [Template:NEW30]

## Contents

  * 1 Overview
    * 1.1 External Components
  * 2 Step-by-step Guides
    * 2.1 Step 1 - Create a ticket
    * 2.2 Step 2 - Create a working folder
    * 2.3 Step 3 - Create the module folder
    * 2.4 Step 4 - Add the module
    * 2.5 Step 5 - Add the files
    * 2.6 Step 6 - Set the compile and link options
    * 2.7 Step 7 - Compile, debug and test
    * 2.8 Step 8 - Commit working code
    * 2.9 Step 9 - Write wiki docs
    * 2.10 Step 10 - Validate
    * 2.11 Step 11 - Notify project
  * 3 See also
Developers can create new modules in GridLAB-D. This [How To] guide provides step-by-step instructions for creating a module that will run on all the supported [platforms]. 

## Overview

The process is generally as follows: 

  1. Create a ticket to describe and track the status of the enhancement the module provides
  2. Create a working folder for the existing GridLAB-D code
  3. Create a module folder for the new module code
  4. Add the module to the solution files (varies by platform)
  5. Add the files for `main.cpp`, class implementation files, and other support files
  6. Set the compile and link options
  7. Compile, debug and test
  8. Commit working code to the repository
  9. Write wiki docs
  10. Validate across all platforms
  11. Notify project module is ready for release
### External Components

Some modules require external components that are licenses separately from GridLAB-D. GridLAB-D allows different methods to include external components, depending on the license under which the component is distributed. As a general rule, any public domain or BSD-style license code may be included in GridLAB-D. Any other type of license is prohibited, include GPL and LGPL licenses. 

The general guidelines are as follows: 

Public domain
    These may be included directly in the source code of GridLAB-D with not restrictions. These codes may be combined directly and/or intermingled with GridLAB-D sources without restrictions. It is strongly recommended that recognition of all public domain contributions be given for both technical and ethical reasons.

BSD-style and other open-source licenses that do not include "contamination" clauses
    These codes may be included directly in the source code of GridLAB-D under the license terms. These codes should be included in separate source files with GridLAB-D sources in the `third_party` folder.

Open-source licenses that include "contamination" clauses (e.g., GPL, LGPL)
    These codes may only be included in library form provided doing so does not contaminate GridLAB-D's BSD-style license with new license requirements. A `tools` repository folder is provided for all pre-compiled products that are optionally used by GridLAB-D. The installation of these codes on a user's system is done separately because they require separate user consent to the product's license agreement. GridLAB-D's installation program may offer to install some of these codes.

Proprietary code
    Proprietary code may not be used in GridLAB-D unless specific written permission by the copyright holder to release the code into the open-source under GridLAB-D's BSD-style license. As the developer you are responsible for obtaining this permission and you should be prepared to demonstrate that you have obtained the copyright holder's permission when the [GridLAB-D project team](mailto:gridlabd@pnnl.gov) requests it.

Your code
    If you are the author of the code you must use one of the above methods. We recommend using the GridLAB-D license, which is a BSD-style license.

If you have any questions or concerns, please contact the [GridLAB-D project team](mailto:gridlabd@pnnl.gov). 

## Step-by-step Guides

In these guides the following conventions and variables are used for Linux/MacOSX users 

SVN
    This is the SVN repository URL. You can set this using the command
    
    
    host% **export SVN=<https://gridlab-d.svn.sourceforge.net/svnroot/gridlab-d>**
    

WD
    This is the working directory. You can set this using the command
    
    
    host% **cd _workdir_**
    host% **export WD=$(pwd)**
    

_module-name_
    A module name is a single complete lowercase word with no special characters, hyphens, underscores or digits. Do not use abbreviations, acronyms, etc. unless there is a compelling reason to do so. The name should be descriptive, e.g., an action, a process, a group, a concepts, or a product. Some examples of existing modules are:
Module naming convention  Action | Process | Grouping | Concept | Product   
---|---|---|---|---  
assert  
optimize  
| control  
debug  
market  | commercial  
generators  
network  | climate  
powerflow  
reliability  | mysql  
tape  
  
  
_class-name_
    A class name is a simple or composite lowercase word with no special characters, digits or hyphens. Underscores are permitted. As a general rule acronyms etc. are not desirable. Class names should be description as a type or category of object with like properties and behaviors. Module may implement zero or more classes.

    You will need permission to access the source code repository, these wiki pages and the TRAC system. Please contact the [GridLAB-D project team](mailto:gridlabd@pnnl.gov) to request access. In your email, be sure to explain why you need access and what you are planning to do.

### Step 1 - Create a ticket

Create a new ticket by opening a [new ticket](http://sourceforge.net/p/gridlab-d/tickets//new). When you submit the ticket, it will be assigned a ticket _id_ , which you will use to manage your work and coordinate with other collaborators and the GridLAB-D project team. 

### Step 2 - Create a working folder

Copy the trunk code into a new working branch: 
    
    
    host% **svn copy $SVN/trunk $SVN/ticket/_id_**
    

where _id_ is the ticket number you received above. 

Check out your working copy of the working branch: 
    
    
    host% **cd $WD**
    host% **svn checkout $SVN/ticket/_id_**
    

### Step 3 - Create the module folder

Create the new module folder 
    
    
    host% **cd $WD**
    host% **mkdir _module-name_**
    

### Step 4 - Add the module

Add the module to the working branch in the source repository 
    
    
    host% **cd $WD**
    host% **svn add _name_**
    

### Step 5 - Add the files

Create your working files. The following files are required 

main.cpp
    This is the main module implementation file.

Makefile.am
    This is the linux/unix makefile.

_module-name_.vcproj
    This is the Visual Studio makefile.

An easy way to start them as empty files is to use the touch command: 
    
    
    host% **cd $WD/_module-name_**
    host% **touch main.cpp Makefile.am _module-name_.vcproj**
    

Add the new files to the repository 
    
    
    host% **cd $WD/_module-name_**
    host% **svn add main.cpp Makefile.am _module-name_.vcproj**
    

For each class, you must create both the `.cpp` and the `.h` files for each class. See below for details on add classes. 

### Step 6 - Set the compile and link options

Linux/Unix systems
    Edit the makefile for linux/unix system and add the following lines:
    
    
    pkglib_LTLIBRARIES = _module-name_.la
    _module-name_ _la_SOURCES = \
           _class-name1_.cpp _class-name1.h \_
           _class-name2_.cpp _class-name2.h \_
              ... add other classes ...
           _class-nameN_.cpp _class-nameN.h \_
           main.cpp 
    _module-name_ _la_LDFLAGS = -module -no-undefined -avoid-version -version-info 1:0:0
    
    uninstall-hook:
           -rmdir $(DESTDIR)$(pkglibdir)
    

### Step 7 - Compile, debug and test

**TODO**: 

### Step 8 - Commit working code

You need to commit your working code if you are collaborating with others or you wish to share your progress, obtain assistance, and update the project team. When you are done, you must commit your code to the project for it to become available to others. 

To commit your code, use the following command: 
    
    
    host% cd **$WD**
    host% svn commit
    ... provide a brief description of your changes ...
    ... read the _changeset number_ now ...
    

When you have obtained the changeset number, open the <http://sourceforge.net/p/gridlab-d/tickets/> ticket for your work and update the status with the _changeset number_. 

### Step 9 - Write wiki docs

**TODO**: 

### Step 10 - Validate

**TODO**: 

### Step 11 - Notify project

**TODO**: 

## See also

  * [Dev:HowTo:Create a module]

