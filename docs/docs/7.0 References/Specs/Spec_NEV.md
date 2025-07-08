# Spec:NEV

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Spec:NEV
SPECIFICATION Approval item: 

## Contents

  * 1 Overview
  * 2 Terminology
  * 3 Sections
  * 4 See also
# Overview

The Neutral-Earth Voltage implementation of GridLAB-D will require many changes to the underlying [powerflow] source code. These specifications outline the expected implementation and hope to resolve any conflicts prior to actual coding. 

# Terminology

To ensure all specifications are compatible and discussions are consistent, the following terms will be utilized for NEV-related discussions and specifications. 

Table 1 - NEV Terminology  Term | Definition   
---|---  
Bus | Connection point of link-based objects, representing a point of voltage potential. Interchangeable with node.   
Connection | A specific wire or relationship between two phases of distinct buses/nodes/terminals. e.g., the wire representing phase A between two nodes.   
Link | Connection between two distinct buses or nodes. A physical, specific connection implementation (e.g., a 3-phase line). Can be composed of multiple phases, so long as it is consistent between the from bus/node and to bus/node (i.e., link can't change form between two buses/nodes).   
Load | An electrical path between two distinct phases/terminals on the same bus that consumes/produces some form of power. Under old [powerflow] implementations, this was a current, impedance, or power shunt between a phase and an implied zero-potential reference. A line-to-ground fault could be implemented as a low-impedance `load` connection between a terminal and the ground plane.   
Node | Connection point of link-based objects, representing a point of voltage potential(s). Interchangeable with bus.   
Phase | An individual voltage potential specification on a node or bus. Phase "A" is a very specific implementation, but this will be more generalized for the NEV-related implementation. This will be numeric based for new implementations (e.g., ABC could be 1,2,3). Phase points are defined as terminal connections within the NEV solver framework.   
Terminal | A generalized, individual voltage potential specification on a node or bus. Synonymous with "Phase" under the ABC convention, this will be referenced by number instead. 64 unique phase "specifications" will be supported for flexibility. e.g., phase A could be phase 1, phase B could be phase 2, etc.   
  
# Sections

The following pages will contain specifications for individual aspects of the NEV solver implementation. Note that these pages are not stand-alone and a significant amount of overlap is expected. Please be sure to review all specifications pages. 

[NEV Data Formatting]

[NEV Link Objects]

[NEV Node Objects]

[NEV Solver Implementation]

[Other Module Interactions]

# See also

  * [Overview Page]
  * [Requirements]
  * [Implementation]
  * [Keeler (Version 4.0)]

