# Reliability User Guide - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Reliability_User_Guide
# Reliability User Guide

## Contents

  * 1 Reliability Overview
  * 2 Overall settings
  * 3 Reliability Objects
  * 4 Metrics
    * 4.1 Metrics Parameters
  * 5 Eventgen
    * 5.1 Eventgen external event mode
    * 5.2 Eventgen Parameters
  * 6 Powerflow implementation
  * 7 See Also
## Reliability Overview

The `reliability` module provides the ability to induce events on a system and evaluate their impacts. The `reliability` module collects and outputs module-appropriate metrics to a log file for user evaluation. `reliability` is only an event-inducing and metrics-recording module and requires specific interfaces to different modules of interest. At this time, the only module interface that exists is with the `powerflow` module (see the [Power Flow User Guide]). 

## Overall settings

The `reliability` module has two module-level settings that a user can specify. These two settings influence all reliability-related items in the system, regardless of the module. The two parameters are `maximum_event_length` and `report_event_log`, which would be implemented as 
    
    
    module reliability {
    	maximum_event_length 300 s;
    	report_event_log TRUE;
    	}
    

The `maximum_event_length` specifies the maximum duration any object may be in a fault condition on the system. If a randomly-generated fault exceeds this time limit, it will be thresholded to the value of `maximum_event_length`. However, manual faulting scenarios do not check this limit, and can produce longer fault condition. 

The `report_event_log` property defines whether the `metrics` object will include a list of induced events, or just the output statistics. If enabled, the output file will include when an event started, when it ended, the desired fault, the implemented fault, and some customer information. 

## Reliability Objects

There are two object within the `reliability` module. These two objects will interact with specific module classes to generate the events and record module-appropriate metrics. 

## Metrics

The `metrics` object collects and outputs any of the measurements associated with reliability in a specific module. The actual metrics are computed by module-specific objects, such as the `power_metrics` object in the `powerflow` module. The output file containing the event information and reliability calculations is output by the `metrics` object. 

A minimal `metrics` object would be implemented as 
    
    
    object metrics {
    	name test_metrics;
    	module_metrics_object powermetrics;
    	metrics_of_interest "SAIFI,SAIDI";
    	customer_group "groupid=METEROBJS";
    	}
    

with an equivalent implementation of 
    
    
    object metrics {
    	name test_metrics;
    	module_metrics_object powermetrics;
    	metrics_of_interest "SAIFI,SAIDI";
    	customer_group "groupid=METEROBJS";
    	metric_interval 0 s;
    	report_interval 1 yr;
    	}
    

### Metrics Parameters

Property Name  | Type  | Unit  | Description   
---|---|---|---  
report_file  | char1024  | N/A  | File name where the event log (if desired) and metric calculations are written.   
customer_group  | char1024  | N/A  | Defines the criterion for an object to be considered a customer. Uses specifications similar to `collector` objects with `class=` and `groupid=` being valid keywords.   
module_metrics_object  | object  | N/A  | Link to the module-specific metrics device to compute reliability indices related to that module. An example would be the `power_metrics` object in the `powerflow` module.   
metrics_of_interest  | char1024  | N/A  | Comma-separated list of metrics to output into the `report_file`. Values desired must be published by the module-specific metrics object.   
metric_interval  | double  | seconds  | Intermediate interval over which to calculate reliability indices. Interval metrics will be reset every `metric_interval` seconds. Simulation-long metric calculations are unaffected by this value. Defaults to 0 seconds, so intermediate values are not output.   
report_interval  | double  | seconds  | Time period between writes to the `report_file`, unless prompted to do so earlier by `metric_interval`. Drives the output of overall simulation-long metric calculations. Defaults to 1 year.   
  
## Eventgen

The `eventgen` object creates events on objects in the GLM file to test the reliability of the system. Such induced events often produce a relevant result in the `metrics` object, or in some output of the system. The `eventgen` object allows both manually specified, deterministic events, or randomly-generated, stochastic events. 

`eventgen` objects must be parented to an appropriate `metrics` object to function. This allows the reliability indices to be calculated appropriately when an event is induced. 

The minimal setup required for a randomly-induced event would be 
    
    
    object eventgen {
    	name RandEvent;
    	parent MetricsObj;
    	target_group "class=line AND groupid=FAULTY";
    	fault_type "DLG-X";
    	}
    

which is equivalent to a fully specified version of 
    
    
    object eventgen {
    	name RandEvent;
    	parent MetricsObj;
    	target_group "class=line AND groupid=FAULTY";
    	fault_type "DLG-X";
    	failure_dist EXPONENTIAL;
    	restoration_dist PARETO;
    	failure_dist_param_1 3.858e-7;
    	failure_dist_param_2 0.0;
    	restoration_dist_param_1 1.0;
    	restoration_dist_param_2 1.0002778;
    	max_outage_length 432000 s;
    	max_simultaneous_faults -1;
    	}
    

A deterministic `eventgen` object can be specified as 
    
    
    object eventgen {
    	name DeterEvent;
    	parent MetricsObj;
    	fault_type "DLG-X";
    	manual_outages "nodeB,2000-01-01 5:00:00,2000-01-02 6:00:00";
    	}
    

Note that a deterministic `evengen` object ignores the limits specified in the module-level `maximum_event_length` parameter, as well as the `max_outage_length` and `max_simultaneous_faults` properties. 

### Eventgen external event mode

`eventgen` also has an "external event" mode where an event can be sent via JSON string, via something like HELICS. To support this approach, a typical `eventgen` object would look like: 
    
    
    object eventgen {
    	name ExternalSupported;
    	use_external_faults TRUE;
    	}
    

External events are sent to the `external_fault_event` field, and have a JSON syntax of: 
    
    
    "{
    	\"name\": \"fault1\",
    	\"type\": \"TLL\",
    	\"fault_object\": \"line12\"
    }"
    

where the `name` field is a unique identifier for the fault event, `type` is the same as the powerflow types listed at the end of this wiki page, and `fault_object` is the GridLAB-D object to induce the event on. Note that the `name` is utilized as a unique identifier of this fault, so to turn the fault off, simply sending that name will be sufficnent. e.g.,: 
    
    
    "{\"name\":\fault1\"}"
    

### Eventgen Parameters

Property Name  | Type  | Unit  | Description   
---|---|---|---  
target_group  | char1024  | N/A  | Defines the criterion for the objects to induce faults upon. Uses specifications similar to `collector` objects with `class=` and `groupid=` being valid keywords. Note that stochastically specified `eventgen` objects will induce faults on all objects in the `target_group` field independently, with only consideration for the `max_simultaneous_faults` field. This field must be empty for `manual_outages` to work.   
fault_type  | char1024  | N/A  | Describes the type of faults/events to induce on the specified objects. This is specific to the module the faults/events are being induced upon, or the reliability metrics are measuring.   
failure_dist  | enumeration  | N/A  | Distribution type used to generate times to random "failure" event on the system. Valid distributions are `UNIFORM`, `NORMAL`, `LOGNORMAL`, `BERNOULLI`, `PARETO`, `EXPONENTIAL`, `RAYLEIGH`, `WEIBULL`, `GAMMA`, `BETA`, and `TRIANGLE`.   
restoration_dist  | enumeration  | N/A  | Distribution type used to determine time to restoration on faulted/evented objects. Valid distributions are `UNIFORM`, `NORMAL`, `LOGNORMAL`, `BERNOULLI`, `PARETO`, `EXPONENTIAL`, `RAYLEIGH`, `WEIBULL`, `GAMMA`, `BETA`, and `TRIANGLE`.   
failure_dist_param_1  | double  | Varies  | Parameter 1 for the distribution selected in `failure_dist`. Specifies the following for the distributions listed: 

  * `UNIFORM` \- lower range of the uniform distribution
  * `NORMAL` \- mean of the distribution
  * `LOGNORMAL` \- geometric mean of the distribution
  * `BERNOULLI` \- probability of the distribution
  * `PARETO` \- minimum value of the distribution
  * `EXPONENTIAL` \- coefficient of the distribution
  * `RAYLEIGH` \- _sigma_ of the distribution
  * `WEIBULL` \- _lambda_ of the distribution
  * `GAMMA` \- _alpha_ of the distribution
  * `BETA` \- _alpha_ of the distribution
  * `TRIANGLE` \- _a_ of the distribution

  
failure_dist_param_2  | double  | Varies  | Parameter 2 for the distribution selected in `failure_dist`. If a distribution is not listed below, it does not need the second parameter. Specifies the following for the distributions listed: 

  * `UNIFORM` \- upper range of the uniform distribution
  * `NORMAL` \- standard deviation of the distribution
  * `LOGNORMAL` \- geometric standard deviation of the distribution
  * `PARETO` \- gamma scale of the distribution
  * `EXPONENTIAL` \- _k_scale_ of the distribution
  * `WEIBULL` \- _k_ of the distribution
  * `GAMMA` \- _beta_ of the distribution
  * `BETA` \- _beta_ of the distribution
  * `TRIANGLE` \- _b_ of the distribution

  
restoration_dist_param_1  | double  | Varies  | Parameter 1 for the distribution selected in `restoration_dist`. Specifies the following for the distributions listed: 

  * `UNIFORM` \- lower range of the uniform distribution
  * `NORMAL` \- mean of the distribution
  * `LOGNORMAL` \- geometric mean of the distribution
  * `BERNOULLI` \- probability of the distribution
  * `PARETO` \- minimum value of the distribution
  * `EXPONENTIAL` \- coefficient of the distribution
  * `RAYLEIGH` \- _sigma_ of the distribution
  * `WEIBULL` \- _lambda_ of the distribution
  * `GAMMA` \- _alpha_ of the distribution
  * `BETA` \- _alpha_ of the distribution
  * `TRIANGLE` \- _a_ of the distribution

  
restoration_dist_param_2  | double  | Varies  | Parameter 2 for the distribution selected in `restoration_dist`. If a distribution is not listed below, it does not need the second parameter. Specifies the following for the distributions listed: 

  * `UNIFORM` \- upper range of the uniform distribution
  * `NORMAL` \- standard deviation of the distribution
  * `LOGNORMAL` \- geometric standard deviation of the distribution
  * `PARETO` \- gamma scale of the distribution
  * `EXPONENTIAL` \- _k_scale_ of the distribution
  * `WEIBULL` \- _k_ of the distribution
  * `GAMMA` \- _beta_ of the distribution
  * `BETA` \- _beta_ of the distribution
  * `TRIANGLE` \- _b_ of the distribution

  
manual_outages  | char1024  | N/A  | Manual input describing which object to fault, when to fault it, and when to restore it. In the syntax above, `nodeB,2000-01-01 5:00:00,2000-01-01 6:00:00` would fault the object nodeB from 5 AM on January 1, 2000 to 6 AM on January 2, 2000. All times must follow the _yyyy-mm-dd HH:MM:SS_ format. The fault induced is determined by the `fault_type` field. `target_group` must be empty for this field and implementation type to be considered. Subsequent faults may be specified in the same format (_name, time start, time end_).   
max_outage_length  | double  | seconds  | Defines the maximum time an object is allowed be out of service after faulted/evented. Serves as an upper limit to the value produced by the `restoration_dist` function. This parameter is ignored when a fault is specifed by the `manual_outages` field.   
max_simultaneous_faults  | double  | N/A  | Defines the maximum number of faults a particular `eventgen` object can create at once. If more faults are requested by the `fault_dist` property, they will be ignored. This parameter is ignored when a fault is specified by the `manual_outages` field.   
use_external_faults  | bool  | N/A  | If set to true, expects externally-defined events to come through the `external_fault_event` field in the JSON syntax noted above.   
external_fault_event  | char1024  | N/A  | Input field for externally-defined/populated events. Must be in the JSON format described in Eventgen external event mode  
  
## Powerflow implementation

At this time, the `reliability` module only interfaces with the `powerflow` module. Faults can currently only be induced on `link` objects within the `powerflow` module. Faults available are: 

  * SLG-A, SLG-B, SLG-C, SLG-X -> single-line-ground A, B, C, or random
  * DLG-AB, DLG-BC, DLG-CA, DLG-X -> double-line-ground AB, BC, CA, or random
  * LL-AB, LL-BC, LL-CA, LL-X -> line-line AB, BC, CA, or random
  * TLG -> triple-line-ground - ABC
  * OC-A, OC1-A, OC-B, OC1-B, OC-C, OC1-C, OC-X, OC1-X -> single open conductor A, A, B, B, C, C, random, or random (OC and OC1 are the same)
  * OC2-AB, OC2-BC, OC2-CA, OC2-X -> double open conductor AB, BC, CA, or random
  * OC3 -> triple open circuit - ABC
The following fault types are valid events in `reliability` calls, but should not be explicitly induced. These events are induced by `switch` or `fuse` objects when specific actions occur. They may show up in the output log of `powerflow` `reliability` calls, but should not be explicitly specified as a fault. 

  * SW-A, SW-B, SW-C -> open switch phase A, B, or C
  * SW-AB, SW-BC, SW-CA -> open switch phases AB, BC, or CA
  * SW-ABC -> open switch phases ABC
  * FUS-A, FUS-B, FUS-C, FUS-X -> fuse tripping (forced) A, B, C, or random
  * FUS-AB, FUS-BC, FUS-CA, FUS-X -> fuse tripping (forced) AB, BC, CA, or random
  * FUS-ABC -> fuse tripping (forced) - ABC
# 

See Also

[Reliability]

[Power Flow User Guide]


  
