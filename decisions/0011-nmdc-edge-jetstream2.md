---
status: proposed
date: 2024-04-27
deciders: Mark Flynn, Shreyas Cholia
consulted: Patrick Chain, Bin Hu
informed: Emiley Eloe-Fadrosh
---
# New platform for hosting NMDC-EDGE

## Context and Problem Statement
Our plan is to move NMDC-EDGE from where it is currently hosted at SDSC. We would like to have a solution that is cheaper than our current deployment.

## Considered Options

* Google Cloud Projects (GCP)
* Jetstream2 Virtual Clusters

## Decision Outcome

Chosen option: "Jetstream2 Virtual Clusters", for two reasons:  
1) GCP requires extensive modification of the WDL's and Docker images for workflow execution.
2) The backends offered by GCP, that Cromwell uses to connect to the compute resources, are in transition between
two different options. The older backend, Google Cloud Life Sciences API, is deprecated. The newer backend, Google Batch, is not yet supported by Cromwell. 
This means that any solution we implement now will have to change once the older backend is permanently discontinued and there 
is no guarantee that the newer backend will be ready for use by then.

## Pros and Cons of the Options

### Jetstream2 Virtual Clusters

* Good, because it doesn't cost anything
* Good, because it doesn't require any changes to the WDL or Docker images
* Good, because the development team is committed to helping us
* Bad, because the infrastructure is still under development

## More Information
We will be using the Jetstream2 platform to create a virtual cluster that will run NMDC-EDGE workflows. The virtual
cluster will consist of a head node and one or more worker nodes that will run the workflows. Each of these nodes will
run in its own VM. The NMDC-EDGE website will be running on a separate virtual machine along with Cromwell, MongoDB and MySQL.  
When users submit a new workflow, Cromwell will submit sbatch scripts to the slurm scheduler running on the virtual 
cluster. These jobs will then be executed on the worker nodes. Cromwell will monitor the jobs running on the worker
nodes and copy the output to the website VM when they finish running. We will use Magic Castle to create
a shard storage system for Cromwell and the virtual cluster
