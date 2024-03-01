---
status: accepted 
date: 2023-12-19
deciders: Chris Mungall, Lee Ann McCue
consulted: Anastasiya Prymolenna, Brynn Zalmanek, James Tessmer, Montana Smith, Sam Purvine, Yuri Corilo, Mark Miller, Michael Thorton, Alicia Clum  
informed: Emiley Eloe-Fadrosh, Shreyas Cholia, Eric Cavanna 
---
# Schema Refactoring: Monterey/Berkeley

## Decisions Made

### Class:PlannedProcess
* `Class:PlannedProcess` will be created as a root class.
* All "Activity" classes are now of `Class:PlannedProcess`, and the word "Activity" will be removed.

### Class:MaterialProcessing
* `Class:MaterialProcessing` will be created and be a subclass of `Class:PlannedProcess`.
* All processes involving material-to-material transformations (Biosamples to ProcessedSample, ProcessedSample to ProcessedSample) will be subclasses of `Class:MaterialProcessing` (examples: `Class:Extraction`, `Pooling`, `MixingProcess`, etc.). 
* The output of any subclasses of `Class:MaterialProcessing` will have an output of `Class:ProcessedSample`.
* `Class:ChemicalConversionProcess` will replace `Class:ReactionProcess`.
* `Class:BiosampleProcessing` will be removed. Subclasses of `Class:MaterialProcessing` will replace what was captured. 
* Linking a protocol to a `Class:MaterialProcess` subclass can happen at the individual class level, or as aggregated processes via the `Class:ProtocolExecution`.
* `slot:catalyzed_by` is instantiated on `Class:ChemicalConversionProcess` with `Range:Class:Solution`.
  * `Class:Solution` has `slot:has_solution_components` with `Range:Class:SolutionComponent` where `slot:compound` (instantiated on `Class:SolutionComponent`) has `Range:Class:ProteolyticEnzymeEnum`.
  * `slot:catalyzed_by` will capture any substrate or enzyme that was used to facilitate a chemical conversion process. 
  * Enzymes that can be used as catalysts are available in the `Class:ProteolyticEnzymeEnum`.
  * The `slot:catalyzed_by` will be referenced by proteomics workflows.

### Class:DataGeneration
* `Class:DataGeneration` will be created and take the place of `Class:OmicsProcessing`.
* `Class:OmicsProcessing` will be removed.
  * All reference to `Class:OmicsProcessing` should be eliminated from NMDC schema & Workflows.
  * Typecode `omprc` will still be valid for `Class:DataGeneration`.
* `Class:DataGeneration` will detail the process of inputting a material sample (Biosample or ProcessedSample, some MaterialEntity) into an instrument and generating data (output).
  * Instrument data will be denoted using `slot:data_category` on `Class:DataObject`.
  * The output from `Class:DataGeneration` should always be `instrument_data`.
* `Class:DataGeneration` will have subclasses `Class:NucleotideSequencing` and `Class:MassSpectrometry` .
  * This allows for the expansion of additional data types as NMDC grows.
* `slot:instrument_used` will have `Range:Class:Instrument`.
  * `Class:Instrument` will have separate enumerated slots for `vendor` and `model`.
  * `slot:id` will be the identifier of the `instrument_used`; `vendor` and `model` will NOT be inlined for `Class:DataGeneration`. Rather, they will be inferred / traced using the instrument's id.
* Schema support was added for complex paths of data generation and data processing.
  * `Class:DataGeneration` can have instances where a single sample has multiple data files that need processed together during WorkflowExecution.  
  * Relationships between samples and the data objects will be captured using `slot:part_of`, linking the 'child' back to the 'parent'.

### Class:WorkflowExecution & Class:WorkflowChain
* `Class:WorkflowExecution` will be a subclass of `Class:PlannedProcess`.
* `Class:WorkflowExecution` will replace `Class:WorkflowExecutionActivity` and will be a subclass of `Class:PlannedProcess`.
* To group subclasses of `Class:WorkflowExecution` into implemented steps, `Class:WorkflowChain` was created.
  * This also provides clear connection back to the DataGeneration output, using `was_informed_by`.
    * `was_informed_by` is being removed from `Class:WorkflowExecution`.
  * Any implementation of `Class:WorkflowExecution` will use `Class:WorkflowChain`; even when it's a single linked chain (ie implementation of a single subclass of `Class:WorkflowExecution`).
* `Class:WorkflowExecution` will provide `part_of` with `Range:Class:WorkflowChain`.
  * This replaces 'used' and will no longer be on `Class:WorkflowExecution`.
* Output from `WorkflowExecution` will always have the value `processed_data` in `slot:data_category` of `Class:DataObject`.
* `slot:analyte_category` will be used to inform the type of data that is being generated. This slot can exist on `Class:DataGeneration` and `Class:WorkflowChain`.
  * `slot:omics_type` will be removed and replaced with `slot:analyte_category` to denote the type of data generated.
* `slot:gold_analysis_project_identifiers` and `slot:jgi_analysis_project_identifiers` will be included on `Class:WorkflowChain`.

### Other
* `Class:DataObject` will NOT have subclasses. Rather, it will have `slot:data_category` which has a range `DataCategoryEnum`.
* `slot:data_object_type` will be renamed to `slot:data_object_category`. 
* Slots associated with `Class:Biosample` or `Class:OmicsProcessing` that were incorrectly associated, were removed and added to more appropriate classes. (ex: `slot:pcr_primers` was removed from `Class:Biosample` and added to the new `Class:LibraryPrep`).
* `Class:Reaction` will be removed from the schema until it is used.
* Subclasses will have their own typecodes.
  * This may mean that eventually, if classes are renamed, typecodes might not make sense.
* Reciprocal or redundant relationship slots (`has_part` + `part_of`, `was_informed_by` + `used`) will not be instantiated. Only a single direction should be provided.
* All classes will have `slot:type` with `Range:CURIE` and `designates_type = True`
  * `slot:type` should have a range of the class they are.

### In Progress
* When/how will instances of Class:WorkflowChain be created?
  * By the workflows team, or by NMDC runtime?

## Context and Problem Statement

The NMDC schema was originally built providing one place, Class:OmicsProcessing, for capturing a wide variety of metadata fields. This was also nucleic acid analysis specific. With expansion of data types and required metadata fields it was identified as being overloaded and requiring additional modeling.

This ADR provides the decisions that were made leading up to and during the refactoring process to better capture metadata and describe samples and analyses.

## Decision Drivers

* Subject matter experts, schema leaders, and modelers met and discussed all decisions above. 
* Need for expanded and improved modeling

### Consequences

* Major data migration required
* Workflow updates required
* Data portal ingest code updates required
* Improves data representation
* Improves workflows and automation

## More Information

