# GridLAB-D Documentation Outline

The following is a conversion of the outline originally written by Trevor Hardy and serves as a proposed table of contents for the final documentation. Feel free to edit and add details as needed.

## 0.0 GridLAB-D

### Splash or Landing Page (Not gridlabd.org)
 A few paragraphs and/or a few-minute introductory video describing what GLD is and what people use it for. Lots of pictures, easy to digest, accessible to as broad an audience as possible while answering the question “Is GLD useful for me? Should I keep reading?” 

### Quickstart Guide

#### Installing pre-build binaries 

#### Running existing example 

#### Link to support pages 

### Installation and Compliation Guide

#### Installing and Validating Pre-Built Binaries

#### Building from Source

### Support Page
- Email address 
- Github discussions
- Chat-with-GLD-experts server (Discord, Gittr) 
- Link to example suite
- Link to GLD autotests 
- Link to source code repo


### Publications
- Citing GLD
  - Current preferred citation
  - Historical citations

## 1.0 Prospective Users

### Technical Overview
A few pages, describing what GLD can do in more detail. This section is for those that are pretty sure GLD can do something they want and they want to make sure. Still lots of graphics but meant to be browsed and read in 15 minutes or less.

Probably each of the sections below has one or more links to dedicated content elsewhere if the reader wants to know more.

#### How GLD models the power system and how that’s expressed in a .glm 
- Object definition through parameters 
- Object synchronization through time 
- Quasi-static time series (not true dynamics)

#### Commonly Used Functionality
Showing off the popular integrated models and the interaction between the devices that makes GLD a great platform for doing this kind of modeling.

##### Powerflow

##### Single-zone Structure Model (House)

##### Smart Grid Devices
- Single-zone structure with HVAC (nominally residential home) 
- Rooftop solar 
- Battery 
- EV 

##### Three-Phase Unbalanced Transients (Deltamode)

##### Co-Simulation with HELICS

##### libgld
Using GLD as an engine in a larger application. Make your own GUI, make a purpose-built script that runs your analysis, use other models as part of the GLD simulation.

## 2.0 New Users
- Start with installation instructions
  - make sure to include the .exe easy installto get people up and running quickly
  - also include full install
  - link to build instructions, but those live in dev section
- Demo that starts with quick install and running a small, simple model
- Simpler theory of operation doc--existing one seems more like a dev page



## 3.0 Modeler Guide
Where does delta mode fit? Where is it first introduced, where is the bulk of the content?
- consult with dev team about role of delta mode


### Who is a “modeler” and what does this guide intends to provide? 

### Class hieararcy and property inheritance

### GLD Model File Formats

### Device models
One page for each model listing all properties and what they do. Often there are multiple properties that are related so we might want to create sub-sections around functionality and describe the related parameters there.

Link to any related examples in "6.0 Examples"

### Non-device models

#### Tape Module objects

#### Clock

#### Message

#### Market objects

#### Other Objects Trevor is forgetting

### Verifying or Debugging a Model
- What are the red flags?
- Common warnings that can be avoided
- If we make validation scripts, how to use them


## 4.0 Developer 

### Who is a “developer” and what this guide intends to provide 

### GLD object synchronization process
- UML sequence diagram?

### Parallelization implementation 

### Loader 

### Device Model Structure

### libgld

### Contributing

#### Style Guide

#### Running and Adding tests

#### Submitting a PR

#### Doxygen API documentation (largely auto-generated) 
Does this make a class hierarchy diagram automatically or do we need to add one in separately?


## 5.0 Integrator Guide

### Who is an “integrator” and what this guide intends to provide 

### Overview of libgld

### Common APIs

#### Loading and running a model, pausing execution

#### Setting a property value 

#### Getting a value out from the model (APIs to make your own Tape module)

### Common Applications of libgld
Just links to things in "6.0 Examples"?

### Doxygen API reference?
Somewhere we need to have a comprehensive list of the APIs Integrators can use 



## 6.0 Examples
Collection of examples each with their own documentation page and supporting files (.glms, weather, etc). Ideally these are tested as part of some kind of CI/CD thing so we can know they always work. Ideally we have one dedicated example for each feature.