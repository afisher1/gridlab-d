# Eclipse - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Eclipse

---
 
 



## Contents

  * 1 Step 1: GLM File Association
  * 2 Step 2: External Tool Configuration
  * 3 Step 3: Create a project
  * 4 Step 4: Create GLM files
  * 5 Step 5: Run the simulation
Eclipse \-- Using Eclipse as your GridLAB-D integrated modeling environment 

This page describes how to set up Eclipse as your integrated modeling environment. It is based on the Eclipse C/C++ IDE. 

## Step 1: GLM File Association

[![](//images.shoutwiki.com/gridlab-d/thumb/b/bf/Eclipse_step_1.png/300px-Eclipse_step_1.png)](/wiki/File:Eclipse_step_1.png)

[]

Step 1. GLM File Association

Open **Eclipse - > Preferences** and add an editor file type association for GLM files to the internal text editor. 

  


## Step 2: External Tool Configuration

[![](//images.shoutwiki.com/gridlab-d/thumb/7/78/Eclipse_step_2.png/300px-Eclipse_step_2.png)](/wiki/File:Eclipse_step_2.png)

[]

Step 2. External Tool Configuration

Open **Run - > External Tools -> External Tools Configuration** and clock the **New Launch Configuration** icon (in the upper left corner of the explorer pane). Enter the following values in the **Main** tab: 

Location
    `_your-gridlabd-folder_ /bin/gridlabd.bin`
Working Directory
    `${project_loc}`
Arguments
    `${project_name}.glm`

Click **Apply** and **Close**. 

Select the **Environment** tab and add the following environment variables to set 
    
    
    GRIDLABD=${env_var:HOME}/gridlabd
    GLPATH=${env_var:HOME}/gridlabd/lib/gridlabd:${env_var:HOME}/gridlabd/share/gridlabd
    CXXFLAGS=-w -g -O0
    

## Step 3: Create a project

[![](//images.shoutwiki.com/gridlab-d/thumb/3/39/Eclipse_step_3.png/300px-Eclipse_step_3.png)](/wiki/File:Eclipse_step_3.png)

[]

Step 3. Create Project

Open **File - > New Project**, select **General / Project** , and click **Next**. Specify the project name and the project folder. Click **Finish**. 

## Step 4: Create GLM files

[![](//images.shoutwiki.com/gridlab-d/thumb/c/c0/Eclipse_step_4.png/300px-Eclipse_step_4.png)](/wiki/File:Eclipse_step_4.png)

[]

Step 4. Create GLM File

If your project folder already contains files, they will be listed in the project's explorer pane. To create a new GLM file, open **File - > New -> Other**, select **General / File** , and click **Next**. In the **New File** dialog, select the project, enter the GLM file name, and click **Finish**. 

## Step 5: Run the simulation

Once you have coded the GLM file(s), open **Run - > External Tools -> GridLAB-D Modeler**, and observe the output in the console window. You can also click the **Run GridLAB-D Modeler** icon on the toolbar to start GridLAB-D. 

Warnings and errors are displayed first in the output. Then the standard output is displayed. You can use the file name and line number to locate the cause of any warnings or errors. 

By default the run configuration uses the project name as the GLM file name. You can change this behavior using the **External Tools Configuration** dialog in Step 2. You can also change the command line arguments, adding options such as `--verbose` or `--profile` to the arguments. 


