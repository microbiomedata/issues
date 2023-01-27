---
status: proposed
date: {YYYY-MM-DD when the decision was last updated}
deciders: {list everyone involved in the decision}
consulted: {list everyone whose opinions are sought (typically subject-matter experts); and with whom there is a two-way communication}
informed: {list everyone who is kept up-to-date on progress; and with whom there is a one-way communication}
---
# Synchronize NMDC Schema Versions across NMDC infrastructure components

## Context and Problem Statement

Currently NMDC schema is not synced across different parts of the infrastructure. 
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
  - 7.3 is required for metaB per Alicia. 

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
    - This will require a lot of manual coordination and conversions. 
2. Use Schema v3.2 
3. Use Schema v7.0
4. Use latest released schema v7.3
5. Use future release 

## Decision Outcome

Chosen option: "{title of option 1}", because
{justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

