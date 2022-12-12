---
status: proposed
date: 2022-12-11
deciders: Shreyas Cholia
consulted: {list everyone whose opinions are sought (typically subject-matter experts); and with whom there is a two-way communication}
informed: {list everyone who is kept up-to-date on progress; and with whom there is a one-way communication}
---
# Standardize convention for host names in NMDC

## Context and Problem Statement

Hostnames in the `*.microbiomedata` should have a standard naming pattern in order to 
- make it easier for humans to remember endpoints
- enable automation by using a standard pluggable convention 
- integrate with underlying kubernetes / rancher platform
- simplify management of certificates

## Decision Drivers

* Restrictions imposed by Spin - Spin has internal names and we would like parity with these if possible
* Provisioning of certificates and ensuring that we retain flexibility in doing so
* Names supported by cloudflare moving forwards

## Considered Options
We would like to capture the service name (data, api, db etc.) and the environment (production, sandbox, dev) in the hostname followed by `microbiomedata.org` For the canonical production host we can drop the environment to make it simpler for public user access

* `{service}-{env}` with `-` as the separator 
    - eg. `data-sandbox.microbiomedata.org`
* `{service}.{env}` with `.` as the separator 
    - eg. `data.sandbox.microbiomedata.org`
    - This introduces a potential extra subdomain in DNS depending on the hosting provider

We will use the following service names for public facing services (This list can be expanded))
* `api` - API service
* `data` - Data Portal  

We will use the following environment names (This list can be expanded)
* `prod` - production (optional since this is the user facing instance)
* `dev` - development 
* `sandbox` - user facing sandbox 


## Decision Outcome

Chosen option: "`{service}-{env}` with `-` as the separator", because it allows us to keep names right above the    `.microbiomedata.org` domain simplifying underlying infrastructure management rules. 

Additional details: 
* Approve the above list of service and environment names. Services and environments outside this list do not have a prescribed naming convention as yet, but these may be added in the future.
* `prod` environment name can be dropped if needed for ease of use (eg. `data.microbiomedata.org` instead of `data-prod.microbiomedata.org`)

### Pros
- Potentially simplifies wildcard certificate management.
- Hostname management in the service platform is simpler without the extra sub-domain. Makes future cloudflare deployments more straightforward.
- Aligns more closely with Spin naming conventions.

### Cons
- Some existing services deployed under additional subdomains (`nmdc.dev.microbiomedata.org` and `api.dev.microbiomedata.org`) may need new names. 
