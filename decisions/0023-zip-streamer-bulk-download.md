---
status: accepted
date: 2025-05-19
deciders: Eric, Patrick, Shreyas, Mike
consulted: Alicia
informed: Dev Team
---
# Move from `mod_zip` to ZipStreamer for bulk downloads

## Context and Problem Statement

[`mod_zip`](https://github.com/evanmiller/mod_zip) is currently being used to generate bulk downloads but is having issues:

1. `mod_zip` no longer works with newer versions of nginx because its build tools break with the transition from Mercurial to Git (see https://github.com/evanmiller/mod_zip/issues/114).
2. `mod_zip` is sensitive to file size mismatches between the manifest (which is derived from the database) and the actual filesystem (see https://github.com/microbiomedata/nmdc-server/issues/1587#issue-2966854262).

For these reasons, we want to update to an alternate solution. 

## Considered Options

* Patch older data portal Docker image with code updates
   - this is actually already being implemented as a temporary solution while we test ZipStreamer
   - this is not ideal, because we are freezing an internal older Docker image build and can't upgrade nginx now
* Migrate to an alternate bulk download tool; i.e [ZipStreamer](https://github.com/scosman/zipstreamer)

## Decision Outcome

Chosen option: "Migrate to an alternate bulk download tool; i.e [ZipStreamer](https://github.com/scosman/zipstreamer)", because of the aforementioned issues with `mod_zip`.

### Consequences

* Good, because we are no longer tied to `mod_zip` and the limitations it puts on our nginx version.
* Good, because ZipStreamer uses JSON to define the file descriptors in the zip
* Good, because it's easy to deploy in its own Docker container
* Bad, because it can still take many hours for users to download large datasets locally
* Bad, because it is not an exceptionally popular or actively-maintained library
* Bad, because it does not log what it is doing, which makes debugging difficult
* Bad, because it [silently omits upstream files that are unavailable](https://github.com/scosman/zipstreamer/issues/29)

## Validation

There are number of [tests in place in the `nmdc-server` repository](https://github.com/microbiomedata/nmdc-server/blob/main/tests/test_download.py) that validate that the backend can properly build a bulk download. However, these tests are not entirely ZipStreamer specific. During pre-release testing, we typically test medium-sized bulk downloads manually on the dev site.

## More Information

We have not reevaluated `mod_zip` or any other solutions since we switched to ZipStreamer. It could be that `mod_zip` has improved since this decision was made. The ZipStreamer solution has been working well for small to large download sizes (KiB to ~100GiB), but is less reliable for very large download sizes (+100GiB).
