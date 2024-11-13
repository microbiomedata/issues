---
# Update these YAML values so they describe this decision. Delete the leading `→` characters.
status: proposed
date: 2024-11-08
deciders: Shreyas Cholia, Sierra Moxon, Jeff Baumes, Jing Cao, Mark Miller, Katherine Heal, Brynn 
consulted: Sujay Patil, Alicia Clum  
informed: Chris Mungall 
---
# Use of nmdc_client

## Context

We have a number of different solutions to make complex queries against the runtime API, especially involving graph traversal. Jeff Baumes recently proposed an nmdc_client library that can capture some of the complexities and hide them from users.  

### Summary of Jeff Baumes GDoc
This document collects bits of work that have been done in constructing a python client for NMDC. Many have been disparately working on this problem.

https://api.microbiomedata.org/docs#/metadata/list_from_collection_nmdcschema__collection_name__get
- An endpoint that lets you get at the NMDC objects from any collection
- You specify the collection name
- You specify the query as a Mongo query object
- Joins are not supported

https://github.com/polyneme/nmdc-client-api/blob/main/index.ipynb
- Breaks up ids into groups because of URL limit
- Traverses certain types of links to move between types

https://github.com/microbiomedata/nmdc_notebooks
- The nmdc_api.py file has some patterns that have been useful for getting data and associating between NMDC types
- It also breaks up ids into groups because of the URL size limit
- Also enables traversal if you know the related collections and slot names
- Has some code for working with dataframes

https://api.microbiomedata.org/docs#/find/find_data_objects_for_study_data_objects_study__study_id__get
- An endpoint that performs a join to find objects of a certain type (DataObject) from another (Study)
- It uses the alldocs collection (an aggregation of our source of truth collections) to make the join
- During the retreat, Jing also implemented another prototype endpoint to go in the other direction, from DataObject to Biosample

Graph database
- Mark Miller spun up a graph database view of NMDC data to help query and inspect connections between NMDC objects

https://github.com/microbiomedata/refscan
- A library that draws out connections between NMDC classes and constructs a network
- Utilizes the actual schema and SchemaView to inspect NMDC slot relationships without requiring any hard-coded knowledge of NMDC (other than the Database class)

nmdc-server (Data portal)
- Has an endpoint used by the client which can link across collections, but this is not “client friendly” in general and was designed to be consumed by the data portal.

https://github.com/jeffbaumes/nmdc-client
https://jeffbaumes.github.io/nmdc-client/
- An attempt to pull some of these ideas together
- Uses the schema to do some introspection and lets the user work with the schema from “nmdc_schema.nmdc” which gives the users Python code completion on classes instead of using strings.
- Uses SchemaView to introspect the NMDC schema
- Introspects which collection to query for a given schema type so the user does not need to know what database collections to use. Also is able to detect schema classes based on typecodes in order to do intelligent linking.
- Draws from ideas from nmdc_api.py to look up large sets of ids and work with dataframes, especially how to merge in new ids from another type.
- Supports returning data as pandas dataframes, python dictionaries, or “nmdc_schema.nmdc” schema objects.
- Supports walking the entity DAG, enabling finding associated entities of one type from any other type.
- To accomplish this, the library currently requires a few minutes to construct the full graph of links the first time an association is requested. After that, it is cached and walking the DAG is fast.
- The DAG currently hard-codes these certain set of relations to follow: Study (→ Study)* → Biosample (→ MaterialProcessing → ProcessedSample)* → DataGeneration → DataObject → (WorkflowExecution → DataObject)*


## Considered Options

* Supporting a client side library with downloaded graph in memory - https://github.com/jeffbaumes/nmdc-client
* Addressing the graph traversal server side with a flexible associations endpoint - https://github.com/microbiomedata/nmdc-runtime/issues/717 


## Decision Outcome

**Decision:** Use server side endpoint

**Details:** 
- Polyneme will look at reviving associations endpoint that will serve as a flexible approach to traverse schema
    - https://github.com/microbiomedata/nmdc-runtime/issues/717 
- Jeff Baumes' nmdc_client downloads the entire graph in memory client side - this is a useful prototype but this is best approached as a server-side operation, since client side may have scaling issues and support will be difficult. 
    - We will look to borrow concepts and design from this on the server side code
- Server side implementation will be language agnostic
- Hold off on specific nmdc_client implementation, but notebooks team should feel free to borrow veneer code and convenience functions from nmdc_client example to setup helper functions etc.
- We will revisit an nmdc_client library in the future
    - this is still a good idea for simplifying complex operations and multi-step API calls. For now implement pieces as support code in notebooks repo.  

