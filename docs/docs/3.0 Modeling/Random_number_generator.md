# Random number generator

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Random_number_generator

The random_number_generator [global variable] determines which random number generation method is used during simulation. 

## RNG2

This specifies the platform dependent style of random number generation. It is also not thread-safe. 

## RNG3

This specifies the platform independent style of random number generation. It is also thread-safe. 

# GLM
    
    
    globals {
      random_number_generator RNG3;
    }
    #set random_number_generator=RNG3
    

# Notes

RNG2 is the default random number generator for [Four Corners (Version 2.2)] and older. From [Grizzly (Version 2.3)] on, RNG3 is the default random number generator. 

# Command line
    
    
    host% gridlabd --define random_number_generator=RNG3
    

# See also

  * [Random]
  * [Multithreaded Random Number Generation]

