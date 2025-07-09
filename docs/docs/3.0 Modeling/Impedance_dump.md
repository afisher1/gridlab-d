# Impedance dump

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Impedance_dump
## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Properties
  * 4 Remarks
  * 5 See also
impedance_dump \-- Dump impedance and line equation data from powerflow [link] objects. This is useful for debugging parameters or tracking down issues in the model. 

## Synopsis
    
    
    object impedance_dump {
    	[char256] filename "";
    	[char32] group "";
    	[timestamp] runtime 'INIT';
    	int32 runcount 0;
    }
    

## Description

The impedance_dump object produces a dump of the impedances and line equation information at [link] objects in powerflow. 

The output format of the impedance dump fits the line equation format of William Kersting's "Distribution System Modeling and Analysis" book. I.e., the impedance_dump outputs the $\displaystyle{}a, b, c, d, A, B$ matrices of the equations: 

$$\displaystyle{} \begin{bmatrix} VLG_{in} \\\ I_{in} \end{bmatrix} = \begin{bmatrix} a & b\\\ c & d\end{bmatrix}\cdot{}\begin{bmatrix}VLG_{out} \\\ I_{out}\end{bmatrix}$$

$$\displaystyle{}VLG_{out} = A\cdot{}VLG_{in}-B\cdot{}I_{in}$$

with the $\displaystyle{}b$ matrix typically representing the traditional impedance of a line. 

## Properties

Property name | Type | Description   
---|---|---  
filename | [char256] | XML output file to save the impedance information.   
group | [char32] | Search criteria for objects of interest. See [Finding Object] for syntax. Defaults to "all link objects".   
runtime | [timestamp] | Time to run the impedance dump. By default, this is after the first successful time interval has completed (e.g., [commit] of the first timestep).   
runcount | int32 | Number of times the impedance dump has executed (see Remarks).   
  
## Remarks

Currently, the impedance_dump object will only execute once, at the selected `runtime`. If multiple dumps are desired, a [player] or some other interface would need to update the `runtime` and reset the `runcount` values. Also note that the file in `filename` will be overwritten, so if this route is used, something must externally rename or copy the XML prior to running the next impedance dump. 

  


## See also

[Powerflow module]

[Powerflow User's Guide]


