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
- `env_broad_scale` will terms that branch from be from ‘biome’ only.
We will’ evaluate what is lost when ecosystem is excluded and determine what needs “re-added” or requested of envo
Env_loca_scale will be material entity - biome - environmental material & narrowed from there. See 3.ii above
Env_medium will be environmental material
For SOIL extension specifically
Env_broad_scale will be biome - aquatic biome then human reviewed
Split leaf nodes & non-leaf nodes.
Review these lead and non-leaf nodes for elimination.
Expect leaf nodes to be removed.
Env_local_scale, see 3.ii above
Env_medium will be all environmental materials with soil or soils in their name or description. Possible stemming if needed.


<!-- This is an optional element. Feel free to remove. -->
### Consequences

* Good, because {positive consequence, e.g., improvement of one or more desired qualities, …}
* Bad, because {negative consequence, e.g., compromising one or more desired qualities, …}
* … <!-- numbers of consequences can vary -->

<!-- This is an optional element. Feel free to remove. -->
## Validation

{describe how the implementation of/compliance with the ADR is validated. E.g., by a review or an ArchUnit test}

<!-- This is an optional element. Feel free to remove. -->
## Pros and Cons of the Options

### {title of option 1}

<!-- This is an optional element. Feel free to remove. -->
{example | description | pointer to more information | …}

* Good, because {argument a}
* Good, because {argument b}
<!-- use "neutral" if the given argument weights neither for good nor bad -->
* Neutral, because {argument c}
* Bad, because {argument d}
* … <!-- numbers of pros and cons can vary -->

### {title of other option}

{example | description | pointer to more information | …}

* Good, because {argument a}
* Good, because {argument b}
* Neutral, because {argument c}
* Bad, because {argument d}
* …

<!-- This is an optional element. Feel free to remove. -->
## More Information

{You might want to provide additional evidence/confidence for the decision outcome here and/or
 document the team agreement on the decision and/or
 define when this decision when and how the decision should be realized and if/when it should be re-visited and/or
 how the decision is validated.
 Links to other decisions and resources might here appear as well.}
