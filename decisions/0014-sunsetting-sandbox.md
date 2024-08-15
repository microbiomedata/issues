---
status: accepted
date: 2024-08-14
deciders: Montana Smith, NMDC PIs, Mike Nagler, Patrick Kalita 
consulted: Entire NMDC team via [GitHub discussion](https://github.com/microbiomedata/issues/discussions/743), meetings, and slack were given the oppotrunity to voice opinions
---
# Sunsetting the NMDC Sandbox UI

This ADR outlines the process we used to decide to sunset the NMDC Sandbox Submission Portal, the decision that was made, and the planned updated to ensure functionality is maintained. 

## Context and Problem Statement

Currently, the NMDC submission portal sandbox is wiped every Sunday.
Some ambassadors or other users are still using the production environment for tests, workshops, and fake submissions. Additionally, people don't always know about the NMDC Sandbox site or there's a desire to have fake/test studies persist after a Sunday (ex: Prep for a workshop on Friday, and the submission still be there for Monday's session; Re-use the same submission for multiple workshops & not need to re-submit)

We proposed a few solutions in the [GitHub dsicussion](https://github.com/microbiomedata/issues/discussions/743) and in a small meeting (contact Montana for those google notes). Team members were invited to contribute to and voice opinions on the GitHub discussion. Additionaly, we polled slack for reactions to confirm people were aware and we received 12 approvals. 

<!-- This is an optional element. Feel free to remove. -->
## Decision Drivers

* People aren't using the sandbox site 
* Sandbox is wiped and people may need test submissions to persist
* Metrics are not gathered for sandbox

## Considered Options

**Solution 1**
* Provide a "delete this submission" button.
    - Sandbox NEVER gets wiped. People can just delete their submissions when they're ready

**Solution 2**
* Extend the time.
    - Submissions are deleted ever quarter?
    - Submissions are deleted 90 days after creation?
    - Should provide a warning? How?
* Add a "trash" where people can restore the submission for so many days after deletion?

**Solution 3**
* Add a "delete this is a test" check box to the production site & those get deleted every so many days
    - This checkbox can be checked after the fact so it's not deleted until someone says they want it to be.
    - I think a LOT of people make test submission and forget about them, so the NMDC team should have the ability to delete them & send a notice

## Decision Outcome

Chosen option: Combo solution
* Add “delete this submission” option to https://data.microbiomedata.org/submission/home
    - Anyone with “creator role” (submitter & PIs) and admins can delete a submission
* Eliminate sandbox
* Add “Create test submission” button to [prod+dev](https://data.microbiomedata.org/submission/home)
    - Test, fake, workshop, TBD what we title this button
    - Provide clear tool tip text to elaborate / explain when to make a new submission vs test submission
        - Test submissions can NOT be submitted
        - Test submissions can NOT be converted to real submissions
        - Test is not equal to Draft
* Eliminate deletion policy until we get a good sense of HOW test submissions are being created & IF people delete their own tests
    - Submissions will NEVER be auto deleted
* Add a “date last modified” attribute to submissions to track how often people refer to a test submission & determine when tests could be deleted
* Add "Create test submission” option to the app
* Add ability to track what submissions in the submission portal come from the app (for metrics)

<!-- This is an optional element. Feel free to remove. -->
### Consequences    

**Pros**:
- dataportal sandbox is confusing, eliminates this
- Users forget about / don't access sandbox
    - One place makes it easier
- Less code to manage

**Cons**:
- Need to update all documentation that references the -sandbox.

## Summary

This ADR outlines the decision and some goals of this change. The work is still in progress & this ADR will be updated when documentation is updated & sandbox is officially sunset. For additional details on discussions you can contact Montana or refer to the above linked GitHub issue. 
