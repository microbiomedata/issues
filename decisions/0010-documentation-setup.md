---
status: {proposed}
date: 2024-03-01
deciders: Shreyas, Eric, Mark M., Yan, Sierra, Donny 
consulted: 
informed: 
---
# Simplify setup for documentation building

## Context and Problem Statement

Currently the [NMDC documentation site](https://nmdc-documentation.readthedocs.io/en/latest/index.html) attempts to build the underlying docs by pulling from other repos (schema and workflows). This process relies on manually pushing updates and introduces duplication of documentation with possible inconsistencies.

We would like to simplify the process to avoid extra steps and duplicate docs.  

## Considered Options

1. Existing approach - run a build script that scrapes the docs from nmdc-schema and nmdc-workflows and adds it to NMDC_Documentation
2. Link out to the documentation pages from other repos, so that users are directly pointed at the current docs managed by those repos. Note that we may need to consider what we want to do with the previous version of the pages (pre link-out) in the NMDC_Documentation repo. 


## Decision Outcome

* Chosen option: 2. Link Out to repo specific docs. This allows us to lower the maintenance and update, and we can always point to current documentation.

[ Include Diagram ] 

Note that a lot of the core documentation will still stay in the main NMDC_Documentation repo. 

## Future Considerations

* Move from readthedocs to github pages to avoid account management issues.
* DONE - Move ADRs out of the NMDC_Documentation repo into a repos more suitable for project management (issues).
