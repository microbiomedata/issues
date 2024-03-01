# Decisions

This directory contains decision records for NMDC.

For new ADRs, please use [adr-template.md](adr-template.md) as basis.
More information on MADR is available at <https://adr.github.io/madr/>.
General information about architectural decision records is available at <https://adr.github.io/>.


The filenames are following the pattern NNNN-title-with-dashes.md (ADR-0005), where
- `NNNN` is a consecutive number and we assume that there won’t be more than 9,999 ADRs in one repository.
- the title is stored using dashes and lowercase, because [adr-tools] also does that.
- the suffix is .md, because it is a Markdown file.

Decisions are placed in the subfolder decisions/ to keep them close to the documentation but also separate the decisions from other documentation.

See [ADR-0000](0000-use-markdown-any-decision-records.md) for an example