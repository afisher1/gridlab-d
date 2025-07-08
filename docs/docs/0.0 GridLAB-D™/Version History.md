---
title: Version History
excerpt: >-
  This page provides an archive of GridLAB-D version announcements.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Announcements

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Announcements


_**Announcements**_

GridLAB-D v4.3 ([Navajo (Version 4.3)]) is now available!
    Windows and Linux builds can be found at [GridLAB-D downloads](https://www.gridlabd.org/downloads.stm//). The Mac release is coming soon. In the mean time, GridLAB-D v4.3 can be built from the source code at [GridLAB-D GitHub Repository](https://github.com/gridlab-d/gridlab-d//).

_**Previous Announcements**_

GridLAB-D v3.2 ([Jojoba (Version 3.2)]) is now available!
    Windows, Linux, and Mac can be found at [GridLAB-D v3.2](http://sourceforge.net/projects/gridlab-d/files/gridlab-d/Last%20stable%20release//).

[Jojoba (Version 3.2)] candidate release (November 15, 2015)
    This candidate release is for Windows, Linux and Mac OSX. The downloads can be founds at [Candidate Release](http://sourceforge.net/projects/gridlab-d/files/gridlab-d/Candidate%20release//)
    Release notes are available at: [History].

GridLAB-D v3.0 is now available!
    Windows, Linux, and Mac can be found at [GridLAB-D v3.0](http://sourceforge.net/projects/gridlab-d/files/gridlab-d/Last%20stable%20release//).

GridLAB-D v3.1 is branched
    It was branched from v3.0 and ticket 730 for completion, testing, and validation in Jan. 2014. Currently, the goal is to release v3.1 by Summer 2014. It will included expanded delta-mode and microgrid capabilities, expanded hardware-in-the-loop capabilities, DG controllers, and multi-threading support, among others. Feel free to see progress in the [source tree](http://sourceforge.net/p/gridlab-d/code/HEAD/tree/branch/3.1/).

GridLAB-D v3.0 Release Candidate is now available! (posted 10/1/2013)
    Release candidates for Windows, Linux, and Mac can be found at [Candidate Release](http://sourceforge.net/projects/gridlab-d/files/gridlab-d/Candidate%20release/). Please report any bugs or concerns.

[GridLAB-D Association] Formation Authorized (posted 20 November 2011)
    The US Department of Energy has authorized PNNL to begin the legal process of forming an association of GridLAB-D stakeholders, including interested parties from user, vendor, academic and government communities to help direct and fund the growth of GridLAB-D. Contact [David Chassin (PNNL)](mailto:david.chassin@pnnl.gov?subject=GridLAB-D%20Association%20Inquiry) for details.
    **Update** \- Initial kickoff meeting was held in July 2013 - for more information or to join the GridLAB-D Association, please contact the [GridLAB-D Team](mailto:gridlabd@pnnl.gov?subject=GridLAB-D%20Association%20Inquiry)

[Grizzly (Version 2.3)] Launched (posted 10 November 2011)
    A new release of GridLAB-D was started by PNNL. This release is will incorporate support for microgrids and advanced multi-objective controls. Contact [Jason Fuller (PNNL)](mailto:jason.fuller@pnnl.gov?subject=Version%202.2%20Inquiry) for details.

[Four Corners (Version 2.2)] Released (posted 28 September 2011)
    A new release of GridLAB-D was released for general use. It supports the American Recovery and Reinvestment Act (ARRA) Smart Grid Investment Grant (SGIG) analyses performed by PNNL for the US Department of Energy. See [Downloads](http://sourceforge.net/projects/gridlab-d/files/) for details.

_**Release Notes**_


**Coulee** is the branch of GridLAB-D that led to Version 1.2. It included a number of cross-platform releases of Version 1.1 ([Buckley]). 

**Coulee** is the branch of GridLAB-D that led to Version 1.3, which was never released. This version was begun in May 2008 and never released. This developer-only version included preliminary support for 

  * Distribution reliability metrics calculations
  * Power markets
  * Commercial multi-zone office modeling
  * Distribution automation
  * Control network modeling
  * Java runtime modules
  * KML support (Google Earth data files)
  * XSD, XSL, and CSS support (XML stylesheets)

**Eldorado** is the branch that led to Version 2.1, which was release in November 2010. In included the following capabilities: 

  * Linux support include (RH/Fedora, Ubuntu)
  * Mac OS X support
  * Windows 64 bit support (w/o runtime classes)
  * Support for schedule skews
  * Improvements to markets and controllers

**Hatwai** (Version 3.1) is a minor upgrade of GridLAB-D released September 30, 2014. 

  * [New capabilities] (these capabilities were first implemented in this version)
  * [Approved capabilities] (these capabilities were approved for implementation in this version)
  * [Proposed capabilities] (these capabilities were proposed for implementation in this version)
  * [Tickets](http://sourceforge.net/p/gridlab-d/tickets/milestone/Version%203.1%20Stable/) (tickets relating specifically to the development of this version)


**Navajo** (Version 4.3) is a minor upgrade of GridLAB-D planned for Fall 2020. Changes and improvements in Navajo will be outlined here. 

* *New Capabilities*

  * Ability for global [ #define] statements to take unit values - [deltamode_timestep] utilizing
  * Addition of global to force complex output format
  * Addition of internal load order for [ deltamode] modules, to make sure things execute in proper order
  * Addition of three-phase or higher-voltage connectivity to the [house] model
  * Addition of HELICS-interfaceable event field to [ eventgen] object
  * Improvement to [house] to enable adding the powerflow [ induction motor] to house HVAC models
  * Improvement to evcharger_det to include simple subsecond capabilities and simple SAE J2894 capabilities
  * Addition of powerflow flag to convert models to constant impedance at particular voltage triggers (outside in-rush)
  * Improvements to multi-island powerflow capabilities, including the ability to solve them separately
  * Improvements of wind power capability curve
  * Addition of [inverter_dyn] object - a new microgrid/dynamics-oriented inverter
  * Addition of DC model to [solar] object for use with [inverter_dyn]
  * Addition of [sync_check] object
  * Addition of ability to export the right-hand-side vector as part of the NR matrix dump
* *Bug Fixes*

  * Performance improvements to NR solver
  * Fix to constant current implementations in load objects to be more intuitive and flexible to off-nominal angles (migration of deltamode capabilities)
  * Fixes to compilation process and files to better handle MSYS2 and cross-platform consistency
  * Fix for `_err` autotest checking so a failure to fail is not considered a success
  * Fixes for HVAC operation with a tripped breaker
  * Fixes to diesel_dg and existing inverter object for deltamode initialization
  * Fixes to frequency calculation methods to better handle deltamode/QSTS transitions
  * Fix so base [line] objects can take matrix definitions, though not encouraged
  * Fix for normalized schedules
  * General clean-up of generator and powerflow objects and headers for new API and eliminate erroneous inclusions
  * Tickets (tickets relating specifically to the development of this version) 
    * [Tickets for 4.3 RC1](https://github.com/gridlab-d/gridlab-d/milestone/26)

**Lugo** (Version 4.1) is a minor upgrade of GridLAB-D planned for October 2018. Changes and improvements in Lugo were focused primarily on the deltamode transient analysis (microgrids) and FNCS/HELICS integration, with several minor features and fixes incorporated. 

* New Capabilities

  * FNCS and HELICS interfaces incorporated 
    * Several bug fixes and improvements to the interfaces
    * Includes deltamode capabilities with FNCS/HELICS
  * MySQL module 
    * Improved writing mechanism to reduce overhead
    * Several usage fixes
  * Generator module 
    * Cleaned up several deprecated objects that had no functionality
    * Inverter has had significant improvements to its functionality 
      * Deltamode improvements for better transient simulations 
        * Ability to operate as a voltage source inverter and be grid forming 
          * Operate in either isochronous or droop mode
        * Droop controller for f/P and V/Q in grid following implementations
        * Ramping limits to reflect power electronics slew limitations
      * Improvements to the inverter IEEE 1547 and IEEE 1547a functionality
      * New Volt/Watt mode
    * Diesel generator has had improvements to its functionality 
      * Simplified steady state model to remove inaccurate model
      * Simple overload checking incorporated 
        * Deltamode improvements for better transient simulations 
          * Added convergence limits for voltage to help control deltamode simulation intervals
          * Simplified set point variables for frequency, voltage, real power, and reactive power dispatch outputs
          * Implemented new control modes for resiliency analysis 
            * Constant reactive power dispatch mode
            * Constant real power dispatch mode
            * CVR method for transient reduction, per [this paper](https://doi.org/10.1109/TPWRS.2018.2859742)
          * Simplified fuel use and emissions calculations
  * Powerflow module 
    * Fixes for tape-shielded cables
    * Multiple islands supported, including the ability for an island to "fail" and not terminate the simulation
    * Tiered-TOU pricing fixed for triplex_meter
    * Induction motor models 
      * Single-phase and three-phase implementation
      * Deltamode support
      * Simple protection schemes (thermal trip and under-voltage contactor)
    * AMI-like fields for meter and triplex_meter
    * fault_check incorporated SINGLE_DEBUG to help track down feeder topology problems
    * Deltamode capability improvements 
      * Capacitor objects now can update in deltamode
      * Regulator objects now can update in deltamode
      * Separate integration method selections for series vs. shunt impedance and in-rush calculations
    * Improved GFA functionality in node-type objects
  * Tape module 
    * Deltamode support for group_recorder
    * New object - metrics_collector
  * Build/Support 
    * Migration to MSYS2-based environment for Windows compiling
    * Updated incorporated superLU MT version to 3.1
    * Updates to build numbering to better reflect GitHub hash system
    * Migration of several objects to GLD API
  * Tickets (tickets relating specifically to the development of this version) 
    * [Tickets for 4.1 RC1](https://github.com/gridlab-d/gridlab-d/milestone/22)

**Keeler** (Version 4.0) is a major upgrade of GridLAB-D planned for some time in 2015. 

  * [New capabilities] (these capabilities were first implemented in this version)
  * [Approved capabilities] (these capabilities were approved for implementation in this version)
  * [Proposed capabilities] (these capabilities were proposed for implementation in this version)
  * [Tickets](http://sourceforge.net/p/gridlab-d/tickets/milestone/Version%204.0%20Stable/) (tickets relating specifically to the development of this version)

  **Jojoba** (Version 3.2) is a minor upgrade of GridLAB-D planned for late 2015. 

  **TODO** 

  * [New capabilities] (these capabilities were first implemented in this version)
  * [Approved capabilities] (these capabilities were approved for implementation in this version)
  * [Proposed capabilities] (these capabilities were proposed for implementation in this version)
  * Tickets (tickets relating specifically to the development of this version) 
    * [RC1](http://sourceforge.net/p/gridlab-d/tickets/milestone/Version%203.2%20RC1/)
    * [Stable](http://sourceforge.net/p/gridlab-d/tickets/milestone/Version%203.2%20Stable/)