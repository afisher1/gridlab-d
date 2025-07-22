# Generic Schedule Object

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Generic_Schedule_Object
## Contents

  * 1 Schedules
    * 1.1 Schedule format
    * 1.2 Schedule properties
    * 1.3 Schedule methods
    * 1.4 Schedule state variables
    * 1.5 Schedule Caveats
  * 2 Shape Types
    * 2.1 Analog
    * 2.2 Random pulse
    * 2.3 Pulse modulation
      * 2.3.1 Amplitude Modulation
      * 2.3.2 Pulse-width Modulation
      * 2.3.3 Frequency Modulation
    * 2.4 Queued pulses
The Schedule object stems from the separate schedule methods present in several objects in GridLAB-D ~ after observing that the same basic system was in place in three different objects, the conclusion was made that the Schedule should be turned into a stand-alone class that can be used by many different classes with a minimum of effort. 

# Schedules

When the Schedule initializes, it parses its schedule definition into a data structure that can be quickly traversed in order to determine the most desired value for the current time. This may be specific to the minute of the hour, to the hour of the day, to the day of the week, to the day of the month, and to the month of the year. Multiple fields may be used to narrow the particular field as desired. When the given time has been isolated, its value will be cached, and possibly pushed onto any subscribers. The schedule will then determine when it will change its value, and cache that timestamp for future use. If the schedule is called to synchronize when it does not need to determine its next value, it will short-circuit without pushing its value and without re-traversing its schedule. 

## Schedule format

The Schedule object will use the same formatting as the POSIX crontab utility. Absent scale values will be assumed to be 1.0. 

Examples: 
    
    
     * * * * * 1.0           # the value 1.0 all the time
     0-30 * * * * 1.0        # the value 1.0 for the first half of every hour
     * 8-17 * * * 0.5        # the value 0.5 from 8am to 5pm every day
     * 17-8 * * * 0.5        # the value 0.5 from 5pm to 8am every day
     * 8-12,1-17 * * * 0.5   # the value 0.5 from 8am to noon and from 1pm to 5 pm every day
     0 0 1,15 * *  0.2       # the value 0.2 at midnight on the 1st and 15th of every month
     0 12 1 * 1,7 * 0.3      # the value 0.3 at noon on the 1st of January and July
     0 12 1-7 * 1 0.4        # the value 0.4 on the first Monday of every month
    

Schedule objects may not contain more than four blocks of time values, and no block may have more than 63 unique nonzero values. All schedules implicitly use zero as the first value for their dynamic value range. 

## Schedule properties

The schedule includes the following properties: 

  * a schedule definition string
  * the current value
  * the current timestamp the current value is valid until
  * the default value to use if the current time is not defined by the schedule
## Schedule methods

The schedule has public functions... 

  * to subscribe to the automatic updating of the schedule's scalar
  * to unsubscribe to the schedule's updating
## Schedule state variables

To define a generic bimodal schedule machine, the follow variables must be defined: 

  * $s = \\{on,off\\}$ is the mode
  * $E$ is the energy (in kWh) used during the _on_ mode;
  * $V$ is the supply voltage;
  * $I_{constant}$ is the current of the constant current during the _on_ mode (in amps);
  * $Z_{constant}$ is the impedance of the constant impedance during the _on_ mode (in ohms);
  * $P_{constant}$ is the power of the constant power during the _on_ mode (in kW);
  * $P = P_{constant} + V I_{constant} + V^2 / Z_{constant}$ is the power (in kW) used during the _on_ mode;
  * $\epsilon_{t_{on}}$ is the random variation in the _on_ time (in seconds);
  * $\epsilon_{t_{off}}$ is the random variation in the _off_ time (in seconds);
  * $t_{on} = E / P + \epsilon_{t_{on}}$ is the _on_ time (in seconds);
  * $\phi = t_{on} + t_{off} + \epsilon_{t_{off}}$is the period of _on_ plus _off_ time (in seconds);
  * $\theta = t_{on} / \phi$ is the duty cycle (unitless);
  * $t_{off} = t_{on} / \theta - t_{on}$ is the duration of the _off_ time;
  * $q$ is the value of the internal state variable (unitless)
  * $\delta_{on} + \epsilon_{\delta_{t_{on}}}$ is the threshold value of $q$ at which the mode becomes _on_ ;
  * $\delta_{off} + \epsilon_{\delta_{t_{off}}}$ is the threshold value of $q$ at which the mode becomes _off_ ;
  * $r_{on} = ( \delta_{off} - \delta_{on} ) / t_{on}$ is the rate at which $q$ approaches $\delta_{off}$;
  * $r_{off} = ( \delta_{on} - \delta_{off} ) / t_{off}$ is the rate at which $q$ approaches $\delta_{on}$;
  * $q_0 = Uniform(\delta_{on},\delta_{off})$ is the initial value of the internal state variable
  * $q_{t+dt} = q_t + r_s dt$ is the value of the internal state variable at $dt$ seconds have elapsed
  * $s =
\begin{cases} s_t=on: & \begin{cases} 
    
    
       q_t \ge \delta_{t_{off}}: & off \\
       q_t < \delta_{t_{off}}: & on 
     \end{cases} \\
    

s=off: & \begin{cases} 
    
    
       q_t \le \delta_{t_{on}}: & on \\
       q_t > \delta_{t_{on}}: & off 
     \end{cases}
    

\end{cases} $$

## Schedule Caveats

The use of schedules should be moderated due to their size of about 15MB apiece. If large numbers of objects take linearly related schedule input data, [schedule transforms] should be considered. 

# Shape Types

## Analog

The present value in the raw load shape is copied to the property directly. If the energy is non-zero, then $power = value \times energy / interval$. 

The power of the load is the average energy computed above divided by the load shape interval. Typically, this is 1 hour, so the power in (kW) is equal to the energy/duration in (kWh/h). Do not assume that this is always true. 

Table 1 - Analog load shape parameters  Parameter | Unit | Default | Source | Description   
---|---|---|---|---  
raw_value | kWh/h | 0.0 | load shape | The value of the load shape for the end-use at the given time   
value | pu.kWh/h | 0.0 | load shape | The raw load shape value / the total area of the load shape   
energy | kWh | 1.0 | load shape | The magnitude of the complete load shape (i.e., the area under the curve)   
interval | s | 3600 | load shape | The time over which the load shape value prevails   
  
## Random pulse

The probability of a pulse being generated is proportional to the value multiplied by the scalar: $Prob\\{on\\} = value \times scalar$. 

The energy is the energy of entire load shape. If the duration of the pulse is non-zero, then the end-use power is the energy divided by the duration: $power = energy / duration$. 

If the constant power, current, and/or impedance of the pulse is non-zero, then the duration of the pulse is the energy divided by the actual power: $duration = energy / power$. 

Either power or duration must be specified. If neither is specified, it is an error. 

There can be no more than one pulse per interval. 

Table 1 - Random pulse load shape parameters  Parameter | Unit | Default | Source | Description   
---|---|---|---|---  
raw_value | kWh/h | 0.0 | load shape | The value of the load shape for the end-use at the given time   
value | pu.kWh/h | 0.0 | load shape | The raw load shape value / the total area of the load shape   
scalar | pu | 1.0 | load shape | The number of random pulses generated per load shape   
energy | kWh | 1.0 | load shape | The energy of complete load shape (i.e., the area under the curve)   
interval | s | 3600 | load shape | The time over which the load shape value prevails   
duration | s | 0 | load shape | The duration of the pulse, if constant   
power | kW | 0 | load shape | The nominal power of the pulse, if constant   
  
## Pulse modulation

The loadshape value is the energy of a series of pulse generated during the interval and is equal to the shape value multiplied by the scalar: $ count = value \times scalar$. 

The energy specifies the total energy used during a complete load shape. The period is interval divided by the loadshape value: $period = interval / value / scalar$. 

If the power of a pulse is non-zero, then the duration is the energy divided by the power divided by the loadshape value: $duration = energy / power / scalar$. 

If the power is not specified (i.e., power is zero), then the duration must be specified. The power is energy divided by the duration divided by the scalar: $power = energy / duration / scalar$. 

Table 1 - Random pulse load shape parameters  Parameter | Unit | Default | Source | Description   
---|---|---|---|---  
raw_value | kWh/h | 0.0 | load shape | The value of the load shape for the end-use at the given time   
value | pu.kWh/h | 0.0 | load shape | The raw load shape value / the total area of the load shape   
scalar | pu | 1.0 | load shape | The count of pulses during a complete load shape.   
interval | s | 3600 | load shape | The time over which the load shape value prevails   
energy | kWh | 1.0 | load shape | The energy of complete load shape (i.e., the area under the curve)   
duration | s | 0 | load shape | The duration of the pulse, if constant   
power | kW | 0 | load shape | The nominal power of the pulse, if constant   
  
### Amplitude Modulation

[![Example of an amplitude modulated loadshape](../../images/300px-Modulated_loadshape.png)](/wiki/File:Modulated_loadshape.png)

Figure 1 - Example of an amplitude modulated loadshape

Modulated loadshapes where the power varies as a function of load have fixed duty cycle and period have amplitude modulation. AM is indicated by _modulation_ = MMT_AMPLITUDE, and 
    
    
     period[s/pulse] = duration[s/event] / pulsecount[pulse/event]
     MPT_TIME:  duty_cycle[1] = pulsevalue[s/pulse] / period[s/pulse]
     MPT_POWER: duty_cycle[1] = energy[kWh/event] / pulsevalue[kW] / duration[s/event] * 3600[s/h] 
     power[kW] = schedulevalue[kWh/h/event] * scalar[/event]
     ton[s/pulse] = duty_cycle[1]* period[s/pulse]
     toff[s/pulse] = period[s/pulse] - ton[s/pulse]
    

### Pulse-width Modulation

[![Example of a pulse-width modulated loadshapes](../../images/300px-Pw_modulated_loadshape.png)](/wiki/File:Pw_modulated_loadshape.png)

Figure 2b - Example of a pulse-width modulated loadshapes

Modulated loadshapes where the power is fixed and the period is fixed have pulse-width modulation. PWM is indicated by _modulation_ = MMT_PULSEWIDTH, and 
    
    
     MPT_TIME: power[kW] = energy[kWh/h] / pulsevalue[s/pulse]
     MPT_POWER: power[kW] = pulsevalue[kW]
     pulsecount[cycle] = energy[kWh] / power[kW] * duration[s] / 3600[s/h]
     period[s/cycle] = duration[s] / pulsecount[cycle]
     ton[s] = schedulevalue[kWh/h] * scalar[kW/cycle] / energy[kWh] / pulsecount[/cycle]
     toff[s] = period[s/cycle] * 1[unit] - ton[s]
     duty_cycle[cycle] = ton[s] / period[s/cycle]
    

### Frequency Modulation

Modulated loadshapes where the power is fixed and the on-time is fixed have frequency modulation. FM is indicated by _modulation_ = MMT_FREQUENCY, and 
    
    
     MPT_TIME: 
       ton[s/pulse] = pulsevalue[s/pulse]
       power[kW] = pulseenergy[kWh/pulse] / ton[s/pulse] * 3600[s/h]
     MPT_POWER: 
       power[kW] = pulsevalue[kW]
       ton[s/pulse] = pulseenergy[kWh/pulse] / power[kW] * 3600[s/h]
     dutycycle = energy[kWh/event] / schedulevalue[kWh/event] / scalar
     period[s/pulse] = ton[s/pulse] / dutycycle
     toff[s] = period[s/pulse] - ton[s/pulse]
    

## Queued pulses

The normalized load shape value multiplied by the energy divided by the interval is the accrual rate $r_{off} = value \times scalar / interval $. The queue must be initialized to a random value between $q_{on}$ and $q_{off}$, which are specified in the load shape. 

If the power is non-zero, then when the load is on, the accrual rate also includes the power, e.g., $r_{on} = r_{off} - power / energy$. The queue is incremented by the accrual rate times the timestep $q += r_x dt$. When the queue exceeds $q_{on}$, the state of the machine switches to _on_ , and the load is calculated accordinately (see below). When the queue falls below $q_{off}$, the state of the machine switches to _off_. The duration of a pulse is the time taken to move from $q_{on}$ to $q_{off}$ at the rate $r_{on}$. 

If the duration is non-zero, then it must be set to the time required to decrement the queue by 1 unit and $power = r_{off} - (q_{on} - q_{off}) / duration$. 

When the load is on, the on-power (including on-current and on-impedance fractions) is applied to the target end-use load and when the load is off, the off-power (including off-current and off-impedance) is is applied to the target end-use load. 

Table 1 - Random pulse load shape parameters  Parameter | Unit | Default | Source | Description   
---|---|---|---|---  
raw_value | kWh/h | 0.0 | load shape | The value of the load shape for the end-use at the given time   
value | pu.kWh/h | 0.0 | load shape | The raw load shape value / the total area of the load shape   
interval | s | 3600 | load shape | The time over which the load shape value prevails   
energy | kWh | 1.0 | load shape | The energy of complete load shape (i.e., the area under the curve)   
duration | s | 0 | load shape | The duration of the pulse, if constant   
power | kW | 0 | load shape | The nominal power of the pulse, if constant   
scalar | pu | 1 | load shape | The total accrual on the queue for one complete load shape   
q_on | pu | 1 | load shape | The queue accrual value at which the machine turns on   
q_off | pu | 0 | load shape | The queu accrual value at which the machine turns off   
  

