# Integral

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Integral
**_Proposed for review_**

integral \- Built-in derivative data type 

## Synopsis
    
    
    class _class_name_ {
      integral _name_[_[unit]_];
    }
    object _class_name_ {
      _name_ S(_variable1_)d(_variable2_);
      _name_ S(_variable1_)dt;
    }
    

## Description

The integral property automatically calculates the average integral of _variable1_ with respect to another _variable2_. If _dt_ is used, the integral is with respect to the global clock. The value of the integral is updated whenever _variable1_ or _variable2_ changes. If _dt_ is used, the value of the integral is updated after the object's clock is updated. 

Both variables must have either double or complex underlying type. The multiplication is performed according to the usual mathematical rules for a product of double and complex values, as appropriate. 

The units of the integral must be consistent the units obtained when multiplying _variable1_ by _variable2_. The conversion is performed automatically each time the integral value is updated. If either of the variables has no unit, then the integral cannot have a unit, and the values are simply multiplied. 

Variables can be local to the object, in which case they are simply named. If a variable in another object is used, the name of the object must be prepended, as in `object_name.variable`. 

[Global variables] may used provided the are of double or complex type. The syntax for a [global variable] is `::variable` for core globals and `module::variable` for module globals. 

## Example

The following example maintains the integral of the random variable _x_ with respect to time and names it _Sx_. 
    
    
    class example {
    	[random] x[m];
    	integral Sx[m/s];
    }
    
    object example {
    	x "type:[normal](0,1); [refresh]:1h";
    	Sx S(x)dt;
    	object recorder {
    		property "x,Sx";
    		interval -1;
    		file example.csv;
    	};
    }
    

## See also

  * [Built-in types]
  * [derivative]
