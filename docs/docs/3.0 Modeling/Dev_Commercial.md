# Dev:Commercial - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Commercial
# Dev:Commercial

## Contents

  * 1 Building
    * 1.1 Class members
    * 1.2 Default HVAC Controller
  * 2 See also
Dev:Commercial \- Commercial building developer's guide 

Note
    The small [office] building is part of the original implementation of the commercial module and is expected to be deprecated when the full commercial building implementation is completed. This will include deprecation of the [multizone] class. Small office buildings will be derived from the [building] implementation when that is validated and released.

## Building

The [building] class implements that abstract class used to solve all linearized multizone building models. All multizone commercial building classes are derived from this class. 

### Class members

_General Properties_ 
| Property| Unit | Constraints | Default | Description | Remarks  
--|--|--|--|--|--|
T | [degF] | N×1 ∈ **R** | Ø | Node temperatures |   
N | ([int16]) | ∈ **N** + | 1 | Number of nodes in model   
U | [Btu/degF/h] | N×N symmetric ∈ **R** *2 | Ø | Node conductances   
C | [Btu/degF] | N×1 ∈ **R** * | Ø | Node capacitance | NaN indicates outdoor node   
Q | [Btu/h] | N×1 ∈ **R** | Ø | Node heat flows |   
Qs | [Btu/h] | N×1 ∈ **R *** | Ø | Node solar heat gain |   
Qi | [Btu/h] | N×1 ∈ **R *** | Ø | Node internal heat gain |   
  
_Default HVAC properties_ 
| Property| Unit | Constraints | Default | Description | Remarks  
--|--|--|--|--|--|
Qfl | [Btu/h] | N×1 ∈ **R *** | NaN | Node fan heat gain at low power |   
Qfh | [Btu/h] | N×1 ∈ **R *** | NaN | Node fan heat gain at high power |   
Qf | [Btu/h] | N×1 ∈ **R *** | NaN | Node heat gain from fans |   
Qhc | [Btu/h] | N×1 ∈ **R *** | NaN | Node heating capacity |   
Qh | [Btu/h] | N×1 ∈ **R *** | NaN | Node heat gain from heating |   
Qcc | [Btu/h] | N×1 ∈ **R *** | NaN | Node cooling capacity |   
Qc | [Btu/h] | N×1 ∈ **R *** | NaN | Node heat loss from cooling |   
**Note** : NaN is used to indicate that no default HVAC equipment is associated with the node   
  
_Default controller properties_ 

| Property| Unit | Constraints | Default | Description | Remarks  
--|--|--|--|--|--|
Ts | (double) | N×1 ∈ {0,1,2,3,4,5} | NaN | HVAC state | 0=OFF, 1=VENT, 2=HEAT, 3=COOL, 4=AUX, 5=ECON   
Vm | [pu/h] | N×1 ∈ **R *** | NaN | Minimum ventilation required |   
Th | [degF] | N×1 ∈ **R +** | NaN | Heating set-point | Must be less than $Tc-2Td$  
Tc | [degF] | N×1 ∈ **R +** | NaN | Cooling set-point | Must be greater than $Th+2Td$  
Td | [degF] | N×1 ∈ **R +** | NaN | Set-point deadband |   
tl | [s] | N×1 ∈ **N** + | 300 | Control lockout time | Must be less than or equal to maximum_timestep   
**Note** : NaN is used to indicate that no default control equipment is associated with the node   

  
_Other members_

   |  |  |  |  |  |  
--|--|--|--|--|
autosize | (bool) | ∈ {TRUE,FALSE} | FALSE | Enables automatic sizing of arrays   
load | ([enduse]) |  |  | Electric end use load composition   
  
### Default HVAC Controller

The default controller implements a simple single zone vent/heat/cool/aux control. To override the default controller you must implement the _plc_() function is the derived class. 

The default control strategy for node _n_ is as follows: 
    
    
    if mode == OFF || mode == VENT
      if T < Th-2*Td
        mode = AUX
      else if T < Th - Td/2
        mode = HEAT
      else if T > Tc + Td/2
        mode = COOL
      else if Vm > 0 
        mode = VENT
      else
        mode = OFF
    
    else if _mode_ == HEAT
      if T < Th-2*Td
        mode = AUX
      else if T > Th+Td/2
        if Vm > 0
          mode = VENT
        else
          mode = OFF
    
    else if _mode_ == COOL
      if T < Tc - Td/2
        if Vm > 0
          mode = VENT
        else
          mode = OFF
    
    else if _mode_ == AUX
      if T > Th - Td/2
        if Vm > 0
          mode = VENT
        else
          mode = OFF
    

Note
    Implementing the _plc_() function for a building means that the default controller is disabled for all nodes in the building. This means that if you want to continue using the default controller for some nodes you must call the [building::plc]() function directly for that node.

## See also

  * [User's manuals]
    * [Commercial module]
    * Building types 
      * [Office]
      * [Large office] **TODO**: 
      * [Small office] **TODO**: 
      * [Retail] **TODO**: 
      * [Grocery] **TODO**: 
      * [Food_service] **TODO**: 
      * [Lodging] **TODO**: 
      * [School] **TODO**: 
      * [Health] **TODO**: 
  * Technical documents 
    * [Requirements]
    * [Specifications]
    * [Technical support document]
    * Developer's guide
    * [Validation]
  * [Residential]
  * [Modules]
