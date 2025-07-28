# Technical Overview

    A few pages, describing what GLD can do in more detail. This section is for those that are pretty sure GLD can do something they want and they want to make sure. Still lots of graphics but meant to be browsed and read in 15 minutes or less.

    How GLD models the power system and how that’s expressed in a .glm

    * Object definition through parameters
    * Object synchronization through time
    * QSTS

    Three-phase un-balanced QSTS powerflow

    Smart grid device modeler

    Single-zone structure with HVAC (nominally residential home)

    * Rooftop solar
    * Battery
    * EV

    Three-phase un-balanced transient

    * Deltamode
    * Detailed inverter and induction motor models

    Co-Simulation and HELICS

    Other features Trevor doesn’t use as much and thus forgets about

    libgld (if it comes into existence)

# Capabilities

The GridLAB-D system currently implements modules to perform the following functions: 

  * Power and energy flow and control
  * Load electric, thermal, and control behavior
  * Economic behaviors
  * Data collection and analysis
  * Physical and economic boundary condition management
  * Integration with other software

# Power Flow

The power flow component of GridLAB-D is separated into a distribution module and a transmission module. While the distribution systems are the primary focus of GridLAB-D, the transmission module is included so that the interactions between two or more distribution systems can be simulated. 

## Transmission System

The transmission system is included to allow for the interconnection of multiple distribution feeders. If a transmission module was not included each distribution system could only be solved independently of other systems. While distribution systems can be solved independently, as is common in current commercial software packages, GridLAB-D will have the ability to generate a power flow solution for multiple distributions systems interconnected via a transmission or sub-transmission network. Traditionally the ability to examine interactions at this level has been limited by computational power. To address this limitation, GridLAB-D is being developed for execution on multiple processor systems. In the current version of GridLAB-D the AC power flow solution method used for the transmission system is the Gauss-Seidel (GS) method, chosen for its inherent ability to solve for poor initial conditions, and to remain numerically stable in multiprocessor environments. 

## Distribution System

In order to accurately represent the distribution system the individual feeders are expressed in terms of conductor types, conductor placement on poles, underground conductor orientation, phasing, and grounding. GridLAB-D does not simplify the distribution system component models. The distribution module of GridLAB-D utilizes the traditional forward and backward sweep method for solving the unbalanced 3-phase AC power flow problem. This method was selected in lieu of newer methods such as current injection methods for the same reasons that the GS method was selected for the transmission module; converging in the fewest number of iterations is not the primary goal. Just as with the transmission module the distribution modules will only start with a flat start at initialization and all subsequent solutions will be derived from the previous time step. 

Metering is supported for both single/split phase and three phase customers. Support for reclosers, islanding, distributed generation models, and overbuilt lines are anticipated in coming versions. 

The following power distribution system components are implemented and available for use: 

  * Overhead and underground lines
  * Transformers
  * Voltage regulators
  * Fuses
  * Switches
  * Shunt capacitor banks

# Buildings

Commercial and residential buildings are implemented using the Equivalent Thermal Parameters model. These are differential models solved for both time as a function of state and state as a function of time. Currently implemented residential end-uses are: 

  * Water heaters
  * Refrigerators
  * Stand-alone freezers
  * Dishwashers
  * Clothes washers and dryers
  * Electric ranges
  * Microwaves
  * Electric plugs and lights
  * Internal gains
  * House loads (including air conditioning, heat pumps, and solar loads)

Commercial loads are simulated using an aggregate multi-zone Energy Technology Perspectives (ETP) model that will be enhanced with more detailed end-use behavior in coming versions. 


# Applications
Today's power systems simulation tools don't provide the analysis capabilities needed to study the forces driving change in the energy industry. The combined influence of fast-changing information technology, novel and cost-effective distributed energy resources, multiple and overlapping energy markets, and new business strategies result in very high uncertainty about the success of these important innovations. Concerns expressed by utility engineers, regulators, various stakeholders, and consumers can be addressed by GridLAB-D. Some example uses include: 

## Rate structure analysis

Multiple differentiated energy products based on new rate structure offerings to consumers is very attractive to utilities because it creates the opportunity to reveal demand elasticity and gives utilities the ability to balance supplier market power in the wholesale markets. The challenge is designing rate structures that are both profitable and attractive to consumers. GridLAB-D will provide the ability to model consumer choice behavior in response to multiple rate offerings (including fixed rates, demand rates, time-of-day rates, and real-time rates) to determine whether a suite of rate offerings is likely to succeed. 

## Distributed resources

The advent of new distributed energy resource (DER) technologies, such as on-site distributed generation, BCHP and Grid-FriendlyTM appliance controls creates a number of technology opportunities and challenges. GridLAB-D will permit utility managers to better evaluate the cost/benefit trade-off between infrastructure expansion investments and distributed resources investments by including the other economic benefits of DER (e.g., increase wholesale purchasing elasticity, improved reliability metrics, ancillary services products to sell in wholesale markets). 

## Peak load management

Many peak-shaving programs and emergency curtailment programs have failed to deliver the expected benefits. GridLAB-D can be calibrated to observe consumer behavior to understand its interaction with various peak shaving strategies. The impact of consumer satisfaction on the available of peak-shaving resources can be evaluated and a more accurate forecast of the true available resources can be determined. GridLAB-D will even be able to evaluate the consumer rebound effect following one or more curtailment or load-shed events in a single day. 

## Distribution automation design

GridLAB-D can support some aspects of the design and analysis of distribution automation technology, allowing utilities to offer heterogeneous reliability within the same system but managing power closer to the point of use. 

# Theory of Operation

## Synopsis
**core/exec.c (C code)**
```
STATUS exec_start(void)
```

## Description
During command line processing, GridLAB-D™ loads one or more GLM files listed and uses the directives in those files to set global variables, set global variables, run scripts, load modules, define classes, create objects, and link to external applications. Once all the objects are created and initialized, the main exec loop is started and the global clock begins to advance.


### Clock operation
GridLAB-D's main processing loop advances the global clock by varying time steps depending on the pending state changes of the object's defined in the model until no object reports that is has a pending state change.

Pending state state changes are called sync events. There are two types of sync event:

**hard events**
these are events that must be handled in order for the simulation to continue. If there are no hard events pending, the simulation will stop.


**soft events**
these are events that are only handled if they occur before a hard event. If there are only soft events pending, the simulation will stop.
The stoptime global variable is a special hard event if it is not set to NEVER. If the stoptime is set to NEVER, the simulation will only stop when there are no pending hard events. Otherwise the simulation will stop when the stoptime is reached.


### Object ranks
Objects are assigned ranks when they are created. The rank strategy varies depending on the solver being used and the parent-child relationships between the objects. Normally, the modeler only controls the parent-child relationship by either nesting objects when they are defined, or using the name property. Modelers cannot strictly control the rank of objects because modules will generally apply their own ranking rules to ensure a stable solution.


### Synchronization procedure
Objects are instances of classes. Each class has a synchronization characteristic the determines when it receives synchronization signals from the main exec loop. The sync signals are sent in the following order

- create - called once per object
- init - called until object confirms successful initiatialization (only called once prior to Hassayampa (Version 3.0))

*do until clock stops*

- precommit - called once per timestep before sync; used to setup for new timestep
    *do until valid timestep is found'*
    - presync - called once per pass from the top rank down; used to prepare objects for bottom-up pass
    - sync - called once per pass from the bottom rank up; used to perform main calculations in objects
    - postsync - called once per pass from the top rank down; used to complete calculations
- commit - called once per timestep after sync; used to lock-in states
- finalize - called once per simulation after clock stops
- term - called to terminate simulation

### Parallelization and multithreading
Any iterative process that does not have interdependencies can be run in parallel using multiple thread. When the threadcount is not 1, GridLAB-D™will create threads to handle a wide variety of loops that can be operated in parallel.


### Realtime operation
When running in run_realtime mode, GridLAB-D™limits how fast the clock will advance based on the value of the realtime global variable.


### Internal synchronization events
There are a number of internal synchronization events that take place while the main exec loop is running.

### Before precommit
- Link - links are updated first
- Instance - All slave instances are resumed
- Random - All random variables are updated
- Schedule - All schedules are update
- Loadshape - All loadshapes are updated
- Transforms - Schedule transforms are updated
- Enduse - All enduses are updated
- Heartbeat - All heartbeat signals are sent

### Before commit
- Transforms - Non-schedule transforms are updated.
- Instances - Slave instances are waited on

### After commit
- Scripts - Script sync events are run.


### Debugging
Debugging the main exec loop can be very challenging. The --debugger command line option can be used to facilitate the process. In addition, copious use of the tape module's recorder object, particularly with intervals set to 0 or -1 can also be very helpful.