# Residential (module) - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Residential_(module)
Simulates single-family homes with various appliances, electronics, and occupants. 

# Synopsis
    
    
    [module] residential;
    [module] residential {
      [default_outdoor_temperature] 74.0 [degF];
      [default_humidity] 75.0 [%];
      [default_etp_iterations] 100;
      [implicit_enduses] [LIGHTS]|[PLUGS]|[OCCUPANCY]|[DISHWASHER]|[MICROWAVE]|[FREEZER]|[REFRIGERATOR]|[RANGE]|[EVCHARGER]|[WATERHEATER]|[CLOTHESWASHER]|[DRYER];
      [house_low_temperature_warning] 55 [degF];
      [house_high_temperature_warning] 95 [degF];
      [thermostat_control_warning] [TRUE];
      [system_dwell_time] 1 [s];
      [aux_cutin_temperature] 10 [degF];
    }
    

# Classes

As of [Four Corners (Version 2.2)]

  * [house] – Single-family home model.
  * [residential_enduse] – Abstract residential end-use class.
  * [waterheater] – Typical residential water heating appliance.
  * [ZIPload] – Generic constant impedance/current/power end-use load.
As of [Hassayampa (Version 3.0)]
    These may be available in earlier versions but they have not been validated and are not supported.

  * [lights] – Typical residential lights.
  * [occupantload] – Residential occupants (sensible and latent heat).
  * [plugload] – Typical residential plug loads.
Unsupported
    These may be available in many versions but they have not been validated and are not supported.

  * [clotheswasher] – Typical residential clothes washing appliance.
  * [dishwasher] – Typical residential dish washing appliance.
  * [dryer] – Typical residential clothes drying appliance.
  * [evcharger] – Standard electric vehicle charger.
  * [freezer] – Typical residential freezing appliance.
  * [microwave] – Typical residential microwave appliance.
  * [range] – Typical residential cooking appliance.
  * [refrigerator] – Typical residential refrigeration appliance.
# Variables

  * [default_line_voltage] ([complex][3]) Incoming line voltage to use when no power objects are defined (default is 240V+0j,120V+0j,120V+0j).
  * [default_line_current] ([complex][3]) Line current across the outside energy meter (default is 0A+0j,0A+0j,0A+0j).
  * [default_outdoor_temperature] ([double]) Used when no climate/weather data is available (default is 74 degF).
  * [default_humidity] ([double]) Used when no climate/weather data is available (default is 75%).
  * [default_solar] ([double][9]) Used when no climate/weather data is available (default is 0,0,0,0,0,0,0,0,0).
  * [default_etp_iterations] ([int64]) Limits the number of iterations the ETP solver will perform before stopping (default is 100).
# Bugs

Due to parsing limitations on arrays [default_line_voltage], [default_line_current], and [default_solar] cannot be set from a GLM file. 

# See also

  * Residential module
    * [User's Guide]
    * [Appliances]
    * [house] class – Single-family home model.
    * [residential_enduse] class – Abstract residential end-use class.
    * [occupantload] – Residential occupants (sensible and latent heat).
    * [ZIPload] – Generic constant impedance/current/power end-use load.
  * Technical Documents 
    * [Requirements]
    * [Specifications]
    * [Developer notes]
    * [Technical support document]
    * [Validation]

