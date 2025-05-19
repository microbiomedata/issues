---
# Update these YAML values so they describe this decision. Delete the leading `→` characters.
status: → proposed 
date: → 2025-05-19
deciders: → Eric, Patrick, Shreyas, Mike
consulted: → Alicia
informed: → Dev Team
---
# Move from mod_zip to zip_streamer for bulk downloads

## Context and Problem Statement
[mod_zip](https://github.com/evanmiller/mod_zip) is currently being used to generate bulk downloads but is having issues:

1. mod_zip no longer works with newer versions of NGINX because its build tools break with the transition from mercurial to git. 
  { link to related pages or docs }
2. mod_zip is sensitive to file_size mismatches between DB and filesystem { link to related issues }

For these reasons we want to update to an alternate solution. 

## Considered Options

* Patch older data portal docker container with code updates
   - this is being implemented as a temporary solution while we test zip streamer
   - not ideal because we are freezing an internal older docker image build and can't upgrade nginx and not 
* Migrate to an alternate bulk download tool i.e [zipstreamer](https://github.com/scosman/zipstreamer)

## Decision Outcome

