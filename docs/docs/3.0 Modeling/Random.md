# Randomvar - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Random

---


## Synopsis
    
    
    class my-class {
      randomvar property-name;
    }
    object my-class {
      my_random_var "specification"; 
    }
    

## Description

The randomvar property type is an object property that changes randomly over time. The way in which it changes is controlled by the definition of the randomvar property. The definition of a random property uses a string defined either in the source GLM file or update by a player or external feed that contains all the attributes needed to define or modity the random property. The specification string is formatted as follows: 
    
    
    "type:distribution(a[,b]); refresh:delay [time_unit]; min:low; max:high; state:seed"
    

For example, the following code adds a randomized property called _clouds_ to the **climate** class, creates an object that has a randomvar property updated hourly from a truncated Weibull distribution, and records the value every 5 minutes in a CSV file: 
    
    
     clock {
     	timezone PST+8PDT;
     	starttime '2001-01-01 0:00:00 PDT';
     	stoptime '2002-01-01 0:00:00 PDT';
     }
     module climate;
     module tape;
     
     class climate {
     	randomvar clouds;
     } 
     
     object climate {
     	clouds "type:weibull(0.5,0.5); min:0.0; max:1.0; refresh:1h;";
     	object recorder {
     		property clouds;
     		interval 600;
     		file "random_builtin.csv";
     	};
     }
    

## ASCII formatting

When converting from a randomvar to a string, only the current value of the random number is formatted using the [double_format] [global variable]. 

When converting from a string to a randomvar, the _specification_ string is used to modify the behavior of the random number generator that underlies the property. 

## See also

  * [Built-in types]
  * [Random numbers]
  * Source documentation 
    * [Random number generators](http://gridlab-d.sourceforge.net/doxygen/3.0/group__random.html)

