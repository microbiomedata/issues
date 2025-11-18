---
status: accepted
date: 2025-07-01
deciders: Chris Mungall (@cmungall), Montana Smith (@mslarae13), Mark Miller (@turbomam), Katherine Heal (@kheal)
consulted: Samantha Obermiller (@samobermiller), Bea Meluch (@bmeluch), Patrick Kalita (@pkalita-lbl), Lee Ann McCue (@leeannmccue), Sujay Patil (@sujaypatil96), Alicia Clum (@aclum)
informed:
---

# UCUM implementation in NMDC

## Context and Problem Statement

NMDC captures measurements in two ways: the **submission-schema**, where values appear as a single `{value} {unit}` string, and the **nmdc-schema, for data storage**, where a `QuantityValue` class stores the numeric part in the `has_value` slot and the unit in the `has_unit` slot. Although many slots document a `preferred_unit` (originating from MIxS), most currently accept any unit (e.g., `cm`, `meter`, or even `banana`) in `has_unit`, which jeopardizes data interoperability.

Many other MIxS-derived slots lack a `preferred_unit` annotation or contain ambiguous or non-UCUM units, such as:

- `abs_air_humidity` lists "gram per gram or cubic centimeter per cubic centimeter"  
- `efficiency_percent` uses "mg/kg (ppm)"  
- `inside_lux` mislabels a power-density unit as illuminance
- `rel_humidity_out` has ambiguous humidity units  
- `api` relies on a petroleum-industry convention

These inconsistencies hinder reliable validation and downstream analysis. By adopting UCUM, we will standardize every measurement to a canonical UCUM symbol and enforce the mapping through the `storage_units` annotation.

To address this, NMDC will adopt the **Unified Code for Units of Measure (UCUM)**, a canonical, machine-readable set of unit symbols that can be automatically validated. This ADR describes:

- how existing NMDC metadata was migrated and restructured into a UCUM-valid format
- how annotations on slot definitions are used to require UCUM units
- the ETL updates required for NMDC's data ingest (work in progress),  
- how ingested data can be checked and validated

## Considered Options

- Create various subclasses of `QuantityValue` for the ranges of slots we want constrained, for example `QuantityValueTemp` where `has_unit` is required and constrained to 'C'. Downside to this approach was that a separate `QuantityValue` subclass had to be made for each `has_unit` value we wanted to require.
- Create `QuantityValuewithUnit` as a subclass of `QuantityValue`, where `has_unit` is a required slot and constrained to enum `UnitEnum`. Downside to this approach was that any of the `UnitEnum` values could be used for any slot (i.e., 'm' could be used for `temp` slot).
- **Numeric-only slots with LinkML `unit` metaslot** - Convert all `QuantityValue` slots to scalar numeric types (int/float) and use the LinkML `unit` metaslot to store UCUM symbols, which can represent one or multiple allowed units per slot. This simplifies validation but requires a major schema redesign, breaks compatibility with existing data, and undermines the MIxS precedent that many slots could allow multiple units.
- **Conversion pipeline on ingest** - Accept any unit in the submission schema and perform extensive unit conversion during ETL ingestion. Flexible for submitters but adds significant engineering overhead and runtime conversion complexity.  
- **Hybrid approach (@cmungall's suggestion)** - Keep the existing `QuantityValue` structure, constrain allowed units to the UCUM symbol for the MIxS-preferred unit(s), and auto-convert full-name units to UCUM on ingest. Balances MIxS alignment with practical data handling.  
- **Exploratory `range_expression` association** - Instead of using `storage_units` annotations, associate slots and units via a `range_expression` (as discussed in [PR 2599](https://github.com/microbiomedata/nmdc-schema/pull/2599)). That's an experimental LinkML feature that could be applied to any schema development goal, not just the UCUM case.  
- **Chosen solution** - Introduce a `storage_units` annotation that lists allowed UCUM symbols per slot and enforce it via validation tests. This aligns with the hybrid approach while providing a clear, maintainable mechanism for unit standardization.

## Decision Outcomes

Upon implementation of UCUM, all NMDC slots with `range: QuantityValue` will have a `storage_units` annotation to designate allowed units and required formats for data.

## Validation

The UCUM implementation introduces several layers of validation:

- **Schema annotation tests** - We wrote automated tests (`tests/test_units_alignment.py`) that check each `QuantityValue` slot has a `storage_units` annotation. A small number of slots currently retain temporary `units_alignment_excuse` annotations (e.g., `api`, `efficiency_percent`, `inside_lux`) for cases requiring resolution of MIxS specification inconsistencies or analysis of complex/non-UCUM units. The goal is to eliminate all excuse annotations by providing proper UCUM `storage_units` for every slot.
- **UCUM code validation** - Automated tests (`tests/test_has_unit_enum.py`) verify that all permissible values in the `UnitEnum` are valid UCUM codes using the ucumvert parser library.
- **Instance data validation plugin** - A custom validation plugin (`NmdcSchemaValidationPlugin`) extends LinkML's pluggable validation framework to validate instance data against the `storage_units` constraints. This plugin checks that the `has_unit` value in each `QuantityValue` object matches the allowed units specified in the slot's `storage_units` annotation, and works in conjunction with LinkML's built-in `JsonSchemaValidationPlugin`.

These three mechanisms together guarantee that both the schema definition and the actual data conform to the UCUM standard.

## More Information

### Migrating existing NMDC data

**Implementation of annotations and slot requirements**

The UCUM integration introduces a new annotation `storage_units` on slots of type `QuantityValue`. This annotation lists the allowed UCUM unit symbols (pipe-separated) and is used by the NMDC validation suite.

This section describes the process for migrating existing data. 

```yaml
# Example slot definition in a LinkML schema
nitrate:
  range: QuantityValue
  description: "Measured concentration of nitrate"
  annotations:
    storage_units: "mmol/L|g/L"
```

**Annotation semantics**

- **`storage_units`** - a string containing one or more UCUM symbols, separated by pipes if necessary.
- The `storage_units` annotation is **mandatory** for every `QuantityValue` slot. `units_alignment_excuse` was allowed during development and should not be added to any subsequent `QuantityValue` slots.
- The `UnitEnum` used to validate `storage_units` entries is a project-specific, organic enumeration of UCUM symbols; it may not be suitable for other schemas.

**Enforcement**

Automated tests (described in the [Validation](#validation) section above) run on every PR via CI; failures block merging until compliance is achieved.

**Workflow for new slots**

- Define the slot in the appropriate YAML file under `src/schema/`.
- Add a pipe-delimited list of UCUM symbols to `storage_units`. Each element must be a permissible value of `UnitEnum`; add to `UnitEnum` if necessary.
- Run `make test` locally to ensure the validation passes.

**Owner responsibilities**

- Audit existing `QuantityValue` slots and populate `storage_units` where possible.
- Review PRs that modify schema files to ensure compliance.

**Historical migration process**

The migration to UCUM-compliant units proceeds through a pipeline that is fully scripted and version-controlled. All artifacts referenced below exist in the repository or in linked GitHub issues/PRs. *Note: Some of the scripts mentioned in this pipeline, particularly those in the `units/scripts` directory, are non-essential for the core migration process.*

**1. Inventory & proposal generation**

- **Extract slot inventory** - `make units-schema-extract` runs `units/scripts/schema_extract_preferred_units.py` and produces `output/schema_preferred_units.tsv`, a table of every `QuantityValue` slot together with any existing `preferred_unit` annotation.
- **Generate UCUM proposals** - `make units-schema-convert` runs `units/scripts/schema_convert_to_ucum.py` on the TSV above, producing `output/schema_ucum_input.tsv`. This file contains a proposed `storage_units` value for each slot **or** flags the slot as a problem that will need a `units_alignment_excuse`.
- **Reference** - [Issue #552](https://github.com/microbiomedata/nmdc-schema/issues/552) lists the MIxS slots with missing or inconsistent `preferred_unit` values; [PR #2599](https://github.com/microbiomedata/nmdc-schema/pull/2599) shows the diff of the generated proposals.

**2. Human review & gap list curation**

- Check `output/schema_ucum_input.tsv` and update the definitive gap list in `units/docs/units-problems-definitive.md`. This document records:
  - Slots that can receive a `storage_units` annotation.
  - Slots that cannot be expressed in UCUM and therefore receive a `units_alignment_excuse` (e.g., `api`, `soil_text_measure`).
- The curated TSV is committed back to the repo, becoming the single source of truth for the migration.

**3. Apply annotations to the schema**

- **Generate yq commands** - `make units-schema-generate` runs `units/scripts/schema_generate_yq_commands.py` which reads the curated TSV and writes two command files:
  - `output/yq_commands_single_unit.txt` - one-liner `yq` commands for slots with a single allowed unit.
  - `output/yq_commands_multi_unit.txt` - commands for slots with a pipe-separated list of allowed units.
- **Apply changes** - Pipe the command files into the shell (or open a PR) to modify the YAML schema files under `src/schema/`. The resulting PR is the one merged as [PR #2599](https://github.com/microbiomedata/nmdc-schema/pull/2599).

**4.  Production data migration & validation**

_To be deprecated by `NmdcSchemaValidationPlugin`-based methods_



- **Export MongoDB** - `make local/mongo_via_api_as_unvalidated_nmdc_database.yaml` uses `pure-export` to dump the live NMDC database to a single YAML file.
- **Validate production data** - Scripts in `units/scripts/` (such as `ucum_validate_units.py`) check production data against the schema's `storage_units` constraints.
- **Validation mechanisms** - As described in the [Validation](#validation) section above, multiple automated tests ensure schema compliance.
