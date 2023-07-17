# CyFi Geant4 code
## Table of contents
* [Introduction](#Introduction)
* [Structure](#Structure)
* [Setup](#Setup)

## Introduction
This code is for the GEANT4 simulation of squared scintillating fibers wrapped in cylindrical geometry

This Class is to be include in a full blown simulation to create the muEDM scrintillating fibers tracker geometry


## Structure
CyFi contains:
* inc
    * CreateCyFi.hh
    * CreateHelix.hh
* src
    * CreateCyFi.cc
    * CreateHelix.cc
	
CreateHelix creates a namespace with few functions to generate a squared helix.

CreateCyFi calls the functions from CreateHelix and, given the geometry parameters, places the tracker geometry (with SiPMs https://github.com/b-vitali/SiPM.git) in the LogicWorld by ` CyFi->Create(fLogicWorld); `

## Setup
How do you implement this in your own simulation?

Let's assume a sim fodler, containing scr, inc, and a build folder.

After cloning the repository you will have an additional folder with its own scs and inc.

```
$ git clone https://github.com/b-vitali/CyFi_CHeT.git
```

Then you need to include these files in your CMakeLists.txt

It should look something like this:

```
include_directories(
    ...
    ${PROJECT_SOURCE_DIR}/CyFi/cc
    ${PROJECT_SOURCE_DIR}/CyFi/inc
    ...
    ${PROJECT_SOURCE_DIR}/inc
    ${Geant4_INCLUDE_DIRS}
    ${ROOT_INCLUDE_DIRS}
)

file(GLOB sources 
    ...
    ${PROJECT_SOURCE_DIR}/CyFi/src/*.cc
    ...
    ${PROJECT_SOURCE_DIR}/src/*.cc
)

file(GLOB headers 
    ...
    ${PROJECT_SOURCE_DIR}/CyFi/inc/*.hh
    ...
    ${PROJECT_SOURCE_DIR}/inc/*.hh
)
```

At this point you need to ` #include "CreateCyFi.hh" ` and call it.

You first need to initialize the class passing the geometry information.

Than you call the create function passing as argument the pointer to the LogicWorld.
```
CreateCyFi * CyFi = new CreateCyFi(15*cm, 3.5*cm, 1*mm, 55*deg, 0*deg); 
CyFi->Create(fLogicWorld);
```

