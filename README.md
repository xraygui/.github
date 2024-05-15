# Nifty Beamline System (NBS)

This repository will aim to develop a complete system for running Bluesky-based X-ray beamlines at synchrotron light sources. This includes
* An opinionated beamline structure that organizes bluesky/ophyd objects
* A Bluesky-Queueserver based control package
* A GUI framework that takes advantage of this structure to make it easy to build custom controls for specific beamlines
* A simulation framework for building a "digital twin" of a beamline for GUI/controls testing
* A Viewer for data stored in Tiled

# Current Status
This entire system is a total work-in-progress. It is not ready for general use, but is being developed and re-worked constantly at the NSLS-II SST-1 beamline. Once it achieves a modicum of stability, it is our intention to run the system on several endstations and beamlines, and help anyone else who wants to get it running on theirs. Right now, it would be best to treat this as a highly experimental system with a completely unstable API that could change at any time.

# Advantages
## Config files
NBS uses config files to define and load a beamline, based on a generic control framework. This makes setup of new beamlines _fast_, as no new code needs to be written to get basic functionality up and running. Config files also allow for the QueueServer, GUI, and Simulation to be kept synchronized. Config files are a much better place to store configuration than in raw python code.

## Vertical integration
By defining a QueueServer, a GUI, a Simulation, and a Viewer, we can provide a control system that has several advantages over "generic" Bluesky. By assuming that every beamline has
* An energy-defining object
* A sample manipulator
* Detectors
* Motors
* Gate Valves
* Shutters
* Optics

We can produce standard "plans" that will work across all beamlines running NBS, and standard GUI tools that automatically expose the most important objects. Every beamline needs to move and scan energy, so why should every beamline have a different "spelling" of the move energy command? By explicitly declaring important classes of objects, we can provide a standard way to list motor positions, gate valve/shutter status, etc, and display them in a GUI. We can also record the _meaning_ of each object in metadata, so that groups looking at old data taken by that one PhD student who graduated 3 years ago don't have to guess the beamline structure from a set of inconsistently-named objects and PVs, which may have shifted from beamtime-to-beamtime as detectors and motors were swapped out.

If we adhere to a certain set of conventions for meaning, we can also ensure that the metadata always exists to make sensible plots directly from Tiled. 

# Disadvantages
## Specificity
This package aims to be _opinionated_. There will be beamlines doing odd things that do _not_ fit into this structure. This is fine. They may benefit from some of the more generic tools, but not the overall framework. The assumption is that you are trying to run an EPICS/Bluesky based beamline, with QueueServer, and that the universe of things you want to do is reasonably close to NEXAFS, which is what most of the "common" functionality is built around.

## Complexity
This package (so-far) sacrifices some clarity of code for generality. Object and plan loading has to happen in some unfortunately magical ways. The scan decorators that add a lot of built-in functionality to the basic built-in bluesky scans have probably gotten out of hand. Managing config files can get complex, and wind up just as bad as having to directly edit source code. As smarter people aid in development, we will hopefully try to increase the readability of the code.

# Current users
Development of this system is currently ongoing at NSLS-II/SST and BESSY-II/UE52.
