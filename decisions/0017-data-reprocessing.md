---
# Update these YAML values so they describe this decision. Delete the leading `→` characters.
status:  proposed
date:  2024-09-30
deciders:  Patrick C., Emiley, Alicia, Lee Ann 
consulted: Paramvir, Alex C., Amy, Natalia, Nikos, Shane, Paul, Yuri, Sam, Juila, Leah
informed: NMDC leadership
---
# Decide on NMDC workflow reprocessing guidelines

## Context and Problem Statement

Tools and versions change in the workflows and NMDC needs to determine when to reprocess datasets to ensure interoperablity. This must be balance by how much compute this costs, especially for the sequencing workflows. Currently the NMDC automation code will reprocess if the minor version is incremented, using sematic versioning. 


## Considered Options

* sync all project all the time with new versions of workflows

This is not feasible at this time given the compute constrains and balancing bringing in new datasets against reprocessing existing datasets. 
* reprocess on request only

This is certainly possible but at this point we have far less data than other organizations so this option seems too limiting at this time.
* yearly update, syncronized with user facility updates

This would work for annotation but other compoents of JGI's workflow aren't syncronized on the same schedule nor are EMSL workflows. 
* metric based

Out of scope for now, this would be an separate project to determine what metrics to use. There were a lot of interesting suggestions from the interviews related to focusing reprocessing on novel or not well characterized projects.
* ad hoc when there are major changes (ie change of tool). 

Use semantic versioning rules here to determine when datasets would be reprocessed. A major version change as it pertians to workflows would be a change in the main workflow tool. For example, MEGAHIT to rnaSPAdes for metatranscriptome annotation, updating from MetaBAT 2 to SemiBin.
* determine subset of results to reprocess

For example for compute intense workflows like metagenomes only reprocess high and medium quality metagenome-assembled genomes when there is a new version of KEGG.  For NMDC, this seems too restrictive for now. 

## Decision Outcome

Chosen option: "ad hoc when there are major changes", because this seems to strike the best balance between dataset interpoerablity and opportunity cost.

