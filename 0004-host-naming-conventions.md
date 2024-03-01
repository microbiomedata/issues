---
status: accepted
date: 2022-12-11
deciders: Shreyas Cholia
consulted: Donny Winston, 
informed: Emiley

---

# Standardize convention for host names in NMDC

## Context and Problem Statement

Hostnames in the `*.microbiomedata` DNS namespace should have a standard naming pattern in order to 
- make it easier for humans to remember endpoints
- enable automation by using a standard pluggable convention 
- integrate with underlying kubernetes / rancher platform
- simplify management of certificates

## Decision Drivers

* Restrictions imposed by Spin - Spin has internal names and we would like parity with these if possible
  * Spin internal names follow the convention of `{service}.{namespace}.{rancher-env}.svc.spin.nersc.org` where we primarily control the host and namespace. We have already been using `-` separator in the service and namespace. eg. data-dev.nmdc-dev.development.svc.spin.nersc.org
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

We will use the following environment names - Not all of these have been deployed (This list can be expanded)
| Env Name | Description | Status | 
| -------- | ----------- | ------ |
| `prod` | Production environment for primary instance of service in the pipeline (optional since this is the user facing instance) | Deployed in Spin Prod | 
| `dev` | Development environment for internally building new features, data, schemas etc. without impacting production | Deployed in Spin Dev | 
| `sandbox` | User facing sandbox that allows external users (especially in workshops) to play with system without real live data. | Deployed in Spin Prod | 
| `test` | Test Environment for internal NMDC developer testing | Future | 
| `stage` | Allows for previews of new features before launch. Should mirror production environment after launch | Future |   

## Decision Outcome

Chosen option: "`{service}-{env}` with `-` as the separator", because it allows us to keep names right above the    `.microbiomedata.org` domain simplifying underlying infrastructure management rules. 

Additional details: 
* Approve the above list of service and environment names. Services and environments outside this list do not have a prescribed naming convention as yet, but these may be added in the future.
* `prod` environment name can be dropped if needed for ease of use (eg. `data.microbiomedata.org` instead of `data-prod.microbiomedata.org`)

*Comment:* Using the `-` convention mostly avoids adding sub-domains which other tools may have opinions about. For instance we've had trouble with managing extra sub-domains via Cloudflare and some cert authorities are more restrictive about these. But I recognize that this is a little squishy in that everything in the current set up still works with sub-domains. As such this may be more of a defensive choice. 

### Pros
- Potentially simplifies wildcard certificate management.
- Hostname management in the service platform is simpler without the extra sub-domain. Makes future cloudflare deployments more straightforward.
- Aligns more closely with Spin naming conventions.

### Cons
- Some existing services deployed under additional subdomains (`nmdc.dev.microbiomedata.org` and `api.dev.microbiomedata.org`) may need new names. 
- Finegrained Wildcard management of subdomain certs (though it isn't clear we want this)


