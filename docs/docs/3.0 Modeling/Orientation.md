# Orientation

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Orientation
  
orientation – Type of panel orientation 

## Synopsis
    
    
    module generators;
    class [solar] {
         enumeration {DEFAULT=0, FIXED_AXIS=1, ONE_AXIS=2, TWO_AXIS=3, AZIMUTH_AXIS=4} orientation;
    }
    object [solar] {
         orientation _value_ ;
    }
    

## Remarks

Currently implemented orientation types include DEFAULT and FIXED_AXIS. DEFAULT implies the panel is tracking for ideal insolation. FIXED_AXIS represents an array with a fixed [tilt angle] and a fixed [direction it faces]. 

### DEFAULT

This orientation uses ideal insolation. Insolation is calculated by 
    
    
    Insolation = [solar_flux] * [shading_factor]
    

where the value for [solar_flux] either comes from a climate object, or if no climate object is included [solar_flux] is set to 1000. 

### FIXED_AXIS

When orientation is set as [FIXED_AXIS], [weather], [tilt_angle], [orientation_azimuth], [SOLAR_TILT_MODEL], and [shading_factor] are all used to calculate the solar radiation. 

### ONE_AXIS

_Not yet implemented_

### TWO_AXIS

_Not yet implemented_

### AZIMUTH_AXIS

_Not yet implemented_

## See Also

  * generators
    * [solar]
      * [shading_factor]
      * [tilt_angle]
      * [orientation_azimuth]
  * climate
    * [weather]
      * [solar_flux]

