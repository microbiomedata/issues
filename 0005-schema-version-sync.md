---
status: proposed
date: 2023-01-26
deciders: Shreyas Cholia, Chris Mungall
consulted: Elais, Mike N, Mark M, Patrick K, Alicia, Donny, Jeff, Anastasiya, Set
informed: Leadership team
---
# Synchronize NMDC Schema Versions across NMDC infrastructure components

## Context and Problem Statement

Currently (2023-01-31) NMDC schema is not synced across different parts of the infrastructure. 
* Submission portal - v7.2
* Mongo DB / Runtime Production - v3.2
* Prod Data Portal DB (Postgres) - v3.2; 
* Dev Data Portal Migration testing - v7.0
* Workflows - v3.2 + v7.2 (for ID minting and new features)

This creates a lot of impedence and is holding back rollout of feature updates on data portal. We wish to 
1. bring all parts of the infrastructure up to a common version 
2. ensure a process that will enable future versions of NMDC schema to stay in sync

**Notes for Context**

* Submission portal 
  - Uses its own schema derived from NMDC-Schema, MIXS etc. (versions will be captured in Submission Portal Schema in the future)
  - Controlled by the sheets_and_friends repo.  
  - There is a process by which submission schema gets assembled (combination of NMDC, Mixs etc) 
  - Need for a script to convert back as we populate Mongo (Sujay)
  - Uses v7.2

* Mongo/Runtime 
  - Prod Mongo supports version 3.2 (May 2022).
  - Currently python dependency on recent version, but underlying schema use is using older version 3.2
  - Dev DB was tested with an updated dataset w v7.0
  - 7.3+ is required for metaB per Alicia. 

* Data Portal Postgres 
  - Currently prod data is at 3.2 
  - Ingest process version updates are manual, need code changes
  - Process
      + Run ingest in dev, manual testing on data portal
      + Run migration 
      + May need some changes to Postgres objects

* Workflows 
  - May be subsumed by Runtime
  - Turn off validation for older objects
  - Use mixed IDs for now.


## Considered Options

1. Use multiple schema versions in an ad-hoc manner
    - **Pros**: 
        + Components can operate independently 
    - **Cons**: 
        + This will require a lot of manual coordination and overhead, not sustainable
2. Use Schema v3.2 
    - **Pros**: 
        + Current version of production so no changes required to data portal or mongo
    - **Cons**:
        + Other components are already at newer version of Schema 
3. Use Schema v7.0
    - **Pros**: 
        + Data ingest between Mongo and Postgres tested
    - **Cons**:
        + Already behind latest schema release used by other components
4. Use latest released schema 
    - **Pros**: 
        + Allows us to bring all components to a common and latest baseline
    - **Cons**:
        + May not support features currently in dev
        + Will need to institute a process to coordinate future feature upgrades.
5. Use future release 
    - **Pros**: 
        + Allows us to bring all components to a common baseline
    - **Cons**:
        + Will require us to wait and we will always be an ongoing concern

## Decision Outcome

Chosen option: 4. Use latest released schema 
(v7.4.4 at the time of writing - exact version will be determined during implementation)

This option allows us to bring all components to a known, released NMDC schema version.
We will also institute a process whereby all future NMDC schema releases will be done in coordination with Submission Portal, NMDC Runtime Mongo, Data Portal DB, Workflows. 

Effectively *a new schema version will always be accompanied by a coordinated process to release it across the major system components.*

Schema updates that will impact 

We recommend forming a working group or squad to coordinate initial syncing activity, as well as future release management. We may need additional planning around how this group operates.  

**Process:** Schema Updates that impact API and Data Portal should be coordinated using an issue ticket that tags representatives from the NMDC Server, NMDC Runtime and NMDC Schema groups. The issue should only be closed once all affected areas are updated. 

