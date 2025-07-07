# Modify - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Modify
# Modify

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Example
  * 4 History
  * 5 See also
modify \- GLM directive to modify a property of an existing object 

## Synopsis
    
    
    modify _name_._property_ _value_ ;
    

## Description

The modify directive allows GLM files to modify properties of an existing object after the object has already been created. This is particularly useful when object properties require additional information from a second source but it is not practical to include that information in the original object definitions. 

## Example

In file `model.glm`: 
    
    
    module powerflow;
    #include "objects.glm"
    #include "locations.glm"
    

In file `objects.glm`: 
    
    
    object load {
    	name load_1;
    	groupid nodevolts;
    	phases ABCN;
    	constant_power_A 40000.000000+20000.000000j;
    	nominal_voltage 2401.7771;
    }
    object load {
    	name load_2;
    	groupid nodevolts;
    	phases BN;
    	constant_power_B 20000.000000+10000.000000j;
    	nominal_voltage 2401.7771;
    }
    

In file `locations.glm`: 
    
    
    modify load_1.latitude 35N23.117; modify load_1.longitude 119W0.132;
    modify load_2.latitude 35N23.146; modify load_2.longitude 119W0.132;
    

## History

The modify directive was added in <https://sourceforge.net/p/gridlab-d/code/5594>. 

## See also

  * [Directives]
