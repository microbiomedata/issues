---
status: accepted
date: 2023-08-01
deciders: "@eecavanna, @Shalsh23, @shreddd, @turbomam, @dwinston"
consulted: (same as `deciders`)
informed: All existing `nmdc-schema` and `nmdc-runtime` contributors
---
# Mongo schema changes will always be accompanied by data migration scripts

## Context and Problem Statement

The Mongo database used by the NMDC contains data that adheres to the specific version of `nmdc-schema` being imported by `nmdc-runtime`. Over time, new versions of `nmdc-schema` are imported into `nmdc-runtime`.

Currently, when a new version of `nmdc-schema` is imported into `nmdc-runtime`, the maintainers of `nmdc-runtime` do not always know whether any changes to existing data will be required in order to make it so that the existing data adheres to the new version of `nmdc-schema`; and, if so, what those changes are.

## Considered Options

1. An `nmdc-schema` maintainer and an `nmdc-runtime` maintainer discuss each new version of `nmdc-schema` before it gets imported into `nmdc-runtime`
2. An `nmdc-schema` maintainer commits a data migration script (which may be a no-op) in the `nmdc-schema` repository along with each unit of schema changes

## Decision Outcome

Chosen option: "An `nmdc-schema` maintainer commits a data migration script (which may be a no-op) in the `nmdc-schema` repository along with each unit of schema changes"

This was chosen because it will make it so the data changes (or lack thereof) are explicitly documented, in a standard format and location, and can (eventually) be performed by a CI system.
