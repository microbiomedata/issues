---
status: accepted
date: 2022-11-09
deciders: Patrick Kalita, Jeff Baumes, Shreyas Cholia, Emiley Eloe-Fadrosh, Julia Kelliher
informed: Ingrid Ockert
---
# Improve Connection Between Home Page and Products

## Context and Problem Statement

The home page and landing site at [microbiomedata.org](https://microbiomedata.org) is a WordPress site that was set up by an outside contractor at the outset of the project.
Since that time our actual products ([data](https://data.microbiomedata.org/) and [submission](https://data.microbiomedata.org/submission/home) portals, [EDGE](https://nmdc-edge.org/home)) have matured.
How do we transition the landing page from just providing information about the project to driving visitors to use our products?

## Considered Options

* Retain WordPress landing site and make incremental improvements to it
* Transition to a headless CMS and implement home/landing site content in existing [custom application](https://github.com/microbiomedata/nmdc-server)

## Decision Outcome

Chosen option: "Retain WordPress landing site", because it allows small (but important) changes to be made more quickly and has less overall risk. Specifically, the incremental improvements are:

* Add more prominent links to data portal, submission portal, EDGE to home page
* Assess adding "news feed"-type content to home page. This may include general news, upcoming events, newly added data, social media feeds
* Design a common header and footer that can be implemented in the WordPress landing site and the data/submission portal site in order to reduce the friction in moving between those sites
* Assess adding data summary statistics provided by the [data portal API](https://data.microbiomedata.org/docs) to the home page

## Pros and Cons of the Options

### Retain WordPress landing site

* Good, because allows us to roll out changes quickly and incrementally
* Good, because we have people on the project with experience updating WordPress content
* Bad, because it will require updates to WordPress themes and templates; a developer will need to learn how to make such changes

### Transition to a headless CMS

* Good, because certain things like headers and footers don't need to be implemented in more than one system
* Good, because enables highly custom content to be added to home page
* Bad, because required further architectural decisions around headless CMS
* Bad, because potentially requires project staff to maintain CMS deployment (existing WordPress site is hosted on common LBL infrastructure)
* Bad, because requires significant developer work upfront

## More Information

* [Discussion document](https://docs.google.com/document/d/1eYjY4gJKzPmzfH3LBmOpFIiB2EhBu1vvqz-4bFLJ4JU/edit#)
