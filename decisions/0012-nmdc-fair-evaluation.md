# ADR 
---
status: accepted
date: 2024-04-08 
deciders: Shreyas Cholia, Donny Winston, Jing Cao
consulted: Chris Mungall
---
# Gap analysis with fair enough metrics 

We will test a representative sampling of NMDC data objects using the existing fair enough metrics API. This [summary sheet](https://docs.google.com/spreadsheets/d/1i7V6_RVweDDny_hmhDDoSJjM9FLO_embsrLaHXC179E/edit#gid=0) describes how metrics are defined out of the box. Results will be used for a gap analysis and should fall into one of four categories: 1) Passes metric and is okay, 2) passes metric and not okay, 3) fails metric and is okay - metric needs updating 4) fails metric and is not okay. 

## Problem Statement

How can we evaluate NMDC's implementation of FAIR principles? While there is a [proliferation of tools](https://github.com/orgs/linkml/discussions/1811) for self-evaluations of FAIR principles, we seek a solution that programmatically evaluates the FAIRness of our project. An ideal solution would: 1) reflect the current state of NMDC; 2) identify gaps and areas of improvement; 3) offer value to NMDC team members as well as visibility to upstream stakeholders. 

## Considered Options

* CI pipeline in Dagster that evaluates FAIRness of new/updated data objects
* Dashboarding with Strudel
* NMDC-hosted API or workflow with custom `fair-test` metrics
* Exploratory notebook for gap analysis with the `fair-enough-metrics` [API](https://metrics.api.fair-enough.semanticscience.org/docs). 

## Decision Outcome

Chosen option: Gap analysis with fair enough metrics, because it:
- provides a snapshot of the current state of NMDC 
- minimizes duplication of work. For example, a CI pipeline may duplicate efforts from data validation
- allows for flexibility in interpreting results
- balances goals with level of effort
