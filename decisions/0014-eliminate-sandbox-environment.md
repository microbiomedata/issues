---
status: accepted
date: 2024-08-14
deciders: Montana Smith, NMDC PIs, Mike Nagler, Patrick Kalita 
consulted: All NMDC team members—via [GitHub discussion](https://github.com/microbiomedata/issues/discussions/743), meetings, and Slack —were given the opportunity to express their opinions
---
# Eliminating the NMDC Submission Portal sandbox environment

This ADR outlines the process we used to decide to eliminate the NMDC Sandbox Submission Portal, the decision that was made, and the planned updated to ensure functionality is maintained. 

## Context and Problem Statement

Currently, the NMDC Submission Portal sandbox is wiped every Sunday; i.e. all submissions are deleted from the underlying Postgres database.Some ambassadors or other users are still using the production environment for tests, workshops, and fake submissions. Additionally, people don't always know about the NMDC Sandbox site or there's a desire to have fake/test studies persist after a Sunday (ex: Prep for a workshop on Friday, and the submission still be there for Monday's session; Re-use the same submission for multiple workshops & not need to re-submit)

We proposed a few solutions in the [GitHub Discussion](https://github.com/microbiomedata/issues/discussions/743) and in a small meeting (contact Montana for those Google notes). Team members were invited to contribute to and voice opinions via the GitHub Discussion. Additionally, we polled the team on Slack for reactions to confirm people were aware, and we received 12 approvals and 0 objections.

<!-- This is an optional element. Feel free to remove. -->
## Decision Drivers

* Even though we have been maintaining a sandbox environment, many people have continued to create test submissions in the production environment.
* Sandbox is wiped weekly and people may need test submissions to persist longer than that
* Metrics are not gathered for sandbox

## Considered Options

**Solution 1**
* Provide a "delete this submission" button.
    - Sandbox NEVER gets wiped. People can just delete their submissions when they're ready

**Solution 2**
* Extend the time.
    - Submissions are deleted every quarter?
    - Submissions are deleted 90 days after creation?
    - Should provide a warning? How?
* Add a "trash" where people can restore the submission for so many days after deletion?

**Solution 3**
* Add a "delete / this is a test" check box to the production site & those get deleted every so many days
    - - This checkbox could be marked even after the submission has been created, so the submission would not be deleted until someone has said they want it to be.
    - I think a LOT of people make test submission and forget about them, so the NMDC team should have the ability to delete them & send a notice

## Decision Outcome

Chosen option: Combo solution
* Add “delete this submission” option to https://data.microbiomedata.org/submission/home
    - Anyone with “creator role” (submitter & PIs) and admins can delete a submission
* Eliminate sandbox environment
* Add “Create test submission” button to all environments; e.g. [production](https://data.microbiomedata.org/submission/home), development, etc.
    - The button's label is TBD (e.g. "Test", "Fake", "Workshop", ...)
    - - Provide clear tooltip text to elaborate / explain when to make a real submission vs test submission
        - Test submissions can NOT be submitted
        - Test submissions can NOT be converted to real submissions
        - Test is not equal to Draft
* Eliminate deletion policy until we get a good sense of HOW test submissions are being created & IF people delete their own tests
    - Submissions will NEVER be auto deleted
* Add a “date last modified” attribute to submissions to track how often people refer to a test submission & determine when tests could be deleted
* Add "Create test submission” option to the NMDC Field Notes mobile app
* Add ability to track what submissions in the submission portal come from the mobile app (for metrics)

<!-- This is an optional element. Feel free to remove. -->
### Consequences    

**Pros**:
- dataportal sandbox is confusing, eliminates this
- Users forget about / don't access sandbox
    - One place makes it easier
- Data Portal sandbox environment will no longer exist
- Less code to manage
- Less infrastructure to maintain
- Avoid paying for unnecessary compute resources

**Cons**:
- Need to update all documentation that references the sandbox environment.

## Summary

This ADR outlines the decision and some goals of this change. The work is still in progress & this ADR will be updated when documentation is updated & the sandbox environment has been eliminated. For additional details on discussions, you can contact Montana or refer to the above linked GitHub issue. 
