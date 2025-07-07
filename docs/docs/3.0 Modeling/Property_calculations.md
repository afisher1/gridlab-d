# Property calculations - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Property_calculations
# Property calculations

## Contents

  * 1 Synopsis
  * 2 Expressions
    * 2.1 Math functions
    * 2.2 Variables
  * 3 Example
  * 4 See also
Property calculations \-- Calculating values in property definitions. 

## Synopsis
    
    
    object class {
       property (_expression_);
    }
    

## Expressions

Conventional math equations are supported for any expression placed inside parentheses in a property definition. Expression are evaluated at load time and therefore take place after creation of the object but before initialization. 

### Math functions

The following math functions are supported 
    
    
    sin(_expression_)
    cos(_expression_)
    tan(_expression_)
    abs(_expression_)
    sqrt(_expression_)
    acos(_expression_)
    asin(_expression_)
    atan(_expression_)
    log(_expression_)
    floor(_expression_)
    ceil(_expression_)
    

Note that the code tries to support `log10` but due to a parser limitation it does not actually work. 

### Variables

Variable references take the form 
    
    
    $_variable-name_
    

or 
    
    
    this._variable-name_
    

Only references local properties or global variables are supported and they must already be defined to be used. If the variable is not defined, its creation default value is used. If the calculation ends up setting the property to the creation default value, it most likely will end up assuming the initialization default value. 

## Example
    
    
    clock {
      timezone GMT0GMT;
      starttime '2015-05-20 00:00:00';
      stoptime '2015-05-21 00:00:00';
    }
    module [residential];
    module [tape];
    object [house]:..5 {
      [floor_area] [random].[uniform](1500,2500);
      [air_volume] (10*ceil($[floor_area]+10));
      object recorder {
        property [floor_area],[air_volume];
        file `data_{id}.csv`;
      };
    

} 

## See also

  * property
    * [Parameter expansion]
    * [Expansion variables]
    * [Functional values]
