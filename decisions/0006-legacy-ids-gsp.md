---
status: {accepted}
date: {2023-01-25}
deciders: {Alicia, Donny, Sam, Mark M., Cam, Chris M. Elais, Grant, Mark F., Patrick K, Set, Simon, Sujay, Yan, Bin}
consulted: {}
informed: {Shreyas}
---
# How to handle legacy NMDC identifiers?

## Context and Problem Statement

NMDC now has a minting service to create NMDC identifiers. This brought up the question of what to do with the legacy identifiers and on what timescale Identifiers are in the mongo database but also embedded in analysis file names and within those files as headers for contig names, protein names, etc.



## Considered Options

* Replace legacy identifiers with new identifiers prior to GSP 2023
* Relax schema validation to allow for a mix of old and new identifiers until after GSP 2023
* Reprocess data using new identifiers prior to GSP 2023

## Decision Outcome

Chosen option: "Relax schema validation to allow for a mix of old and new identifiers until after GSP 2023", because it was determined in the NMDC weekly sync that replacing identifiers would not be a trivial exercise and should not be 
done before GSP 2023 given other prorities. Instead the schema validation rules will be relaxed until after GSP 2023 
with the majority of staff in favor of reprocessing data which would mint new identifiers instead of an ad hoc, potentially
error prone process of replacing identifiers in existing records.

