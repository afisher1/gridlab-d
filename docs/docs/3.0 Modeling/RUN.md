# RUN

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/RUN
RUN \- Global variable to get a unique run identifier 

## Synopsis
    
    
    ${RUN}
    

## Description

The RUN [global variable] dynamically generates a unique 128-bit identifier that is generated the first time it is referenced. This can be used to generate object names, file names, and database entities that are unique for a single run. 

## Example

The following code defines a class _test_ with a random variable _x_. The name of the object is unique and shared in the current run. 
    
    
    class test {
      random x;
    }
    object test {
      name test-${RUN};
      x "type:normal(0,1); refresh:1min";
    }
    

## Bugs

The random number generated is seeded using the current system time with a resolution of 1 second. Consequently, if two runs are started within the same second they are very likely to generate the same sequence of unique ids. 

## Version

The unique run id was introduced in [Hassayampa (Version 3.0)]. 

## See also

  * [Global variables]
  * [GUID]
  * [NOW]

