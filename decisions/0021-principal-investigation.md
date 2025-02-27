status: accepted
date: 2025-01-15
deciders: 1pm metadata sync team, Emiley, Chris, Lee Ann
consulted: all on the 1pm metadata team
informed: all on the 1pm metadata team sync
---
# Update and replace policy and tracking of principal investigator

## Context and Problem Statement

Currently, principal investigator is captured in 2 places. Using the slot `principal_investigator` as well as the CREDIT role `has_credit_association` for Person values.

## Decision Outcome

- the slot `principal_investigator` will be deprecated
- CREDIT roles will be used to identify the PI
- Multiple PIs can be assigned to a single study

## Next Steps

This decision has not yet been implemented. This change will require an update to how the data portal displays PI and a migration.
