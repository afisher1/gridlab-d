# Appliance - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Appliance
**_Proposed for review_** REVIEW NEEDED 

appliance \- General residential appliance model [Template:NEW30]

## Synopsis
    
    
    module [residential];
    object appliance { 
      // inherited from residential_enduse
      durations "_duration-array_ ";
      transitions "_transition-array_ |_transition-matrix_ ";
      impedances "_impedance-array_ ";
      currents "_current-array_ ";
      powers "_power-array_ ";
      heatgains "heatgain-array _";_
    

} 

## Remarks

The appliance model implement a general purpose state-based residential appliance model. The model implements a N state model where N is the length of the durations array. As the appliance changes states, the corresponding values from the impedances, currents, powers, and heatgains are copied to the residential_enduse properties. 

The transitions matrix specified the probability of a state change during the time interval of the state specified in the durations array. If the transitions matrix is a 1xN matrix, the state transitions are linear from state 0 to state N-1 and back to state 0 during the time interval. If the transitions matrix is a NxN matrix, the state transitions are from any given state to any other state with the corresponding probability during the time interval. A transition value of 0 is used to indicate that no transition will occur in the interval. 

### durations

A 1xN array of [doubles] specifying the time duration of each state. The size of the durations array determines the number of states. 

### transitions

A 1xN or NxN matrix of [doubles] specifying the transition probability to the next state (for 1xN) or any state (for NxN) during the time interval. The value of N is determined by the size of the durations array. 

### impedances

A 1xN array of complex values specifying the impedance of the appliance when in each state. The value of N is determined by the size of the durations array. 

### current

A 1xN array of complex values specifying the currents of the appliance when in each state. The value of N is determined by the size of the durations array. 

### powers

A 1xN array of complex values specifying the powers of the appliance when in each state. The value of N is determined by the size of the durations array. 

### heatgains

A 1xN array of [doubles] specifying the heat gains fraction to the interior air of the appliance when in each state. The value of N is determined by the size of the durations array. 

## Example

The following example illustrates a simple linear-state appliance model: 
    
    
    module residential;
    object appliance {
      //           daily,  pump,   agitate,  pump,   spin
      durations   "86400   60      300       60      300";
      transitions "0.25    0.0     0.0       0.0     0.0";
      powers      "0       1+0.1j  1+0.2j    1+0.1j  2+0.2j";
    };
    

The state machine starts roughly once daily with a 1/4 probability. The first state lasts 1 minute with a 1kW+0.1kVAR load. The second state lasts 5 minutes with a 1kW+0.2kVAR load. The third state lasts 1 minute with a 1kW+0.1kVAR load. The last state last 5 minutes with 2kW+0.2kVAR load. 

## Version

The general appliance model was introduced in [Hassayampa (Version 3.0)]. 

## See also

  * [Residential module]
    * [User's Guide]
    * [Appliances]
    * [house] class – Single-family home model.
    * residential_enduse class – Abstract residential end-use class.
    * [occupantload] – Residential occupants (sensible and latent heat).
    * [ZIPload] – Generic constant impedance/current/power end-use load.
  * Technical Documents 
    * [Requirements]
    * [Specifications]
    * [Developer notes]
    * [Technical support document]
    * [Validation]
