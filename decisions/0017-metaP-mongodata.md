---
# Update these YAML values so they describe this decision. Delete the leading `→` characters.
status: accepted 
date: 2024-10-06
deciders: Shreyas Cholia, Katherine Heal, Alicia Clum, Sam Purvine
consulted: Paul Piehowski, Lee Ann McCue, Mark Miller
informed: Everyone in Metadata meeting
---
# Metaproteomics results data should not be in Mongo
## Context and Problem Statement

There are large mongo data records for metaproteomics workflow activities, this causes issues for the API and elsewhere and is not sustainable. 


## Considered Options

* **Rework Aggregation Scripts**

Rewrite the aggregation scripts to access the Metaproteomics processed data (not just the mongo records about those data).

* **Bypass Aggregation Scripts**

Modify the Metaproteomics workflow to directly write the FunctionalAnnotationAggMember records (originally written by aggregation script).


## Decision Outcome

Chosen option: "**Rework Aggregation Scripts**", to maintain modularity of the aggregation process.

We will also rewrite the aggregation script to use the API rather than directly access mongoDB.

