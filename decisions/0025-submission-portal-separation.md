---
status: proposed
date: 2026-11-09
deciders: TBD
consulted: TBD
informed: TBD
---
# Approach for separating the Submission Portal from nmdc-server

## Context and Problem Statement

The `nmdc-server` repository holds two products, the Data Portal and the Submission Portal, which are built, deployed, and operated together as one backend and one frontend application. As NMDC ramps down and is folded into DOE-BRIDGE, the Submission Portal will continue and become common submission tooling for DOE-BRIDGE partners. The future of the Data Portal is less certain. Early discussions also anticipate moderate to major user experience changes, such as separate user journeys for User Facility users and for users bringing their own data.

The decision to separate the two products has been made. This ADR addresses *how* to do it.

## Decision Drivers

* Effort required to reach a standalone Submission Portal.
* Room for the Submission Portal to evolve, including the anticipated user experience changes and, much later, support for additional BER data types.
* Technical debt carried forward, notably the multi-user editing model, the DataHarmonizer integration, and how sample metadata is stored. Sample metadata storage is closely tied to the spreadsheet editor and the editing model, and changes to the forms or the submission schema have repeatedly required hand-written data migrations.
* Whether and how existing data carries over, including in-progress and historical submissions, user accounts, roles, and uploaded images. One view is that existing data must carry over; another is that a new system starts with new data. If data carries over, the transition may require merging data from the old system into the new one several times rather than copying it once.
* Consumers of submission data. Today, finalized submissions are read from the portal's API and handed off to the NMDC MongoDB store. The handoff will presumably move to the DOE-BRIDGE Lakehouse, on a timeline still to be determined. The portal's requests to GitHub and EMSL's LIMS are outbound. Those services are not believed to depend on the portal, so they must be carried over or reimplemented but impose no compatibility constraint.
* No disruption to the Data Portal, including its periodic data ingest, while it operates until retirement.
* Fit with the team's expertise. The team that will build and maintain the Submission Portal did not build `nmdc-server`, and some of its technology choices, particularly on the frontend, would likely not be made today.

## Considered Options

1. Keep the `nmdc-server` repository and incrementally remove the Data Portal
2. New repository, carrying over the Submission Portal frontend and backend with the same general architecture
3. New repository, greenfield reimplementation
4. New repository, carrying over the backend and rebuilding the frontend

## Decision Outcome

Chosen option: TBD

### Consequences

TBD

## Validation

TBD

## Pros and Cons of the Options

### 1. Keep the `nmdc-server` repository and incrementally remove the Data Portal

The Data Portal's code, data, and infrastructure are removed from the `main` branch in increments as the Data Portal is retired.

* Good, because it requires the least up-front effort.
* Good, because no data needs to be moved or merged.
* Good, because the existing API, outbound integrations, and test suite are unchanged.
* Good, because the full git history is preserved.
* Bad, because the separation cannot finish until the Data Portal is retired. Until then, submission data remains tied to the Data Portal's ingest process, which rebuilds and swaps the shared database.
* Bad, because all existing technical debt and technology choices are carried forward, including those that do not fit the team's expertise.
* Bad, because NMDC-specific naming, infrastructure, and database migration history carry into the DOE-BRIDGE era.
* Bad, because future changes happen through incremental refactoring of inherited code.

### 2. New repository, carrying over the Submission Portal frontend and backend with the same general architecture

The Submission Portal's backend and frontend code are copied from `nmdc-server` into a new repository, without the Data Portal code, and run on a new, dedicated database.

* Good, because the Submission Portal is independent of the Data Portal and its ingest process from cutover, without waiting for the Data Portal to be retired.
* Good, because the Data Portal is left untouched.
* Good, because existing data, if carried over, is relatively easy to bring across, even through repeated merges. The database schema is unchanged, and submission data has no database-level dependencies on Data Portal data.
* Good, because the existing API, outbound integrations, and test suite come along largely intact.
* Good, because git history for the carried-over files can be preserved (e.g. with `git filter-repo`).
* Neutral, because the effort is moderate. Several core backend and frontend files contain code for both products and must be separated by hand.
* Bad, because all existing technical debt and technology choices are carried forward, including those that do not fit the team's expertise.
* Bad, because cutover requires coordinating URLs, ORCID login configuration, API consumers, and deployment.
* Bad, because future changes happen through incremental refactoring of inherited code.

### 3. New repository, greenfield reimplementation

A new Submission Portal is built from scratch, reimplementing what works in the current portal and rethinking what does not. The current portal keeps running from `nmdc-server` until cutover.

* Good, because it leaves the most room for the anticipated user experience changes, support for additional data types, and the Lakehouse handoff.
* Good, because the multi-user editing model, the DataHarmonizer integration, and sample metadata storage can be redesigned together.
* Good, because the team can choose technologies that fit its expertise.
* Good, because the Data Portal is left untouched.
* Neutral, because the cost of data continuity depends on whether existing data carries over. If it does, each merge from the old system likely requires transforming data into a new model, possibly several times during the transition. If the new system starts with new data, this cost disappears.
* Neutral, because most of the work behind the existing code carries forward even if the code does not. The planned user experience changes would rewrite much of the frontend under any option. The submission schema, the status workflow and permission rules, the integration agreements, and what the team has learned from users all carry forward. The existing tests, issue history, and documentation record expected behavior and edge cases, and can serve as acceptance criteria for the new implementation.
* Bad, because it requires the most effort and the longest time before a standalone Submission Portal exists.
* Bad, because the outbound integrations (GitHub, EMSL LIMS) must be reimplemented, though this work is mostly self-contained.
* Bad, because consumers of submission data (the handoff to MongoDB or, later, the Lakehouse) may need to change.
* Bad, because the existing test suite cannot be reused directly, which increases the risk of regressions.
* Bad, because the user experience requirements are still in early discussion, so the design may get ahead of decisions.
* Bad, because until cutover, submission data stays in the current portal and remains tied to the Data Portal's ingest process.

### 4. New repository, carrying over the backend and rebuilding the frontend

The Submission Portal backend (data model, API, authentication, permissions, image storage, integrations) is carried into a new repository as in option 2, and the frontend is rebuilt from scratch as in option 3. New work is concentrated in the frontend, which is where most of the anticipated user experience changes fall and much of the technical debt sits. The backend, which is reasonably self-contained, is reused.

* Good, because carrying over existing data, if desired, is as easy as in option 2, since the database schema is unchanged.
* Good, because consumers of submission data and outbound integrations are unaffected, and the backend test suite comes along.
* Good, because the user experience changes and the replacement or redesign of DataHarmonizer start without inherited frontend code.
* Good, because the frontend can use technologies that fit the team's expertise, and the backend's technology choices are ones the team would likely make today.
* Good, because the Submission Portal is independent of the Data Portal from cutover, and the Data Portal is left untouched.
* Good, because git history for the carried-over backend files can be preserved.
* Neutral, because the effort falls between options 2 and 3.
* Neutral, because most of the work behind the existing frontend carries forward even if its code does not. The planned user experience changes would rewrite much of the frontend under any option, and the workflow, permission rules, and lessons learned from users carry forward. The existing frontend's behavior and issue history can serve as a reference for the new one.
* Bad, because backend design debt is carried forward, including the lock-based editing model and sample metadata storage.
* Bad, because the advantages of this option shrink the more the multi-user editing model is rethought. Sample metadata storage is closely tied to that model and to the current spreadsheet editor, so it is the part of the backend most likely to need rework. A substantial editing redesign means substantial backend changes, which moves this option toward option 3.
* Bad, because the new frontend must either conform to the existing API, which limits user experience changes, or drive API changes, which reduce the benefit of reusing the backend.

## More Information

Relevant details of the `nmdc-server` codebase:

* Backend
  * Submission Portal database tables refer only to user accounts and to each other, not to Data Portal tables.
  * Submission Portal API endpoints do not read Data Portal data, but they share core modules with the Data Portal endpoints.
  * Authentication (ORCID login and user accounts) is shared by both products.
  * Form content (study information, multi-omics, shipping, and so on) is stored as structured documents that the backend validates. It could be carried over or restructured without much difficulty.
  * Sample metadata is stored as documents shaped by the spreadsheet editor and the submission schema, and each save replaces a sample set's metadata as a whole. Document storage is not a problem in itself and may suit future support for more data types. The debt comes from whole-set saves, the tie to the spreadsheet editor, and schema versioning. When the submission schema changes, existing sample metadata is migrated where a feasible migration path exists, so stored data is understood to follow the schema version the portal currently depends on, as far as possible. Each schema change requires migration work, and data that could not be migrated is not marked as such.
  * Integrations with GitHub (issue creation) and EMSL (LIMS sample export, project lookup) are outbound requests from the backend.
* Database lifecycle
  * The Data Portal's ingest process builds a second database from MongoDB, copies submission and user data into it, and swaps it into service. Submission Portal data therefore lives on a database whose lifecycle is controlled by Data Portal ingest.
* Frontend
  * The Submission Portal frontend is a self-contained area of the shared single-page application, with its own state management. It depends on a few shared pieces: the API client, the current-user state, the app header and login, and some presentational components.
  * The Data Portal frontend does not depend on Submission Portal code, apart from the shared router and its navigation hooks.
