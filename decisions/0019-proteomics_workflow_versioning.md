---
status: proposed 
date: 2024-11-14 
deciders: Alicia, Paul, Lee Ann, Katherine, Sam, Cam, Yuri
consulted: Mark M., Sierra
informed: Chris M.
---
# Subclass metaproteomics workflows

## Context and Problem Statement

Version 2 of the proteomics workflow is now avaliable. Version 2 uses Kaiko to generate a metagenome reference using machine learning if one does not exist. 
Version 1 of the workflow, which depends on an existing metagenome reference, will contintue to be run. How should these workflows be versioned? 
Existing workflow versioning convention is that if a workflow is return the typecode, shoulder and blade are the same and there is a version increment 
(ex nmdc:wfmp-11-abcd.1 for the first run and nmdc:wfmp-11-abdc.2 for the second run).  Details about NMDC identifies can be found (here)[https://microbiomedata.github.io/nmdc-schema/identifiers/].
Workflow identifiers are identified with a prefix, typecode, shoulder, blade, and version. For example identifier `nmdc:wfmp-11-abcd.1` `nmdc`
is the prefix, `wfmp` is the typecode, `11` is the shoulder, `abdc` is the blade and `1` is the version.


## Considered Options

* Subclass v1 and v2 of proteomics workflow
This option would provide different human readable typecodes to orient a user and it would be clearer that the most recent version of a true referenced based workflow run versus a in-silico 
based reference.  An example would be the first run is a reference-based (V1) with an identifier of nmdc:wfmprb-11-1231.1, the second is a version 2 (Kaiko) workflow run with an 
identifier of nmdc:wfmpis-11-1247.1, the third run there is an updated annotation and V1 is rerun. This would would get an identifer of nmdc:wfmprb-11-1231.2. 
an updated annotation and V1 is rerun. This would would get an identifer of nmdc:wfmp-11-1231.3 with the current modeling.
* Leave metaproteomics as a single workflow execution subclass with workflows sharing an identifier blade
This option does not provide any orientation to the user via a typecode and would be more complex to correctly version the workflows. This would either involve complex logic to look at the 
version to decide if to increment the version and for what blade. Or it would result in the two versions using the same identifier type code, blade and shoulder which would be confusing to the user.
An example would be the first run is a reference-based (V1) with an identifier of nmdc:wfmp-11-1231.1, the second is a version 2 (Kaiko) workflow run with an identifier of nmdc:wfmp-11-1231.2, the third there is 
an updated annotation and V1 is rerun. This would would get an identifer of nmdc:wfmp-11-1231.3 with the current modeling.
* Leave metaproteomics as a single workflow execution subclass with workflows have a different identifier blade
This option makes the most sense in terms of modeling because the workflows produce the same outputs and is consisent with previous decisons about
identifier transparency versus opaqueness.  Having different identifier blades addresses concerns about workflow versioning. Example: reference-based
workflow (v1) run number one would have an identifier of nmdc:wfmp-11-1231.1, a rerun would of that same workflow would be nmdc:wfmp-11-1231.2. The first run using
Kaiko to generate an in-silico reference (V2) mint a new identifier instead of incrementing the version on the existing identifier (ex nmdc:wfmp-11-1272.1), a rerun of V2 would have an
identifier of nmdc:wfmp-11-1272.1.


## Decision Outcome

Chosen option: Leave metaproteomics as a single workflow execution subclass with workflows have a different identifier blade, because it provides
consisisent modeling while also providing versioning that will not add confusion. This does add complexity to the code that will generate workflow metadata
records but this has been discussed and is an acceptable outcome. Schema work will be done to add information to make the make it clear on the workflow execution subclass record
if the analysis is reference based or if it uses an in-silico reference in microbiomedata/nmdc-schema#2256.

