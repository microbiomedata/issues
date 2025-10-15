---
status: accepted
date: 2025-09-24
deciders: Montana Smith, Sierra Moxon, Chris Mungall, Sujay Patil
consulted:
informed: 
---
# Decide on conventions for geographic location names.

## Context and Problem Statement

This surfaced as a problem when trying to submit NMDC metadata to NCBI (e.g., USA vs US vs United States of America). NCBI enforces country names based on INSDC conventions. We needed to decide if we would adopt these as well or if this should be handled on via export code. INSDC conventions can be found at https://www.insdc.org/submitting-standards/geo_loc_name-qualifier-vocabulary/. 


## Considered Options

* Keep existing metadata for country/region values specified in geo_loc_name and have export translator code handle making these changes.
* Adopt INSDC standards as an acceptable community standard for NMDC to use.

## Decision Outcome

Chosen option: "Adopt INSDC standards", because INSDC provides a standard we can point to with guidance. This was decided at the 9/24/2025 metadata meeting after discussion in an issues ticket (https://github.com/microbiomedata/issues/issues/1223).  How to implment this is still outstanding.

