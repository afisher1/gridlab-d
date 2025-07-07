---
title: Advanced Distribution Models
excerpt: >-
  
deprecated: false
hidden: false
metadata:
  robots: index
---
The distribution system models we've looked at thus far have been perfectly adequate for evaluating many common smart grid scenarios but they have relatively simple architectures. In this chapter, we'll look at how to include a number of additional components that GridLAB-D™has modeled that will allow the construction of more realistic electrical distribution systems, systems that are much more similar to what is in use today.

## Voltage Regulators

One of the great challenges for distribution system design is managing voltage under the wide range of loading conditions a distribution feeder experiences over a daily and annual load patterns. ANSI C.84 is the most commonly used in the United States and it defines the acceptable range of voltages based on location in the system with the most stringent being at the service entrance to the customers, nominally 114 V to 126 V a range of +/- 5% of the nominal 120 V.

One of the primary tools in this voltage management task is the voltage regulator. Simply defined, a voltage regulator is a transformer with the ability to adjust (within a limited range) its turns ratio. This allows changes in the input voltage to the regulator to be compensated for and produce a relatively constant output voltage at a target value. For example, a voltage regulator may be placed at a point in the distribution system where the voltage tends to be low under early evening load, say 116 V. This would be the input voltage to the regulator and the regulator settings may be set to give an output voltage of 124 V. In the early morning when the load is much lower and the voltage on the line rises to 120 V. If this was just a transformer with a fixed turns ratio, the output voltage would rise to over 128 V, outside of the acceptable ANSI band. Instead, the regulator adjusts its turns ratio and produces the same output of 124V.

A voltage regulator changes its turns ratio by mechanically switching between a number of taps on the secondary coil of the transformer, shorting out portions of the winding and changing the number of effective turns on the secondary. There are a limited number of tap positions built into the regulator so the range and resolution of the tap changes is fixed and relatively limited. The lifetime of the regulator is often dictated by the number of tap changes it makes and reducing the number, replacement and maintenance costs of the regulators is one goal when designing and operating a distribution system.

To be effective in this regulation role, a voltage regulator needs some kind of input signal to use as a reference as it makes adjustments throughout the day. The simplest signal source would be the voltage on the output of the regulator with tap changes taking place to keep the voltage within specification. Using this strategy is generally not very effective because the voltage drop downstream of the regulator is not accounted for. Ideally the output voltage on the regulator would be on the high side of the ANSI band to allow as much acceptable voltage drop as possible before another voltage regulator would have to be added to the system.

Similarly, targeting a fixed output voltage fails to take into account the loading on the circuit. When the loads are lighter the voltage regulator need not adjust up as much due to the lower voltage drop on the downstream line. When the load is heavier, these additional adjustments are necessary to maintain the voltage with the ANSI specification.

Because of this variability, it is common that voltage regulators contain a small, simple model circuit (called a line-drop compensator) internal to the device that seeks to replicate the loading of the line downstream of the device. The output voltage of the regulator is applied to the line-drop compensator and based on its output voltage, the voltage regulator adjusts to bring the line-drop compensator output voltage to a specified target value. Typically the line-drop compensator is a simple series combination of a generic resistance and reactance value. 

### Example - Voltage Regulators with Line-Drop Compensators

To demonstrate the operation of the voltage regulator, we start where we left off with the battery and solar PV model. Relatively speaking, this is a small model with very light loads and if this was the system in real-life scenario, we wouldn't need to bother with a voltage regulator. So, to make the situation more interesting, two changes were made to the model:
* The line lengths between the houses were increased by a factor of ten.

* The resistance of the overhead lines were increased by a factor of ten.
Both of these changes will result in greater voltage drops for relatively small currents which will make the operation of the voltage regulator more important.

Opening up [dist_sys_voltage_regulator_ldcdist_sys_voltage_regulator_ldc.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%207%20-%20Advanced%20Distribution%20Systems/Distribution%20Systems%20-%20Voltage%20Regulators/) shows these two changes to the model with the addition of in the inclusion of a voltage regulator at the head of the feeder (the substation) and an extra recorder to monitor the voltages down branch 1 and the tap position of the voltage regulator. Looking at the configuration for the regulator we see

```
object regulator_configuration {
  name reg_config;
  connect_type 1;
  band_center 122.000;
  band_width 2.0;
  time_delay 15.0;
  raise_taps 16;
  lower_taps 16;
  current_transducer_ratio 1;
  power_transducer_ratio 58;
  compensator_r_setting_A 1.5;
  compensator_x_setting_A 3.0;
  CT_phase "ABC";
  PT_phase "ABC";
  regulation 0.10;
  Control LINE_DROP_COMP;
  control_level INDIVIDUAL;
  Type B;
  tap_pos_A 0;
  tap_pos_B 0;
  tap_pos_C 0;
  }
```

To understand all of these setting requires more time and space than we have here but the model used in GridLAB-D™is based on that set-out in [Kersting](https://books.google.com/books/about/Distribution_System_Modeling_and_Analysi.html?id=a-MY5JLUTKYC). Without trying to dig into the details, we can look at through these settings and get a rough idea of how the regulator is set-up to work.

*band_center 122.000` shows that the target voltage is 122 volts (on a 120V  basis).

*band_width 2.0` shows that we have a +/- 1V tolerance on the 122V target before the regulator operates.

*time_delay 15.0` shows we are modeling a 15 second time delay every time the regulator actuates. That is, it will take 15 seconds between measuring a need to make a tap change and the tap change going into effect and changing the output voltage.

`raise_taps 16`, `lower_taps 16` shows there are a total of 32 tap positions the regulator has to operate over
`regulation 0.10` indicates that a 10% change will occur from neutral (tap position 0) to the extreme lower or upper positions (tap position +/- 16).
  `compensator_r_setting_A 1.5, compensator_x_setting_A 3.0` are the most important. They show that the modeled effective impedance to the end of the line results in a voltage drop of 0.1 + j0.1 volts (on a 120V basis). The units on this are a bit odd but they are very practical; rather than having to measure or calculate the effective impedance to the end of the line, a simple measurement at that point can directly be used in defining the settings for the line-drop compensator.


Running the model shows there are some problems, though. Looking at "voltage_data.csv" shows that when the simulation starts, the voltage regulator is producing an acceptable voltage for all three residences, roughly 122 V. In the afternoon when the load increases the voltage drop on the line also increases and we start seeing lower voltages, down to 116V on the very last house in the branch. During this period, the voltage regulator does not responde, indicating the modeled impedance in the line-drop compensator is set too low. That is, when the load in the circuit increase, the voltage drop modeled by the line-drop compensator is too little and causing the regulator to believe that no response is needed.

To correct this, let's just guess at new values for the line-drop compensator and re-run the simulation.

```
  compensator_r_setting_A 0.8;
  compensator_x_setting_A 0.8;
```

Re-running this simulation shows that the regulator does a much better job of responding the to changing loading conditions on this branch of the feeder and adjusts as the load changes, namely the air-conditioners switching on and off.

### Example - Voltage Regulators with Voltage Sensing

With communication systems becoming a more common part of electrical distribution systems new techniques of handling the setting of voltage regulation have arrived. Namely, rather than modeling and estimating the voltage at the end of the line, why not just measure it and send that information back to the voltage regulator? GridLAB-D™support just such an operational mode.

Open up [dist_sys_voltage_regulator_sensedist_sys_voltage_regulator_sense.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%207%20-%20Advanced%20Distribution%20Systems/Distribution%20Systems%20-%20Voltage%20Regulators/) and take a look at how the voltage regulator is configured.

```
object regulator {
  name substation_reg;
  phases "ABC";
  sense_node "b1m3_house_meter";
  from substation_meter;
  to feeder_meter;
  configuration reg_config;
  }
  
object regulator_configuration {
  name reg_config;
  connect_type 1;
  band_center 122.000;
  band_width 2.0;
  time_delay 15.0;
  raise_taps 16;
  lower_taps 16;
  CT_phase "A";
  PT_phase "A";
  regulation 0.10;
  Control REMOTE_NODE;
  control_level BANK;
  Type B;
  tap_pos_A 0;
  tap_pos_B 0;
  tap_pos_C 0;
  }
```

The regulator itself now defines a `sense_node` which, as you might guess, is the node at which the voltage will be measured for regulation purposes. The `regulator_configuration` object is simpler with no more compensator settings and the `control` set to `REMOTE_NODE` instead of `LINE_DROP_COMP`. (The `CT_phase`, `PT_phase`, and `control_level` have also been changed to eliminate some extraneous warnings about voltages in phase B and C where there is no load in this system. The example systems we've been using are massively unbalanced which in the real-world would be a serious problem but in simulation, is something we can safely ignore.)

Running this model produces the following warning in the console:

```
WARNING  [2009-08-08 11:26:03 PDT] : b1m1_house - house:23 is outside of ANSI standards (voltage = 106 percent of nominal 120/240)
WARNING  [2009-08-09 04:30:00 PDT] : last warning message was repeated 127 times
```

As the message indicates, the first house in branch 1 has a voltage that is slightly too high. Opening up "voltage_data.csv" shows this is the case, particularly in the afternoon under heavier loading.

This is not an unexpected result as we are targeting a nominal voltage at the end of the line which might be expected to make the voltage at the beginning of the line too high. In this case, an easily fix would be to change our sense node so that we regulate the middle of the line to a nominal value; the end of the line will be slightly lower and the beginning slightly higher but everybody will stay within an acceptable voltage range.

In the `regulator` object change the sense node to

```
  sense_node "b1m2_house_meter";
```

and re-run the simulation.
  The result: no warnings and "voltage_data_.csv" confirms that all three residential voltages are much closer to nominal.
  
## Capacitors

Capacitors are another common device used to help manage the voltage on the distribution system. Particularly during summer days with air-conditioners presenting a high inductive load to the system, a significant portion of the current flowing over the distribution lines is reactive. The default source of this reactive current comes from the substation which can lead to non-trivial voltage drops as the reactive current travels to and from the reactive loads in the distribution system. By providing an alternative source of reactive power closer to the loads (capacitors) the magnitude of the current flowing from the substation is reduced and the voltage profile of the feeder generally improves.

GridLAB-D™ provides the ability to add capacitors to the distribution system and to also control when they are switched into and out of the feeder. There are a few different control modes defined for the capacitor including manual control constantly in or out or able to switched using a `schedule` or `player`, voltage-based, or reactive power based (VAR). Like the voltage regulator, the capacitors can either be switched individually or on a per-phase basis.

### Example - Voltage-based capacitor switching

Open up [dist_sys_capacitordist_sys_capacitor.glm](https://github.com/gridlab-d/course/blob/master/Tutorial/Chapter%207%20-%20Advanced%20Distribution%20Systems/Distribution%20Systems%20-%20Capacitors/) and take a look at the capacitor that has been added to the model.

```
object capacitor {
  phases ABC;				
  name b1m3_capacitor;
  pt_phase ABC;				
  parent branch_1_meter_3;
  phases_connected ABC;		
  control VOLT;
  voltage_set_low 6900.0;
  voltage_set_high 7100.0;
  capacitor_A 10.0 kVAr;
  capacitor_B 10.0 kVAr;
  capacitor_C 10.0 kVAr;
  control_level INDIVIDUAL;
  switchA OPEN;
  switchB OPEN;
  switchC OPEN;
  time_delay 2.0;
  dwell_time 10;
  nominal_voltage 7216.88;
}
```

We can see that the control mode has been set to `VOLT` and that this 10 kVAR capacitor (per phase) has been parented to `branch_1_meter_3` at the end of the branch 1 line. Since the `remote_sense` property has been unspecified, the capacitor will use its own local voltage to determine when the switch; in this case when the voltage falls below 6900 V it will switch in and when it rises above 7100 V it will switch out. `time_delay` is used to model the amount of time it will take for the switching to take place once it has been commanded (2 seconds in this case), and `dwell_time` requires the switching condition be met for the specified period of time (10 seconds) before the switching operation is commanded. Between the two, the time between when the switching condition is met and when the switching will take place is twelve seconds.

Running this model in GridLAB-D™produces "voltage_data.csv" (among others) which contains a record of the capacitor switch state and the measured voltage at the `branch_1_meter_3` node. As can be seen, when the voltage drops below 6900 V the capacitor switches in, the voltage rises, and capacitor switches out and the voltage drops. We can also see the delay caused by the `time_delay` and `dwell_time` parameter values.

![Dist syst switching capacitors.png](/images/Dist_syst_switching_capacitors.png)

