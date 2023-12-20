---
status: proposed {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)}
date: 2023-12-19{YYYY-MM-DD when the decision was last updated}
deciders: Chris Mungall, Lee Ann {list everyone involved in the decision}
consulted: Anastasiya Prymolenna, Brynn Zalmanek, James Tessmer, Montana Smith, Sam Purvine, Yuri Corilo, Mark Miller, Michael Thorton, Alicia Clum, Mark Miller    {list everyone whose opinions are sought (typically subject-matter experts); and with whom there is a two-way communication}
informed: Emiley Eloe-Fadrosh, Shreyas Cholia, Eric Cavanna {list everyone who is kept up-to-date on progress; and with whom there is a one-way communication}
---
# Schema Refactoring: Monterey//Berkeley

## Decisions Made and Implemented
* Class:OmicsProcessing will be removed
  * All reference to Class:OmicsProcessing should be eliminated from NMDC schema & Workflows
* Class:DataGeneration will be created and take the place of Class:OmicsProcessing
* Class:PlannedProcess will be created as a root class
  * Class:MaterialProcessing will be created and be a subclass of Class:PlannedProcess
  * Class:WorkflowExecution will replace Class:WorkflowExecutionActivity and will be a subclass of Class:PlannedProcess
* All 'Activity' classes are now of Class:PlannedProcess, and the word "Activity" will be removed
* To group subclasses of Class:WorkflowExecution into implemented steps, Class:WorkflowChain was created.
  * This also provides clear connection back to the DataGeneration output, using was_informed_by
    * was_informed_by is being removed from Class:WorkflowExecution
  * Any implementation of Class:WorkflowExecution will use Class:WorkflowChain. Even when it's a single linked chain (ie implementation of a single subclass of Class:WorkflowExecution)
* Class:WorkflowExecution will provide part_of with Range:Class:WorkflowChain
* Output from WorkflowExecution will always have slot:data_category: processed_data of Class:DataObject 
* Class:DataGeneration will detail the process of inputting a material sample (Biosample or ProcessedSample, some MaterialEntity) into an instrument and generating data (output)
  * Instrument data will be denoted using slot:data_category on Class:DataObject.
  * The output of DataGeneration should always be instrument_data
* Class:DataGeneration will have subclasses Class:NucleotideSequencing and Class:MassSpectrometry 
* Class:DataObject will NOT have subclasses. Rather, it will have slot:data_category which has a range DataCategoryEnum
* slot:instrument_used will have Range:Class:Instrument
  * Class:Instrument will have separate enumerated slots for vendor and model
  * slot:id will be the identifier of the instrument_used and vendor and model will NOT be inlined for Class:DataGeneration. Rather, it will be inferred / traced using the instrument's id. ## Confirm this ##
* Class:ChemicalConversionProcess will replace Class:ReactionProcess
* All processes involving material to material transformations (Biosamples to ProcessedSample, ProcessedSample to ProcessedSample) will be subclasses of Class:MaterialProcessing (examples: Class:Extraction, Pooling, MixingProcess, etc.).
* Class:BiosampleProcessing will be removed. Subclasses of Class:MaterialProcessing will replace what was captured.
* Slots associated with Class:Biosample that were incorrectly associated, were removed and added to more appropriate classes. (ex: slot:pcr_primers was removed from Class:Biosample and added to the new Class:LibraryPrep)
* slot:omics_type will be removed and replaced with slot:analyte_category
* slot:analyte_category will be used to inform the type of data that is being generated and can be instantiated on Class:DataGeneration and Class:WorkflowChain
* Class:Reaction will renamed to Class:BiochemicalReaction
* ##Add gold_analysis_project_identifiers and (new) jgi_analysis_project_identifiers to WorkflowChain #1123##
* ##schema support for unhappy paths for OmicsProcessing #1144##
* Subclasses will have their own typecodes. While this may mean that eventually, if classes are renamed, typecodes might not make sense.
* Reciprocal or redundant relationship slots (has_part + part_of, was_informed_by + used) will not be used. Only single direction should be provided

* Stopped at Deprecate chimera_check slot in the list




## Context and Problem Statement

The NMDC schema was originally built providing one place, Class:OmicsProcessing, for capturing a wide variety of metadata fields. With expansion of data types and required metadata fields it was identified as being overloaded and requiring additional modeling.

This ADR will provide the decisions that were made leading up to and during the refactoring process to better capture metadata and describe samples and analyses.

<!-- This is an optional element. Feel free to remove. -->
## Decision Drivers

* {decision driver 1, e.g., a force, facing concern, …}
* {decision driver 2, e.g., a force, facing concern, …}
* … <!-- numbers of drivers can vary -->

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
