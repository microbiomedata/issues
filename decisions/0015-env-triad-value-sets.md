---
status: accepted
date: 2026-01-26
deciders: Mark Miller, Montana Smith, Sierra Moxon, Chris Mungall
consulted: Lee Ann McCue, Natalie Winans
informed: NMDC Data Team
supersedes: 0015-env-triad-structure.md, 0015-env-triad-terms.md
---

# ADR-0015: NMDC Environmental Triad Value Sets

## Context and Problem Statement

MIxS defines three environmental context slots (`env_broad_scale`, `env_local_scale`, `env_medium`) that NMDC requires for all Biosamples. Without curated value sets, submitters must navigate ENVO's ~6,900 classes to find appropriate terms, creating cognitive burden and inconsistent term selection across studies.

NMDC needed a sustainable approach to create, maintain, and distribute curated value sets for different sample types (soil, water, sediment, plant-associated).

---

## Decision Drivers

1. **Submitter experience**: Curated dropdowns reduce cognitive burden vs. open ontology navigation
2. **Data consistency**: Constrained choices enable grouping similar samples across studies
3. **Multi-ontology requirements**: Value sets include ENVO, Plant Ontology (PO), and UBERON terms
4. **Maintenance sustainability**: Workflow must be reproducible without excessive coordination overhead
5. **Timely updates**: NMDC release cycles should not depend on upstream ontology releases

---

## Considered Options

### Value Set Creation
- **Option A: Pure OAK Queries** — Rejected: produced value sets too large; no mechanism for expert judgment
- **Option B: LLM-Based Discovery** — Rejected: complex, expensive, non-reproducible
- **Option C: Query-Seeded Expert Voting** — Selected

### Value Set Publication
- **Option A: ENVO Subset Integration** — Rejected after year-long attempt (see [historical context](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/env-triad-envo-integration-history.md))
- **Option B: Self-Published LinkML Enumerations** — Selected

---

## Decision Outcome

### Decision 1: Value Set Creation

**NMDC will create value sets via a query-seeded expert voting workflow.** OAK queries against biosample databases generate candidate terms, which domain experts review and vote on for inclusion. Terms meeting vote threshold (typically vote_sum ≥ 1) are included.

### Decision 2: Value Set Publication

**NMDC will self-publish value sets as LinkML enumerations in [submission-schema](https://github.com/microbiomedata/submission-schema).**

This approach:
- Supports multi-ontology value sets (ENVO + PO + UBERON) in single definitions
- Decouples from upstream ontology release cycles
- Enables direct validation in Data Harmonizer
- Follows the "Application Profile" pattern common in semantic web work

### Decision 3: Discontinue ENVO Subset Integration

**NMDC will discontinue maintaining ENVO subset annotations** and request deprecation of `ENVO:03605010` and its subproperties.

---

## Consequences

### Positive
- Reduced cognitive burden for submitters via curated dropdowns
- Improved data consistency across studies
- Multi-ontology coherence in single enumerations
- Decoupled release cycles
- Reproducible curation with audit trail (vote sums)

### Negative
- Maintenance overhead for periodic voting workflow updates
- Multi-repository coordination (external-metadata-awareness → submission-schema)
- Value sets not discoverable via OLS/BioPortal

---

## More Information

- **Workflow documentation**: [Environmental Triad Value Set Lifecycle](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/environmental-triad-value-set-lifecycle.md)
- **Historical context and action items**: [ENVO Integration History](https://github.com/microbiomedata/external-metadata-awareness/blob/main/docs/env-triad-envo-integration-history.md)
- **Key issues**: [#846](https://github.com/microbiomedata/issues/issues/846), [#844](https://github.com/microbiomedata/issues/issues/844), [#1351](https://github.com/microbiomedata/issues/issues/1351)

---

## References

- [MIxS Standard](https://genomicsstandardsconsortium.github.io/mixs/)
- [ENVO-MIxS Guidelines](https://github.com/EnvironmentOntology/envo/wiki/Using-ENVO-with-MIxS)
- [LinkML Enumerations](https://linkml.io/linkml-model/latest/docs/EnumDefinition/)
