# Implicit enduses

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Implicit_enduses
# Implicit enduses

implicit_enduses \- Enable implicit enduses in the house model. 

## Contents

  * 1 Synopsis
  * 2 Remarks
    * 2.1 NONE
    * 2.2 LIGHTS
    * 2.3 PLUGS
    * 2.4 OCCUPANCY
    * 2.5 DISHWASHER
    * 2.6 MICROWAVE
    * 2.7 FREEZER
    * 2.8 REFRIGERATOR
    * 2.9 RANGE
    * 2.10 EVCHANGER
    * 2.11 WATERHEATER
    * 2.12 CLOTHESWASHER
    * 2.13 DRYER
  * 3 See also
## Synopsis
    
    
    module [residential] {
       implicit_enduses LIGHTS|PLUGS|OCCUPANCY|DISHWASHER|MICROWAVE|FREEZER|REFRIGERATOR|RANGE|EVCHARGER|WATERHEATER|CLOTHESWASHER|DRYER;
    }
    

## Remarks

Implicit enduses allow the house model to provide loadshape-based appliance models. 

### NONE

**NONE** disables all the implicit enduses. 

### LIGHTS

### PLUGS

### OCCUPANCY

### DISHWASHER

### MICROWAVE

### FREEZER

### REFRIGERATOR

### RANGE

### EVCHANGER

### WATERHEATER

### CLOTHESWASHER

### DRYER

## See also

  * [Globals]
  * [Residential]
