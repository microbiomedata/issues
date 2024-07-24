---
status: accepted
date: 2024-07-23
deciders: Alicia, Bin, Patrick C., Paul, Yuri
consulted: JAWS team, Set, Cam, Sam, Shane, Kaitlyn, Valerie, Mark F., Michael, Cam, Grant, Katherine
informed: leadership team
---
# Conslidate on a single version of the WDL specification

## Context and Problem Statement

We needed a pick a version of WDL for the project to consolidate on. This is important for several reasons including 
importing subworkflows, interoperablity with user facility workflows, and to have more speicfic technical requirements 
for developers to work against.  


## Considered Options

* Consolidate on version 1.0
* Consolidate on Draft 2
* Leave workflows as a mix of version 1.0 and draft 2


## Decision Outcome

Chosen option: "Consolidate on version 1.0", because WDL version 1.0 was recommended by the JAWS team.  One of the the most important features
that is not supported in WDL Draft 2 but is by WDL 1.0 is the ablity to import a WDL  with an http prefix. This improves reusablity 
between workflows without having to use git submoudles. It is critical, in EDGE, for example to have imported workflows all
use the same version.

