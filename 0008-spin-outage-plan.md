---
status: proposed
date: 2023-10-04
deciders: "@shreddd, @eecavanna"
consulted: 
informed: "@emileyfadrosh"
---
# NMDC Spin Outage Plan

## Context and Problem Statement

We need to have an approach to managing NERSC Spin outages for the NMDC websites. 

### Additional Context

1. As a preliminary step, we introduced Cloudflare into the DNS resolution chain for the NMDC websites (we changed the chain from `LBLnet → website` to `LBLnet → Cloudflare → website`). That way, we can use Cloudflare to control what the URLs point to, without waiting for LBLnet requests to be fulfilled. Note: @eecavanna and @shreddd have access to Cloudflare.
2. There is a backup instance of the NMDC data portal hosted by EMSL/PNNL.
Its URL is: https://data-microbiomedata.emsl.pnnl.gov/
3. We have setup an outage web page. It is hosted on GitHub Pages. Its URL is: https://status.microbiomedata.org. Its GitHub repo URL is: https://github.com/microbiomedata/nmdc-status-website

## Considered Options

1. Configure the websites' `CNAME` records in Cloudflare to point to `status.microbiomedata.org` (i.e. the outage web page).
2. Use Cloudflare _Single Redirects_ to redirect (`HTTP 307 Temporary Redirect`) the URLs to the outage web page.
3. Use Cloudflare _Page Rules_ to redirect (`HTTP 307 Temporary Redirect`) the URLs to the outage web page.
4. Use Cloudflare _Custom Pages_ to host a custom Error 500 page that would be shown to visitors when the website is down.
5. Updating the `CNAME` records on LBLnet—via their change request process—to point to `status.microbiomedata.org` (i.e. the outage web page).

## Decision Outcome

Chosen option: 2

> Use Cloudflare _Single Redirects_ to redirect (`HTTP 307 Temporary Redirect`) the URLs to the outage web page.

### Justification

- Option 2 provides us with an easy, common way to change where the websites' URLs lead. With a few mouse clicks on Cloudflare, the URL that normally points to a given NMDC website can be changed to point to the outage web page instead.
- The temporary redirect does not impact SEO / search engine rank of the websites.

### Rejected Options

- Option 1: The `CNAME` and proxy setup process can be tricky.
- Option 3: Cloudflare's documentation says _Page Rules_ "should only be used when [Single Redirects or Bulk Redirects] don't meet your needs."
- Option 4: The outage web page is slightly too large for Cloudflare's _Custom Pages_ feature. Also, this option depends upon the backend returning an HTTP 500 status code.
- Option 5: The LBLnet change request process can take a long time.
