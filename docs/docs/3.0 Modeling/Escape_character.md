# Escape character - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Escape_character
Escape character \- Prevents GLM parser from interpreting the next character in the usual way 

The escape character in GLM files is the backslash (\\). It is only considered when loading a value, such as 
    
    
    object my_test {
      value "\", \', or \; would stop parsing the value";
    }
    

You can also use the escape character to prevent the macro parser from processor the ${_name_} sequence, such as 
    
    
    #define MYVAR=1
    object my_test {
      value "$\{MYVAR\} could be confused as a macro expansion"; 
    }
    

## Contents

  * 1 Example
  * 2 Version
  * 3 Caveat
  * 4 See also
## Example

A good example of usage of the escape character can be found in the [core mainloop test](http://gridlab-d.svn.sourceforge.net/viewvc/gridlab-d/trunk/core/autotest/test_exec_mainloop.glm). 

## Version

The escape character is supported as of [Hassayampa (Version 3.0)]. 

## Caveat

The escape character does not work while parsing macros, directives, names, blocks, etc. In these cases the \ is left in place and allowed to pass through to the loader. 

MS Windows
    Although the convention in Windows originally is that directory names in paths be delimited using a \, Windows supports / delimiters. Consequently, the convention in GridLAB-D is to use exclusively / for paths. If you must use \ in a path value, then you must escape it by using \\\\.

## See also

  * [GLM syntax]

