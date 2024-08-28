---
status: proposed / in progress
date: 2024-08-28
deciders: Montana Smith, Mark Miller, Sierra Moxen
consulted: Lee Ann McCue, Natalie Winans
informed: 
---
# Establishing valid terms for MIxS Environmental Triad 

## Context and Problem Statement

Determining what to assert for each of the environmental triad slot (`env_broad_scale`, `env_local_scale`, and `env_medium`) is difficult.
NMDC will provide a reporducible logic to provide users with a curated lists of valid terms for each slot.

These terms will not be limiting, a user can still use string match to enter terms. This is a temporart allowance until user research and refinment of these queries is completed.
Any term provided by a user that does **not** come from the provided list will be evaluated.

*This ADR is in progress. Initialy plan has been outlined here & may change*

## Decision Outcome

To provide a reusable method of establishing valid terms, NMDC will provide rules for "general" allowed terms. Meaning the ontology terms that are identified using the "NMDC General Query" could be applied to **any** environmental sample type (MIxS Extensions).

To ensure interoperability and consistancy, the NMDC General Query will be ammended for each environmental sample type.
- To satisfy NMDC needs, we will start here with soil.
- This ADR will be updated as environment specific queries are created.
- No environment specific query should be "cherry picked". Rather any filtering should be accomplished as a general query. (Example, will not remove a specific term by ID, but rather identify what is the rule that can be created to fit a overall need.)

NMDC General Query
- `env_broad_scale` will be composed of terms that branch from biome [ENVO:00000428]
  - We will then evaluate what is lost when ecosystem is excluded and determine what needs “re-added” or requested of envo
- `env_local_scale` will be composed of terms that branch from material entity [BFO:0000040], excluding biome [ENVO:00000428] & environmental material [ENVO:00010483]
  - Additional narrowing will occur to remove specific branches. 
- `env_medium` will be composed of terms that branch from environmental material [ENVO:00010483]

For **soil** environment (MIxS Extension)
- `env_broad_scale` will exclude aquatic biome
  - Included terms will be separated into leaf nodes & non-leaf nodes & evaluated manually and additional query changes will be made as needed.
  - Expectation is that lead nodes will be removable.
- `env_local_scale` ????? TBC
- `env_medium` will be textually filtered for classes with soil or soils in their name or description.
  - Will evaluate if adding stemming is needed.

Following these initial queries and human evaluation, this ADR will be updated. 

## More Information

Referece the squad meeting notes.
https://github.com/microbiomedata/issues/issues/468
https://github.com/microbiomedata/issues/issues/840
https://github.com/microbiomedata/issues/issues/841
https://github.com/microbiomedata/issues/issues/877