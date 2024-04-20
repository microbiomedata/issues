---
status: accepted
date: 2024-04-19
deciders: Shreyas Cholia, Alicia Clum
consulted: Shane Canon, Chris Beecroft
informed: Kjiersten Fagnan, Emiley Eloe Fadrosh
---
# Implement a Data Management solution for NMDC data

## Context and Problem Statement
NMDC needs a solution to handle handle data management lifecycle as data volumes grow. Current setup is just a shared directory in CFS at NERSC, 
with scripts to back things up to HPSS. 
  - Data should be automatically backed up to storage system (tape).
  - NMDC Services that operate on the data should be able to do so without the fear of accidental data deletion / corruption.
  - Separation of concerns for different services

## Considered Options

1. Current setup (shared dir with HPSS backup scripts)
2. Come up with our own system to manage data life cycle
3. Use JGI JAMO to handle data lifecycle
4. Use another 3rd party solution (iRODs, Cloud etc.)

## Decision Outcome

Use JAMO to handle data managament lifecycle needs (option 3). 

We need something in short order to avoid having a commonly writable area for NMDC data at NERSC (that serves both web and workflows), and JAMO meets our base requirements. Since the EMSL data management setup is sufficiently different this may not apply there, but we can investigate integration in the future. 

### Pros
- JAMO is already a production service utilized by JGI that we can take advantage of
- HPSS has a number of quirks and JAMO handles these automatically
- Can be used with NMDC with minor modifications so we don't need to start from scratch
- External solutions don't play well with NERSC, and we will need to scope out additional budget for cloud based solutions

### Cons
- JAMO can be somewhat heavyweight for our use since JGI has a lot of feature requirements not needed for NMDC
- Not a widely deployed tool, not open source, so it will be harder to run this on other systems like EMSL, and it may be trickier to contribute changes.
