# NEV

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/NEV
Implement neutral earth voltage and generic capabilities into the [powerflow] module. 

# Synopsis

This work will incorporate the ability to model neutral-earth voltages and other more advanced powerflow capabilities into GridLAB-D. Expected release is in [Keeler (Version 4.0)]. 

# Classes

    [Overhead Line Equations]
    [Underground Line Equations]
    [Load Equations]
    [Transformer Equations]

## Protection Devices

Protection devices, i.e. circuit breakers, reclosers, sectionalizers, fuses, and switches, are used to protect the system from dangerously high currents flowing through the system. They are used to break the connection typically on the 3 phases of a distribution line. So while these devices look like a link for load carrying phases of the system they don't necessarily break a neutral line. Some devices do require a ground connection for the electronics contained within however this is a single contact not a link type connect where there is a ground at one end of the device and an ground at the other end of the device. When these devices are closed the impedance across them is very negligible so any cross coupling between phases can be ignored. When these devices are open they are open circuits for the phases they are connected to. 

# Developer info

  * [Requirements]
  * [Specifications]
  * [Implementation]
# See also

  * [Modules]
  * [powerflow]
  * [Keeler (Version 4.0)]

