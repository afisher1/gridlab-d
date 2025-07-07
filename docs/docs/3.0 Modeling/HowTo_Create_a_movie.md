# HowTo:Create a movie - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/HowTo:Create_a_movie
[Template:NEW30] This **[How To]** describe how to create a movie using [Matlab®](http://www.mathworks.com)'s [avifile command](http://www.mathworks.com/help/techdoc/ref/avifile.html) command. 

## System requirements

[Hassayampa (Version 3.0)]
    You must be using a version of GridLAB-D that support the [link directive].
[Matlab link]
    You must have a version of [Matlab](http://www.mathworks.com) that supports the [avifile command](http://www.mathworks.com/help/techdoc/ref/avifile.html).

Suppose you have a GLM file named _my_avi.glm_ that defines a trivial object _test_ as follows: 
    
    
    clock {
      starttime '2001-01-01 0:00:00';
      stoptime '2001-01-02 0:00:00';
    }
    class test {
      [random] x;
    }
    object test {
      [name] my_test;
      x "[type]:[normal](0,1); [refresh]:1[min]; [integrate]";
    }
    

## Step 1 - Link GLM to Matlab using a link control file

Add the [link directive] to your GLM file: 
    
    
    [link] matlab_avi.link;
    

## Step 2 - Create the link control file

Create the link control file using the following command 
    
    
    [target] [matlab]
    [[matlab link|workdir avi
    [[matlab link|window show
    [[matlab link|output 100000
    [global]
    object
    [export] my_test.x x
    [on_init] q=zeros(1000,1); n=1; v=avifile('my.avi'); if(exist('v')) h=figure(1); xlim([1 1000]); ylim([-100 100]); ans=GLD_OK; else ans=GLD_ERROR; end;
    [on_sync] if(n<1000) q(n)=x; plot(1:n,q(1:n); v=addframe(v,getframe(h)); n=n+1; end; ans=TS_NEVER;
    [on_term] close(v); ans=GLD_OK;
    

## Step 3 - Run GridLAB-D
    
    
    C:\GridLAB-D> gridlabd my_avi.glm
    ... perhaps some output is displayed
    C:\GridALB-D>
    

## Step 4 - View the AVI file

Open a file explorer, go the **avi** folder, and double-click on the AVI file. 

## See also

  * [Link (directive)]
    * [Matlab link]
    * [JSON link] [Template:NEW30]
    * [Technical manual]
  * **[How To]**
    * [How to plot data using Matlab]
    * How to create a movie

