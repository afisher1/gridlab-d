# Modules

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Module_Guide

---
 
 
GridLAB-D implements classes and solvers in modules. Modules can be either prebuilt and delivered as a `dll`, `so`, or `dylib` (depending on your computer platform and operating system), or they can be encoded directly in a [GLM] file as a [runtime class]. 

The currently supported modules are 

  * [ Climate ]: contains weather data and reads TMY2 files
  * [ Market ]: provides wholesale market simulation and responsive appliance controllers
  * [Powerflow]: simulates the distribution level power grid models
  * [ Residential ]: models a single-family home and various home appliances
  * [Reliability]: runs one-off reliability analysis on Powerflow models
The currently unsupported modules are 

  * [ Generator]: DG models act as a load on the system.
  * [Commercial]: models office buildings
  * [Communications]: models communications networks
  * [GLJava]: provides JNI interface for loading modules written in Java
  * [Matlab]: provides a Matlab interface for defining classes in Matlab. This is superseded by the [Matlab link] core function.
  * [MatPower]: solves optimal power flow using the MATPOWER solver
  * [Network]: models balanced electric networks
  * [PLC]: models programmable logic controllers
The following modules provide debugging and I/O functions 

  * [Assert]: contains objects that are used in test modules by breaking the simulation if observed values deviate from expected values.
  * [Tape]: object boundary condition I/O module
# See also

  * [Dev:Module]
  * [module (directive)]

