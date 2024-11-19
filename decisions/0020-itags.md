---
status: proposed
date: → 2024-11-19
deciders: Patrick C., Alicia, Lee Ann, Emiley
consulted: Mark M, Sierra, Chris
informed: NMDC leadership team
---
# Minimally support amplicon data

## Context and Problem Statement

We frequently get requests to support amplicion data and have several studies which also have amplicion data (NEON, EMP 500, EcoFab 2.0 ring trail). 
Most recently this was requested by GLBRC collaborators. JGI does not sequence or have a workflow for amplicions and there are public workflows
avaliable. (QIIME)[https://github.com/qiime2/qiime2] is quite popular. 


## Considered Options

* Minimal model amplicon data
  Minimally modeling amplicion data would allow us to provide links at a biosample level for amplicion data without providing workflow records.
  Given limited resources this strikes a balance where a user would know amplicion data exists but without it being a core data type that NMDC provides
  support for.
* Fully model amplicion data
  This would include modeling amplicion data for data generation (DataGeneration), workflows (WorkflowExecution), and files (DataObjects).
* No modeling of amplicion data
  This is the current implmenation. Support for linking out to amplicion data is only provided via umbrella bioproject links on an NMDC study page. 

## Decision Outcome

Chosen option: Minimal model amplicon data, because it strikes a balance between resourcing and balancing user requests.
