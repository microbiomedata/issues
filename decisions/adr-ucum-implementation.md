---
status: accepted
date: 2025-07-01
deciders: Chris Mungall, Montana Smith, Mark Miller, Katherine Heal, 
consulted: Samantha Obermiller, Bea Meluch, Patrick Kalita, Lee Ann McCue, Sujay Patil, Alicia Clum
informed: 
---
# UCUM implenetation in NMDC

## Context and Problem Statement

The NMDC has developed a LinkML schema with slot ranges of QuantityValue to structure data into a {value}{unit} format. Many slots are assigned this range. Additionally, many slots have designations for the expected unit. For example, a slot providing a measurement of distance would require QuantityValue format, but could provide unit 'banana' if someone so chose! More realistically, the variability of cm, centimeter, m, meter, or metre would also be acceptable. This is not standardized and diminishes data interoperability and discoverability.
To address this, the NMDC will adopt the [Unified Code for Units of Measure (UCUM)](https://ucum.org/). This will ensure consistancy across all measurement values and provide clear guidance and expectations for all incomming data.
This ADR outlines:
- what existing NMDC (meta)data was migrated and how existing data was restructred into a UCUM valid format
- how annotations was used to require UCUM units, slot requirement, and provide how to new slots added to NMDC schema will need formatted to comply
- how data ingested into NMDC can be checked and validated.
- what ETL updates will be needed for NMDC's data ingest. (this work still to be completed)
- the updates made to submission portal ingest and submission portal validation

<!-- This is an optional element. Feel free to remove. -->
## Considered Options

* {title of option 1}
* {title of option 2}
* {title of option 3}
* … <!-- numbers of options can vary -->

## Decision Outcome

Chosen option: "{title of option 1}", because
{justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

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
