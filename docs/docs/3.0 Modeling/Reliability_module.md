# Reliability module

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Reliability_module
# Reliability module

## Contents

  * 1 **Power System Faults**
  * 2 **Requirements of the Reliability Module**
    * 2.1 Reliability Metrics
    * 2.2 Data acquisition for metrics calculation
  * 3 **Reliability Module Validation**
    * 3.1 **Simultaneous faults**

In order for GridLab-D to effectively model the effects of distribution feeder reconfiguration, it is necessary to calculate the metrics of IEEE-1366. Distribution reconfiguration is designed to reconfigure the distribution system following a system fault in an attempt to minimize the impact on the end use customers. To properly model this in GridLab-D, it is necessary for the reliability module to interface with the power flow model in order to insert faults, call the restoration object, restore faulted sections, and to calculate the metrics of IEEE-1366 for the duration of the simulation period. This document outlines the requirements of the GridLab-D reliability module, for use with power system analysis. 

## **Power System Faults**

  * All asymmetrical short-circuit faults such as Single-Line-Ground (L-G), Line-Line (L-L), Double-Line-Ground (L-L-G); symmetrical Three-Line-Ground (3-L-G) fault; one open conductor line faults, two open conductor line fault, three conductor open line faults will be enumerated in the reliability module.


  * The user will be able to manually enter the different types of fault and the time when the fault occurs during GridLab-D simulations. The user should also be able to specify the types of distribution used for the calculation of MTTF (Mean Time To Failure) and MTTR (Mean Time To Repair)including but not limited to normal, exponential, log-normal, Weibull, Poisson , and Pareto distribution. The user should also be able to specify the unit of time to be used for the metrics calculation.


  * The reliability module will also have default sets of probabilistic values for every fault condition and will choose those values based on the MTTF (Mean Time To Failure) factor for every link object in the system.


  * The module will also have the capability so that the default values can be overwritten with user defined MTTF (Mean Time To Failure) by any object class.


  * Simultaneous line faults capability will be present inside the reliability module. All the customers affected will be treated equally.
GridLab-D is not designed to perform detailed fault analysis. For the purpose of reconfiguration, when a fault occurs, the first upstream protective device (fuse, switch etc)is assumed to operate, islanding the downstream loads if the system is radial. No reclosing action will be incorporated in this module. 

## **Requirements of the Reliability Module**

The reliability module must be able to perform the following tasks: 

  * Develop distribution curves for the Mean Time To Failure (MTTF) and Mean Time To Repair (MTTR) for every link object in the system. A module level as well as an object level specification with an ability to be overwritten by the user will be provided. The user should be able to specify the types of distribution he/she wants. All the distributions supported by Gridlab-D should be supported including but not limited to normal, exponential, log-normal, Poisson & Weibull distribution curves.
    

  * As an example, the data to obtain Mean Time To Failure (MTTF)calculated by Weibull Distribution is described below. Weibull distribution has become increasingly popular because the three parameters associated with it scale(alpha),shape(gamma)and location can be easily changed to obtain different characteristics. For this module gamma=1(exponential distribution).


$$PDF : f(t) = (\gamma/t)(t/\alpha)^\gamma e^{-(t/\alpha)^\gamma} 
$$ $ \lambda = \frac{\gamma}{\alpha}(\frac{t}{\alpha})^{\gamma -1}$

when, $  \gamma =1$,$\alpha = \frac{1}{\lambda}$

  
where, 

  $\gamma$ = the Weibull slope(the shape parameter)

  $\alpha$ = the characteristic life/scale parameter

  $\lambda$ = Failure rate (Number of failures/user specified time)

  * MTTR = time to diagnose a problem + time to get repair technician on-site + time to physically repair a system. As an example , Pareto distribution to obtain MTTR is described below.

$$PDF: f(t) = \frac{C \alpha m^ \alpha}{t^ \alpha+1}, m<=t<=n 
$$ $ C = \frac {1} {1-(n/m)^\alpha}$

where 

  $\alpha$ = shape parameter of the Pareto distribution

  $m$ = scale parameter of the Pareto distribution

  $n$ = truncation point of the Pareto distribution

  $C$ = Normalization factor

  * When any part of a link is determined to have failed, the reliability module must remove that part from service in the power flow module, and indicate to the powerflow module that the topology of the system has changed.

  * For each load the module must track the number of outages and duration of each outage.

  * At the end of the simulation the module must be able to calculate these relaibility indices : SAIFI, SAIDI, CAIDI, and ASAI for each feeder in the model, as well as each substation.

  * The output of the module will be a .csv file that indicates; the IEEE 1366 metrics for the substations and individual feeders and a list of outage events. It will also have the capability to generate output for the number of customers and different loads affected by the outage.
  

#### Reliability Metrics

The following reliability indices will be calculated in this reliability module: 

  * **SAIFI** = The system average interruption frequency index (SAIFI), shows how often the average customer experiences a sustained interruption over a period of time. In GridLAB-D that period of time is a year. The equation below is used to calculate SAIFI.

$$SAIFI = \frac{\sum N_i}{N_T} 
$$

where, 

    

  $N_i$ = Number of customers interrupted.

  $N_T$ = Total number of customers in the area of interest

  


  * **SAIDI** = The system average interruption duration index(SAIDI), shows the average duration of a sustained interruption for a customer during the reporting period. In GridLAB-D this index is given in minutes. The equation below is used to calculate SAIDI.

$$SAIDI = \frac{\sum r_i N_i}{N_T} 
$$

where, 

    $N_i$ = Number of customers interrupted.

    $N_T$ = Total number of customers in the area of interest

    $r_i$ = restoration time (unit specified by the user)

  * **CAIDI** = The customer average interruption duration index (CAIDI), shows average time needed to restore service. This unit for the index will be user specified in GridLAB-D. The equation below is used to calculate CAIDI.

$$CAIDI = \frac{SAIDI}{SAIFI} 
$$

  * **ASAI** = The average service availability index (ASAI), shows the fraction of time that a customer has received power during the reporting period. This index is given as a percentage within GridLAB-D. The equation below is used to calculate ASAI
  

$$ASAI = \frac{N_T * (Number of hours / year) - \sum r_i N_i}{N_T * (Number of hours / year)} 
$$

where, 


  $N_T$ = Total number of customers in the area of interest

_NOTE: There is no specification between regular years and leap years when calculating this index in GridLAB-D. The regular 8760 hours/year is used when calculated this index._

#### Data acquisition for metrics calculation

In order to calculate the reliability indices metrics various system data needs to be obtained. 

  * Number of customer interruptions can be calulated at the meter level ( triplex meter for houses, meter for industrial loads ) by measuring the voltage or energy consumption at that node. If the measured voltage is zero, the customer lost its service and will go in the list of customer interruption.


  * Number of customers can be caluculated by the amount of triplex meters, meters present on the feeder.
## **Reliability Module Validation**

Initial testing of the reliability module will be done on IEEE 13-node system for various relaibility indices check. It will be further expanded to evaluate the reliability module and feeder reconfiguration performance on two taxonomy feeders. 

  * _SAIFI, SAIDI, CAIDI, ASAI indices check_
    
  * The IEEE 13-node system will be populated with majorly residential/commercial customers. One open conductor fault on the _overhead line_ will be simulated. The mean time to restoration for one conductor of a line is considered and the SAIFI,SAIDI,CAIDI,ASAI indices are calculated. The tests can be repeated for two open conductor and three open conductor faults and L-G, L-L-G, L-L, 3-L-G faults.

  * The IEEE 13-node system will be populated with majorly residential/commercial customers. One open conductor fault on the _underground line_ will be simulated. The mean time to restoration for one conductor of a line is considered and the SAIFI,SAIDI,CAIDI,ASAI indices are calculated. The tests can be repeated for two open conductor and three open conductor faults and L-G, L-L-G, L-L, 3-L-G faults.
  
### **Simultaneous faults**

The reliability indices will be verified for simultaneous faults scenario. The IEEE 13-node system will be populated with residential/commercial loads. A line fault (one open conductor) and a three -phase-ground fault (3-L-G) will be applied on the feeder. The reliability indices SAIFI, SAIDI, CAIDI and ASAI will be calculated. 


  
