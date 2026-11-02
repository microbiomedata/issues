---
status: accepted
date: 2025-05-19
deciders: Eric, Patrick, Shreyas, Mike
consulted: Alicia
informed: Dev Team
---
# Move from mod_zip to zip_streamer for bulk downloads

## Context and Problem Statement
[mod_zip](https://github.com/evanmiller/mod_zip) is currently being used to generate bulk downloads but is having issues:

1. mod_zip no longer works with newer versions of NGINX because its build tools break with the transition from mercurial to git (see https://github.com/evanmiller/mod_zip/issues/114).
2. mod_zip is sensitive to file_size mismatches between DB and filesystem (unable to find reference link, Oct 2026).

For these reasons we want to update to an alternate solution. 

## Considered Options

* Patch older data portal docker container with code updates
   - this is being implemented as a temporary solution while we test zip streamer
   - not ideal because we are freezing an internal older docker image build and can't upgrade nginx and not 
* Migrate to an alternate bulk download tool i.e [zipstreamer](https://github.com/scosman/zipstreamer)

## Decision Outcome

Chosen option: "Migrate to an alternate bulk download tool i.e [zipstreamer](https://github.com/scosman/zipstreamer)}", because of the aforementioned issues with mod_zip.

### Consequences

* Good, because we are no longer tied to mod_zip and the limitations it puts on our NGINX version.
* Good, because zipstreamer uses JSON to define the file descriptors in the zip
* Good, because it's easy to deploy in its own docker container
* Bad, because it can still take many hours for users to download large datasets locally
* Bad, because it is not an exceptionally popular or well-maintained library

## Validation

There are number of [tests in place in nmdc-server](https://github.com/microbiomedata/nmdc-server/blob/main/tests/test_download.py) that validate that the backend can properly build a bulk download. However, these tests are not entirely zipstreamer specific. During pre-release testing, we typically test medium-sized bulk downloads manually on the dev site.

## More Information

We have not reevaluated mod_zip or any other systems since switching our services to Google Cloud. It could be that mod_zip has improved since this decision was made. The zipstreamer solution has been working well for small to large download sizes (KiB to ~100GiB), but is less reliable for very large download sizes (+100GiB).

