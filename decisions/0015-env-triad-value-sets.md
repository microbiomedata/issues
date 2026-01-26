# ADR-0015: NMDC Environmental Triad Value Sets

**Status**: Accepted

**Date**: 2026-01-26

**Supersedes**: Previous draft ADR-0015 versions (0015-env-triad-structure.md, 0015-env-triad-terms.md)

**Deciders**: Mark Miller, Montana Smith, Sierra Moxon, Chris Mungall

**Consulted**: Lee Ann McCue, Natalie Winans

**Informed**: NMDC Data Team

---

## Context and Problem Statement

MIxS defines three environmental context slots (`env_broad_scale`, `env_local_scale`, `env_medium`) that NMDC requires for all Biosamples. These are informally called the "environmental triad" by NMDC staff. Without curated value sets, submitters must identify appropriate terms from ENVO's ~215 biomes, ~5,040 environmental materials, and ~1,770 astronomical body parts—covering most of ENVO's ~6,900 classes. This creates cognitive burden and inconsistent term selection across studies.

NMDC needed a sustainable approach to:
1. Create curated value sets for different sample types (soil, water, sediment, plant-associated)
2. Maintain these value sets as ontologies evolve
3. Distribute value sets for validation in the NMDC Submission Portal (Data Harmonizer)

### Evolution of This ADR

This ADR consolidates and supersedes two previous draft files that were never finalized:
- `0015-env-triad-structure.md` (merged to main via PR #881, Aug 2024)
- `0015-env-triad-terms.md` (remained in unmerged branches until recently)

Neither was complete, and the operational workflow evolved differently than either described. This ADR documents the **current operational approach** and formalizes the **decision to discontinue ENVO subset integration**.

---

## Decision Drivers

1. **Submitter experience**: Curated dropdown lists reduce cognitive burden vs. open ontology navigation
2. **Data consistency**: Constrained choices enable grouping similar samples across studies
3. **Multi-ontology requirements**: Value sets include ENVO, Plant Ontology (PO), and UBERON terms
4. **Maintenance sustainability**: Workflow must be reproducible without excessive coordination overhead
5. **Timely updates**: NMDC release cycles should not depend on upstream ontology releases

---

## Decision 1: Value Set Creation Approach

### Considered Options

#### Option A: Pure OAK Queries (Rejected)

**Approach**: Use automated ontology queries based on MIxS rules (e.g., "all subclasses of biome minus aquatic biome for soil").

**Why rejected**: Produced value sets too large for practical use. No mechanism to incorporate domain expert judgment about term appropriateness.

#### Option B: LLM-Based Discovery (Rejected)

**Approach**: Use large language models to identify appropriate environmental terms.

**Why rejected**: Complex, expensive, non-reproducible results.

#### Option C: Query-Seeded Expert Voting (Selected)

**Approach**: Use OAK queries to seed candidate term lists from empirical data (NCBI, GOLD, NMDC biosamples), then have domain experts vote on inclusion.

**Why selected**:
- Combines systematic term discovery with domain expertise
- Vote sums provide reproducible inclusion criteria (IAA scores available for additional analysis if needed)
- Handles edge cases that automated rules cannot address
- Allows incorporation of evidence from multiple biosample databases

### Decision Outcome: Value Set Creation

**NMDC will create value sets via a query-seeded expert voting workflow.** OAK queries against biosample databases generate candidate terms, which domain experts review and vote on for inclusion. Terms meeting vote threshold (typically vote_sum ≥ 1) are included in value sets.

### Note on "Cherry-Picking"

The original ADR (Aug 2024) established a "no cherry-picking" principle ([#844](https://github.com/microbiomedata/issues/issues/844)): specific terms should not be individually removed; elimination must always be done via general query changes.

This principle was relaxed because no purely query-based approach produced usable value sets. The voting workflow effectively cherry-picks terms, but does so through systematic, reproducible curation with documented rationale (vote sums, expert consensus).

---

## Decision 2: Value Set Publication and Distribution

### Background: ENVO Subset Integration Attempt

From August 2024 through August 2025, NMDC attempted to publish value sets by injecting `oboInOwl:inSubset` annotations into ENVO via ROBOT templates. This would have enabled tools like OLS/BioPortal to query NMDC subsets directly from ENVO releases.

**Implementation details**:
- Created ROBOT template (`nmdc_env_context_subset_membership.tsv`) mapping ENVO terms to NMDC subset IDs
- Defined subset annotation properties in ENVO:
  - `ENVO:03605010` - NMDC environmental context subsets (root)
  - `ENVO:03605013` - Terrestrial biomes
  - `ENVO:03605014` - Environmental features (terrestrial)
  - `ENVO:03605015` - Soil types
  - `ENVO:03605017` - Aquatic biomes
  - `ENVO:03605018` - Environmental features (aquatic)
  - `ENVO:03605019` - Water types
- Integrated into ENVO Makefile build process

**Why ENVO integration was abandoned**:

1. **Technical fragility**: The Aug 2025 ENVO release (v2025-08-19) accidentally dropped all NMDC subset annotations. The multi-step ODK build process has many failure points with silent failures (build succeeds even when annotations are missing).

2. **Architectural limitation (fundamental)**: NMDC value sets contain terms from multiple ontologies:
   - ENVO terms (environmental contexts)
   - PO terms (plant anatomical structures for plant-associated samples)
   - UBERON terms (animal anatomical structures for host-associated samples)

   ENVO can only annotate ENVO terms. Publishing complete NMDC value sets via ENVO subsets is architecturally impossible without coordinating with PO and UBERON maintainers to import NMDC subset definitions into each ontology.

3. **Release coupling**: NMDC value set updates required waiting for ENVO releases, introducing delays and coordination overhead.

### Decision Outcome: Value Set Publication

**NMDC will self-publish value sets as LinkML enumerations in [submission-schema](https://github.com/microbiomedata/submission-schema) and discontinue ENVO subset integration.**

**Why self-publishing was selected**:
- Supports multi-ontology value sets (ENVO + PO + UBERON) in single coherent definitions
- Decoupled from upstream ontology release cycles
- LinkML enumerations directly enforce validation in Data Harmonizer
- Follows the "Application Profile" pattern common in semantic web work

### Distribution Improvements Needed

The current submission-schema GitHub Pages documentation provides basic access to value sets but needs improvements for practical reuse:

1. **Downloadable formats**: Add mechanisms for visitors to download individual enumerations or grouped value sets as TSV, YAML, or other formats. Currently, users must navigate to raw files in the repository.

2. **Navigation between grouping and leaf pages**: Improve ability to navigate between:
   - Grouping pages (e.g., all soil enumerations, all env_broad_scale enumerations)
   - Individual enumeration pages (e.g., EnvBroadScaleSoilEnum)

   The current [NmdcEnvTriadEnums](https://microbiomedata.github.io/submission-schema/NmdcEnvTriadEnums/) index page is mostly empty and doesn't effectively organize the 12 enumerations.

3. **Persistent URLs**: Configure w3id.org namespace for submission-schema to provide stable, citable URLs for value sets independent of GitHub infrastructure.

---

## Combined Decision Summary

**NMDC maintains extension-specific environmental triad value sets as LinkML enumerations in submission-schema, created via expert voting workflow.**

### Key Design Choices

1. **LinkML enumerations** in submission-schema are the authoritative source for value sets
2. **Extension-specific value sets**: Separate enumerations per extension × slot (12 total: 4 extensions × 3 slots)
3. **Multi-ontology support**: Enumerations include ENVO, PO, and UBERON terms as appropriate
4. **Non-restrictive validation**: Data Harmonizer accepts enumerated values OR regex-matching strings (`label [CURIE]` format), allowing valid ontology terms not yet added to enumerations
5. **Expert voting workflow**: Query-seeded candidates reviewed by domain experts

### Enumeration Naming Pattern

```
Env[Scale][Extension]Enum
```

Examples: `EnvBroadScaleSoilEnum`, `EnvLocalScaleWaterEnum`, `EnvMediumPlantAssociatedEnum`

### Currently Implemented Extensions

| Extension | env_broad_scale | env_local_scale | env_medium |
|-----------|-----------------|-----------------|------------|
| Soil | EnvBroadScaleSoilEnum | EnvLocalScaleSoilEnum | EnvMediumSoilEnum |
| Water | EnvBroadScaleWaterEnum | EnvLocalScaleWaterEnum | EnvMediumWaterEnum |
| Sediment | EnvBroadScaleSedimentEnum | EnvLocalScaleSedimentEnum | EnvMediumSedimentEnum |
| Plant-associated | EnvBroadScalePlantAssociatedEnum | EnvLocalScalePlantAssociatedEnum | EnvMediumPlantAssociatedEnum |

Additional MIxS extensions (Air, Built environment, Host-associated, Hydrocarbon resources, Microbial mat/biofilm, Miscellaneous) do not yet have curated value sets. See [#79](https://github.com/microbiomedata/issues/issues/79) for planned expansion.

**Important**: Value sets are maintained in **submission-schema**, not nmdc-schema. The submission-schema is the authoritative source for environmental triad enumerations.

---

## Consequences

### Positive

- **Reduced cognitive burden**: Submitters see curated dropdown choices instead of navigating ontology hierarchies
- **Improved data consistency**: Constrained choices enable better grouping and querying of similar samples
- **Multi-ontology coherence**: Single enumeration can combine ENVO + PO + UBERON terms appropriately
- **Decoupled releases**: Value set updates ship with submission-schema releases, not blocked by ontology release cycles
- **Reproducible curation**: Voting workflow with vote sums provides audit trail

### Negative

- **Maintenance overhead**: Voting workflow requires periodic updates as ontologies evolve
- **Multi-repository coordination**: Workflow spans external-metadata-awareness (voting sheet generation) and submission-schema (schema integration)
- **NMDC-specific artifacts**: Value sets are not discoverable via OLS/BioPortal ontology browsers
- **Documentation gaps**: GitHub Pages needs improvements for download and navigation (see Distribution Improvements Needed above)

### Neutral

- **Reuse by other projects**: Other projects can access value sets via submission-schema GitHub releases or GitHub Pages documentation, but not via standard ontology distribution channels

---

## Maintenance

- **Triggers for updates**: New extension support requested, community feedback on missing terms, significant ontology releases
- **Responsibility**: Metadata team with domain expert consultation
- **Cadence**: As needed; no fixed schedule

---

## Action Items

### Completed

- [x] Implement LinkML enumerations for 4 extensions × 3 slots (12 enums) in submission-schema
- [x] Establish voting workflow with Google Sheets
- [x] Document workflow in external-metadata-awareness (see [Environmental Triad Value Set Lifecycle](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/environmental-triad-value-set-lifecycle.md))

### Proposed

1. **Request ENVO deprecate NMDC subset annotation properties**

   The following annotation properties should be deprecated in ENVO since NMDC will no longer maintain them:
   - `ENVO:03605010` - NMDC environmental context subsets (root)
   - `ENVO:03605013` - Terrestrial biomes
   - `ENVO:03605014` - Environmental features (terrestrial)
   - `ENVO:03605015` - Soil types
   - `ENVO:03605017` - Aquatic biomes
   - `ENVO:03605018` - Environmental features (aquatic)
   - `ENVO:03605019` - Water types

   **Rationale**: These properties were created for NMDC's use but are no longer being maintained. Leaving them in ENVO without active maintenance creates confusion and potential for stale data.

   **Action**: Open issue in EnvironmentOntology/envo requesting deprecation.

2. **Archive ROBOT template generation code**

   The `create_env_context_robot_template.py` script in submission-schema should be archived or removed, with documentation noting it represents a deprecated approach.

3. **Improve value set accessibility** (Issues #1351, #1352, #1353, submission-schema#392)
   - Add TSV/YAML download buttons to enumeration documentation pages
   - Fix NmdcEnvTriadEnums index page to properly organize and link enumerations
   - Improve navigation between grouping pages and individual enumeration pages
   - Configure w3id.org namespace for submission-schema persistent URLs

---

## Implementation Details

For the complete step-by-step workflow for creating and updating value sets, see:

**[Environmental Triad Value Set Lifecycle](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/environmental-triad-value-set-lifecycle.md)** in the external-metadata-awareness repository.

That document covers:
- Voting sheet generation from biosample data
- Expert voting process
- Vote processing and schema integration
- Adding new environments

---

## Related Issues and Discussions

### microbiomedata/issues
- [#846](https://github.com/microbiomedata/issues/issues/846) - Document policies for each env triad term v2
- [#844](https://github.com/microbiomedata/issues/issues/844) - Evaluate OAK success for SOIL curated terms
- [#1138](https://github.com/microbiomedata/issues/issues/1138) - Add sediment PVs to ENVO subsets
- [#1351](https://github.com/microbiomedata/issues/issues/1351) - Create summarizing doc page
- [#1352](https://github.com/microbiomedata/issues/issues/1352) - Add TSV download buttons
- [#1353](https://github.com/microbiomedata/issues/issues/1353) - Configure w3id.org redirects
- [#1354](https://github.com/microbiomedata/issues/issues/1354) - Work with Pier to get PO/UBERON terms to ENVO (closed - no longer pursuing)
- [#502](https://github.com/microbiomedata/issues/issues/502) - Milestone: Harmonizing environmental science standards
- [#468](https://github.com/microbiomedata/issues/issues/468) - Milestone: Define EnvO value sets

### EnvironmentOntology/envo
- [#1642](https://github.com/EnvironmentOntology/envo/issues/1642) - NMDC value sets disappeared from 2025-08-19 release

### microbiomedata/submission-schema
- [#392](https://github.com/microbiomedata/submission-schema/issues/392) - w3id.org namespace configuration

---

## References

- [MIxS Standard](https://genomicsstandardsconsortium.github.io/mixs/)
- [ENVO-MIxS Guidelines](https://github.com/EnvironmentOntology/envo/wiki/Using-ENVO-with-MIxS)
- [LinkML Enumerations](https://linkml.io/linkml-model/latest/docs/EnumDefinition/)
- [submission-schema](https://github.com/microbiomedata/submission-schema) - Authoritative source for value sets
- [Environmental Triad Value Set Lifecycle](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/environmental-triad-value-set-lifecycle.md) - Implementation guide

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2024-08-28 | Initial draft (0015-env-triad-structure.md) | Montana Smith, Mark Miller |
| 2024-08-28 | Alternative draft (0015-env-triad-terms.md) | Montana Smith, Mark Miller |
| 2026-01-26 | Consolidated and updated to reflect operational reality, formalized ENVO discontinuation | Mark Miller |
