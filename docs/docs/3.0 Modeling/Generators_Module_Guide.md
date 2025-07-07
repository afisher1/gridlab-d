# Generators Module Guide - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Generators_Module_Guide
## Contents

  * 1 Generators Objects
    * 1.1 Battery Model
      * 1.1.1 Default Battery
      * 1.1.2 Battery Parameters
      * 1.1.3 Battery State of Development
    * 1.2 Diesel DG Model
      * 1.2.1 Default Diesel
      * 1.2.2 Diesel Parameters
      * 1.2.3 Diesel State of Development
    * 1.3 Energy Storage Model
      * 1.3.1 Default Energy Storage
      * 1.3.2 Energy Storage Parameters
      * 1.3.3 Energy Storage State of Development
    * 1.4 Inverter Model
      * 1.4.1 Default Inverter
      * 1.4.2 Inverter Parameters
      * 1.4.3 Inverter State of Development
    * 1.5 Microturbine DG Model
      * 1.5.1 Default Microturbine
      * 1.5.2 Microturbine Parameters
      * 1.5.3 Microturbine State of Development
    * 1.6 Power Electronics Model
      * 1.6.1 Default Power Electronics
      * 1.6.2 Power Electronics Parameters
      * 1.6.3 Power Electronics State of Development
    * 1.7 Rectifier Model
      * 1.7.1 Default Rectifier
      * 1.7.2 Rectifier Parameters
      * 1.7.3 Rectifier State of Development
    * 1.8 Solar DG Model
      * 1.8.1 Default Solar
      * 1.8.2 Solar Parameters
      * 1.8.3 Solar State of Development
    * 1.9 Wind Turbine DG Model
      * 1.9.1 Default Wind Turbine
      * 1.9.2 Wind Turbine Parameters
      * 1.9.3 Wind Turbine State of Development
    * 1.10 Model testing
    * 1.11 See Also
The generator, or DG, model within GridLAB-D has never been fully developed. The multitude of control algorithms, different technologies, and implementations available and the various levels of model complexity, make it difficult to fully represent the DG models that are available. Instead, the goal has been to create a very open environment, where users can create their own models, including their own controls and algorithms. Within the auspices of DOE, some models have been created for specific uses ( a simple solar model, a specific wind turbine model, various battery control models ), and should be thought of as examples in how DGs are modeled within GridLAB-D. 

Essentially, the DG models can be thought of as negative loads (or in the case of batteries that are charging, normal loads). They are connected to the powerflow solution through triplex meters or meters (or in some cases, in series through an inverter to a meter or triplex meter), and act as a load on the system using components of a ZIP model. The internal DG models are required to calculate their ZIP components as a function of the user's/model's desired inputs - ie solar panels use the system voltage and solar radiation. These components are then posted back to the powerflow solution via the meters as either a pure current injection or as a combinatorial ZIP model. As the generators are a separate module from powerflow, they are required to have the meter interface and must post their load information rather than being calculated directly within the powerflow solution (as would be with a load object). 

The following will describe the available models within GridLAB-D and how they interface with the powerflow module. Additionally, it will offer ideas/plans for future development. 

# Generators Objects

## [Battery] Model

The battery object is in a state of flux, containing some legacy models and some new models. In early versions of GridLAB-D (pre-v3.0), it was assumed that the model included both the battery and the inverter; the battery was connected directly to a meter (or triplex_meter) object. Post-v3.0, the inverter model has been separated from the battery object. In this case, the battery is connected as a child of an inverter object and the inverter is then connected to the meter object. This may cause some confusion - development of new models will focus on a full separation of the inverter and battery models, but legacy code still exists for those that are still using it. 

The battery model does not explicitly represent a specific battery chemistry, but rather is a generic energy storage device; the parameters can be set to represent a fairly simplistic representation of a wide variety of chemistries and technologies. 

### Default Battery

Because of the multiple models and wide variety of parameters, there is no "default" battery. 

### Battery Parameters

Property Name | Type | Unit | Description   
---|---|---|---  
generator_status  |  enumeration  |  N/A  |  Allows user to define when the generator is in operation or not. 

  * ONLINE
  * OFFLINE

  
generator_mode  |  enumeration  |  N/A  |  Defines the control mode of the inverter. Only CONSTANT_PF has been validated to any degree and allows the user to adjust the power factor (and therefore the Q output) under the constraints of the rated output. Do not set when using the internal battery model. 

  * UNKNOWN
  * CONSTANT_V
  * CONSTANT_PQ
  * CONSTANT_PF
  * SUPPLY_DRIVEN

  
use_internal_battery_model  |  Boolean  |  N/A  |  A boolean to use the internal battery model which is used when the battery's parent is an inverter. Default is FALSE.   
battery_type  |  enumeration  |  N/A  |  The battery type applies a voltage vs. SOC performance curve for the internal battery model to use. Default is UNKNOWN. 

  * UNKNOWON
  * LI_ION
  * LEAD_ACID

  
nominal_voltage  |  double  |  V  |  The nominal open circuit dc voltage for the battery. Used by the internal battery model. Default is blah.   
rated_power  |  double  |  W  |  The rated output power of the battery. It is used by the internal battery model. The rated_output is set by the parent inverter's power rating and efficiency. Default is blah.   
battery_capacity  |  double  |  Wh  |  The rated capacity of the battery. It is used by the internal battery model. Default is blah.   
round_trip_efficiency  |  double  |  pu  |  The round trip efficiency of the battery used by the internal battery model. Defualt is blah.   
state_of_charge  |  double  |  pu  |  The state of charge of the battery used by the internal battery model. Default is blah.   
reserve_state_of_charge  |  double  |  pu  |  The reserve state of charge that the battery is not allowed to go below. It is used by the internal battery model and is used by the parent inverter. Default is 0.   
battery_load  |  double  |  W  |  The current output power of the battery. It is set by the parent inverter and used in the internal battery model.   
rfb_size  |  enumeration  |  N/A  |  Presets for batter ratings and capacity. 

  * HOUSEHOLD: 
    * nominal_voltage = 260 V
    * rated_power = 3.6 kW
    * battery_capacity = 23.4 kWh
    * round_trip_efficiency = 0.9
  * SMALL: 
    * nominal_voltage = 75.2 V
    * rated_power = 18.8 kW
    * battery_capacity = 160 kWh
    * round_trip_efficiency = 0.7
  * MED_COMMERCIAL: 
    * nominal_voltage = 115 V
    * rated_power = 50 kW
    * battery_capacity = 175 kWh
    * round_trip_efficiency = 0.8
  * MED_HIGH_ENERGY: 
    * nominal_voltage = 115 V
    * rated_power = 50 kW
    * battery_capacity = 400 kWh
    * round_trip_efficiency = 0.8
  * LARGE: 
    * nominal_voltage = 8 kV
    * rated_power = 240 kW
    * battery_capacity = 5.76 MWh
    * round_trip_efficiency = 0.9

  
  
### Battery State of Development

This model has only been tested when using the internal battery model. All other modes are considered experimental and may not work as designed. An example of setting up the inverter and battery for load following is shown below. The load following control mode currently only operates at unit power factor. A similar control mode for reactive load following is slated for 3.2 release in summer of 2015. 
    
    
    object inverter {
        name load_follow_inverter;
        inverter_type FOUR_QUADRANT; //Must be in FOUR_QUADRANT to use the load following control scheme.
        four_quadrant_control_mode LOAD_FOLLOWING; //The only mode that works with the battery object.
        parent meter1; //parent must be a triplex_meter or meter object.
        sense_object substation_transformer; //the sense_object must be a meter, triplex_meter, or transformer.
        rated_power 3000.0; //The per phase power output rating of the inverter in VA.
        inverter_efficiency 0.95;
        charge_on_threshold 5.0 kW; //when the load at the sense_object drops below this value the inverter starts to charge the battery.
        charge_off_threshold 7.0 kW; //when the battery is charging and the load at the sense_object rises above this value the inverter stops charging the battery.
        discharge_off_threshold 7.5 kW; //when the battery is discharging and the load at the sense_object drops below this value the inverter stops discharging the battery.
        discharge_on_threshold 9.0 kW; //when the load at the sense_object rises above this value the inverter starts to discharge the battery.
        max_discharge_rate 1 kW; //The maximum power output to demand from the battery when discharging.
        max_charge_rate 1 kW; //The maximum power input to the battery when charging.
    }
    
    object battery {
        name batt_test;
        parent load_follow_inverter;
        use_internal_battery_model true;
        battery_type LI_ION;
        battery_capacity 20.0 kWh;
        round_trip_efficiency 1.0;
        state_of_charge 0.5;
        generator_mode SUPPLY_DRIVEN;
    }
    

## [Diesel DG] Model

[![](//images.shoutwiki.com/gridlab-d/thumb/f/fe/Generator_Module_Guide_Figure_1.png/300px-Generator_Module_Guide_Figure_1.png)](/wiki/File:Generator_Module_Guide_Figure_1.png)

[]

Figure 1 - Generator internal electrical model

The three phase synchronous generator is with classical model for power flow computation. In which, $\bar E_f$ is induced voltage of generator; $\bar V$ is terminal bus voltage; $\bar I$ is terminal bus current. $\bar E_f$, $\bar V$ and $\bar I$ are denoted as complex numbers. They satisfy: 

$$\begin{align} \bar E_{f_A} & = \bar V_A - \left ( R + j X_S \right ) \bar I_A - \left ( R_g + j X_g \right ) \left ( \bar I_A + \bar I_B + \bar I_C \right ) \\\ \bar E_{f_B} & = \bar V_B - \left ( R + j X_S \right ) \bar I_B - \left ( R_g + j X_g \right ) \left ( \bar I_A + \bar I_B + \bar I_C \right ) \\\ \bar E_{f_C} & = \bar V_C - \left ( R + j X_S \right ) \bar I_C - \left ( R_g + j X_g \right ) \left ( \bar I_A + \bar I_B + \bar I_C \right ) \end{align} 
$$

### Default Diesel

### Diesel Parameters

### Diesel State of Development

This model is considered experimental and may not work as designed. 

## [Energy Storage] Model

### Default Energy Storage

### Energy Storage Parameters

### Energy Storage State of Development

This model is considered experimental and may not work as designed. 

## inverter Model

A minimal model could be defined by: 
    
    
    object inverter {
        phases AS;
        generator_mode CONSTANT_PF;
        generator_status ONLINE;
        inverter_type PWM;
        power_factor 1.0;
        parent triplex_meter2;
        << implied child that provides a DC power input >>
    }
    

  


### Default Inverter

Property Name | Type | Unit | Description   
---|---|---|---  
inverter_type  |  enumeration  |  N/A  |  Defines the type of inverter technology and the efficiency of the unit. NOTE: efficiency needs to be made a variable. 

  * TWO_PULSE
  * SIX_PULSE
  * TWELVE_PULSE
  * PWM

  
generator_status  |  enumeration  |  N/A  |  Allows user to define when the generator is in operation or not. 

  * ONLINE
  * OFFLINE

  
generator_mode  |  enumeration  |  N/A  |  Defines the control mode of the inverter. Only CONSTANT_PF has been validated to any degree and allows the user to adjust the power factor (and therefore the Q output) under the constraints of the rated output. 

  * UNKNOWN
  * CONSTANT_V
  * CONSTANT_PQ
  * CONSTANT_PF
  * SUPPLY_DRIVEN

  
V_In  |  complex  |  V  |  DC voltage passed in by the DC object (e.g. solar panel or battery)   
I_In  |  complex  |  A  |  DC current passed in by the DC object (e.g. solar panel or battery)   
VA_In  |  complex  |  VA  |  VA_In = V_In * I_In   
Vdc  |  complex  |  V  |  Not used at this time.   
phaseA_V_Out phaseB_V_Out phaseC_V_Out  |  complex  |  V  |  AC voltage coming from the interconnection point. This is not a user-definable variable, but is assigned by the powerflow node. In the case of a “meter”, this can be 1-, 2-, or 3-phase. In the case of a “triplex_meter”, phaseA_V_Out is used to hold the 240V phase voltage; B & C are not used in this case.   
phaseA_I_Out phaseB_I_Out phaseC_I_Out  |  complex  |  A  |  AC current being delivered to the interconnection point. This is not a user-definable variable, but is calculated by the inverter/DC object. In the case of a “meter”, this can be 1-, 2-, or 3-phase. In the case of a “triplex_meter”, phaseA_I_Out is used to hold the 240V phase voltage; B & C are not used in this case.   
power_A power_B power_C  |  complex  |  VA  |  Calculation of power delivered to interconnection node (e.g. power_A = phaseA_V_Out * ~phaseA_I_Out).   
P_Out  |  double  |  VA  |  Not used as this time.   
Q_Out  |  double  |  VAr  |  Not used as this time.   
power_factor  |  double  |  unit  |  Defines the desired power factor in CONSTANT_PF mode.   
phases  |  set  |  N/A  |  Not used at this time – phases are assumed from the interconnection point.   
inherited from power electronics – NOTE: Many of the PE capabilities are not currently fully functional.   
Rated_kV  |  double  |  kV  |  Used as the nominal voltage when no interconnection point is designated.   
efficiency  |  double  |  unit  |  Efficiency of the inverter. This is assigned by inverter_type and cannot be overridden at this time.   
  
### Inverter Parameters

### Inverter State of Development

This model is considered experimental and may not work as designed. 

## [Microturbine] DG Model

### Default Microturbine

### Microturbine Parameters

### Microturbine State of Development

This model is considered experimental and may not work as designed. 

## [Power Electronics] Model

### Default Power Electronics

### Power Electronics Parameters

### Power Electronics State of Development

This model is considered experimental and may not work as designed. 

## [Rectifier] Model

### Default Rectifier

### Rectifier Parameters

### Rectifier State of Development

This model is considered experimental and may not work as designed. 

## [Solar] DG Model

### Default Solar

A minimal model could be created via: 
    
    
    object solar {
        generator_mode SUPPLY_DRIVEN;
        generator_status ONLINE;
        panel_type SINGLE_CRYSTAL_SILICON;
        efficiency 0.2;
        parent inverter1;
        area 2500 sf;
    }
    

### Solar Parameters

Property Name  | Type  | Unit  | Description   
---|---|---|---  
generator_mode  |  enumeration  |  N/A  |  Only operational in SUPPLY_DRIVEN at this time. 

  * UNKNOWN
  * CONSTANT_V
  * CONSTANT_PQ
  * CONSTANT_PF
  * SUPPLY_DRIVEN

  
generator_status  |  enumeration  |  N/A  |  Default is ONLINE. Allows a user to dropout a generator. 

  * OFFLINE
  * ONLINE

  
panel_type  |  enumeration  |  N/A  |  Uses pre-defined panel technologies. Defines efficiency, Pmax_temp_coeff, and Voc_temp_coeff. 

  * SINGLE_CRYSTAL_SILICON (default)
  * MULTI_CRYSTAL_SILICON
  * AMORPHOUS_SILICON
  * THIN_FILM_GA_AS (incomplete)
  * CONCENTRATOR (incomplete)

  
power_type  |  enumeration  |  N/A  |  Defines whether the connection is AC or DC. This variable is not currently used at this time as the connection method is determined from the connection device (meter vs. inverter). 

  * AC
  * DC

  
INSTALLATION_TYPE  |  enumeration  |  N/A  |  This variable is not currently used. 

  * ROOF_MOUNTED
  * GROUND_MOUNTED

  
NOCT  |  double  |  degF  |  Nominal operating cell temperature, typically designated by the manufacturer.   
Tmodule  |  double  |  degF  |  Calculated internal temperature of the PV module.   
Tambient  |  double  |  degF  |  Outside air temperature. Comes from a climate object, if available.   
wind_speed  |  double  |  mph  |  Outside wind speed. Comes from a climate object, if available.   
Insolation  |  double  |  W/sf  |  Solar radiation incident upon the solar panel. Comes from a climate object, if available.   
Rinternal  |  double  |  Ohm  |  Not currently used.   
Rated_Insolation  |  double  |  W/sf  |  Insolation level at which the cell is rated for. It is assumed that at rated insolation, power output will be at the rated maximum power output.   
Pmax_temp_coeff  |  double  |  |  Coefficient for the effects of temperature changes on the actual power output. This is explained in further documentation.   
Voc_temp_coeff  |  double  |  |  Coefficient for the effects of temperature changes on the DC terminal voltage. This is explained in further documentation.   
V_Max  |  complex  |  V (DC)  |  Defines the max operating voltage of the PV module. This is used for de-rating purposes and is not currently used as a generator cutoff.   
Voc_Max  |  complex  |  V (DC)  |  Not needed and needs to be removed   
Voc  |  complex  |  V (DC)  |  Defines the open circuit voltage as specified by the PV manufacturer. It occurs when there is no current flowing throught the PV cell   
efficiency  |  double  |  unit  |  Defines the efficiency of power conversion from the solar insolation to DC power. Defines maximum power output.   
area  |  double  |  sf  |  Defines the surface area of the solar module. Defines maximum power output.   
Rated_kVA  |  complex  |  kVA  |  Not currently used.   
P_Out  |  complex  |  kW  |  Not currently used.   
V_Out  |  complex  |  V  |  DC voltage passed to the inverter object.   
I_Out  |  complex  |  A  |  DC current passed to the inverter object.   
VA_Out  |  complex  |  VA  |  Actual power delivered to the inverter.   
FF  |  double  |  None  |  The Fill Factor (FF) is essentially a measure of quality of the solar cell. It is calculated by comparing the maximum power to the theoretical power that would be output at both the open circuit voltage and short circuit current together. This variable is currently not implemented.   
  
### Solar State of Development

This model is considered experimental and may not work as designed. 

## [Wind Turbine DG] Model

A minimal model could be created via: 
    
    
    object windturb_dg {
        parent my_meter1;
        phases ABCN;
        name windturb1;
        Rated_VA 10000;
        turbine_height 40;
    }
    

or using one of the generic turbines: 
    
    
    object windturb_dg {
        parent my_meter1;
        phases ABCN;
        name windturb1;
        Turbine_Model GEN_TURB_POW_CURVE_1_5MW;
    }
    

  


### Default Wind Turbine

When unspecified, turbine_height and Rated_VA default to the following: 

Default Rated_VA: 100,000 W 

Default turbine_height: 37 m 

These defaults are loaded when the wind turbine is defined without specifying the minimally required parameters. For example: 
    
    
    object windturb_dg {
        parent my_meter1;
        phases ABCN;
        name windturb1;
    }
    

The wind turbine object is allowed to have the following parents: 

Meter 

Triplex meter 

Inverter (for power curve-based implementation only) 

### Wind Turbine Parameters

Property Name  | Type  | Unit  | Description   
---|---|---|---  
air_density  |  double  |  kg/m^3  |  Estimated air density. Used in COEFF_OF_PERFORMANCE implementation.   
blade_diam  |  double  |  meters  |  Specifies the diameter of the blades. Used in COEFF_OF_PERFORMANCE implementation.   
cut_in_ws  |  double  |  m/sec  |  Minimum wind speed for generator operation. Used in COEFF_OF_PERFORMANCE implementation.   
cut_out_ws  |  double  |  m/sec  |  Maximum wind speed for generator operation. Used in COEFF_OF_PERFORMANCE implementation.   
Cp_max  |  double  |  p.u.  |  Maximum coefficient of performance. Used in COEFF_OF_PERFORMANCE implementation.   
Cp_rated  |  double  |  p.u.  |  Rated coefficient of performance. Used in COEFF_OF_PERFORMANCE implementation.   
Cp  |  double  |  p.u.  |  Calculated coefficient of performance. Used in COEFF_OF_PERFORMANCE implementation.   
Gen_mode  |  enumeration  |  N/A  |  Control mode that is used for the generator output. Used in COEFF_OF_PERFORMANCE implementation. Default is CONSTANTP. 

  * CONSTANTE
  * CONSTANTP
  * CONSTANTPQ

  
Gen_status  |  enumeration  |  N/A  |  Allows a user to dropout a generator. Default is ONLINE. 

  * OFFLINE
  * ONLINE

  
Gen_type  |  enumeration  |  N/A  |  Allows the user to specify the type of generator. Used in COEFF_OF_PERFORMANCE based implementation. 

  * INDUCTION
  * SYNCHRONOUS

  
Max_Vrotor  |  double  |  (p.u.)*V  |  Induction generator maximum induced rotor voltage in p.u., e.g. 1.2. Used in COEFF_OF_PERFORMANCE based implementation.   
Min_Vrotor  |  double  |  (p.u.)*V  |  Induction generator minimum induced rotor voltage in p.u., e.g. 0.8. Used in COEFF_OF_PERFORMANCE based implementation.   
Max_Ef  |  double  |  (p.u.)*V  |  Synchronous generator maximum induced rotor voltage in p.u., e.g. 1.2. Used in COEFF_OF_PERFORMANCE based implementation.   
Min_Ef  |  double  |  (p.u.)*V  |  Synchronous generator minimum induced rotor voltage in p.u., e.g. 0.8. Used in COEFF_OF_PERFORMANCE based implementation.   
power_curve_csv  |  string  |  N/A  |  Specifies the name of .csv file containing user defined power curve   
power_curve_pu  |  Boolean  |  N/A  |  A Boolean when set to TRUE indicates that the user provided power curve has power values in p.u. Default is FALSE.   
pf  |  double  |  p.u.  |  Desired power factor in CONSTANTP mode. Used in COEFF_OF_PERFORMANCE based implementation.   
phases  |  set  |  N/A  |  Specifies which phases to connect to. Triplex mode is only supported for the POWER_CURVE implementation. 

  * ABCN
  * AS
  * BS
  * CS

  
R_stator  |  double  |  (p.u.)*Ohm  |  Induction generator primary stator resistance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
R_rotor  |  double  |  (p.u.)*Ohm  |  Induction generator primary rotor resistance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
R_core  |  double  |  (p.u.)*Ohm  |  Induction generator primary core resistance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
Rated_V  |  double  |  V  |  Rated generator terminal voltage. Used in COEFF_OF_PERFORMANCE based implementation.   
Rated_VA  |  double  |  VA  |  Rated wind turbine generator power output. Default is 100 kW.   
roughness_length_factor  |  double  |  N/A  |  European Wind Atlas unitless correction factor for adjusting wind speed at various heights above ground and terrain types, default=0.055.   
Rs  |  double  |  (p.u.)*Ohm  |  Synchronous generator primary stator resistance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
Rg  |  double  |  (p.u.)*Ohm  |  Synchronous generator grounding resistance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
turbine_height  |  double  |  meters  |  Specifies the height of the wind turbine hub above the ground. Default is 37 m.   
Turbine_Model  |  enumeration  |  N/A  |  Allows the use of one of the pre-defined generic turbines. Default is GENERIC_DEFAULT. 

  * GENERIC_DEFAULT
  * GENERIC_SYNCH_SMALL
  * GENERIC_SYNCH_MID
  * GENERIC_SYNCH_LARGE
  * GENERIC_IND_SMALL
  * GENERIC_IND_MID
  * GENERIC_IND_LARGE
  * VESTAS_V82
  * GE_25MW
  * BERGEY_10kW
  * GEN_TURB_POW_CURVE_2_4KW
  * GEN_TURB_POW_CURVE_10KW
  * GEN_TURB_POW_CURVE_100KW
  * GEN_TURB_POW_CURVE_1_5MW

  
Turbine_implementation  |  enumeration  |  N/A  |  Allows the user to specify the type of implementation for the wind turbine model. Default is POWER_CURVE. 

  * POWER_CURVE
  * COEFF_OF_PERFORMANCE

  
Wind_speed_source  |  enumeration  |  N/A  |  Allows the user to specify the source of wind speed. Default is BUILT_IN where a wind speed of 8 m/s (at 5-15m level) is used. 

  * BUILT_IN
  * WIND_SPEED
  * CLIMATE_DATA

  
WSadj  |  double  |  m/sec  |  Speed of wind at hub height. This is an output.   
Wind_Speed  |  double  |  m/sec  |  Wind speed at 5-15m level (typical measurement height). This is an output.   
wind_speed_hub_ht  |  double  |  m/sec  |  User-specified wind speed at hub height with a default value of 10 m/s. This is an input. Requires setting the parameter Wind_speed_source to WIND_SPEED.   
ws_rated  |  double  |  m/sec  |  Rated wind speed for generator operation. Used in COEFF_OF_PERFORMANCE implementation.   
ws_maxcp  |  double  |  m/sec  |  Wind speed at which generator reaches maximum Cp. Used in COEFF_OF_PERFORMANCE implementation.   
X_stator  |  double  |  (p.u.)*Ohm  |  Induction generator primary stator reactance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
X_rotor  |  double  |  (p.u.)*Ohm  |  Induction generator primary rotor reactance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
X_magnetic  |  double  |  (p.u.)*Ohm  |  Induction generator primary core reactance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
Xs  |  double  |  (p.u.)*Ohm  |  Synchronous generator primary stator reactance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
Xg  |  double  |  (p.u.)*Ohm  |  Synchronous generator grounding reactance in p.u. Used in COEFF_OF_PERFORMANCE based implementation.   
  
### Wind Turbine State of Development

The power curve-based implementation is included in the GridLAB-D v4.3 Release. The previous versions only support the coefficient of performance based model. This model is considered experimental and may not work as designed. 

## Model testing

[![](//images.shoutwiki.com/gridlab-d/thumb/e/e5/Generator_Module_Guide_Figure_2.png/300px-Generator_Module_Guide_Figure_2.png)](/wiki/File:Generator_Module_Guide_Figure_2.png)

[]

Figure 2 - Generator testing model

Note
    The matlab code ready but is waiting for Generator data.

  


## See Also

  * [Generator Module Guide]
  * [Generators (module)]
    * [Battery]
    * [Diesel dg]
    * [Energy storage]
    * inverter
    * [Microturbine]
    * [Power electronics]
    * [Rectifier]
    * [Solar]
    * [Windturb dg]
  * [Modules]

