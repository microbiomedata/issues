---
status: proposed
date: 2022-11-16
deciders: cmungall, dwinston, scanon, turbomam, sujaypatil96, mslarae13, pkalita-lbl
consulted: aclum, elais, emileyfadrosh, kfagnan, lamccue, shreddd, simroux
informed: corilo, hubin-keio
---
# A regular expression for NMDC identifiers

## Context and Problem Statement

Team members need string identifiers (IDs) to associate with data and metadata.
The problem is to set a scheme for identifiers that can be routinely and efficiently:

- minted,
- exchanged, 
- bound to metadata,
- referenced by workflows within a variety of compute and storage environments,
- cited, and
- resolved to informative metadata by web services.

<!-- This is an optional element. Feel free to remove. -->
## Decision Drivers

* workflows need running, en masse, ASAP
* no one wants to mint bespoke identifiers that they will later:
    * need to modify, or
    *  map to official (resolvable) ones.
* we want NMDC-stewarded IDs for studies and biosamples
    * we currently use IDs that we do not ourselves resolve, e.g. GOLD identifiers
    * we do not have control over GOLD etc. identifier resolution. 

## Considered Options

1. purely opaque IDs -- no typecode, no variant/fragment expression

    * e.g. `nmdc:<opaque_part>`

2. opaque IDs with addition of stable typecode

    * e.g. `nmdc:<typecode><delimeter><opaque_part>`

3. opaque IDs with addition of stable typecode and with variant/fragment expression

    * e.g. `nmdc:<typecode><delimeter><opaque_part><delimeter><vf_expression>

## Decision Outcome

Decided on option (3) because:

* re: typecode -- want human-recognizable identifier-type mark to classify by eye.
* re: variant/fragment expression -- want structurally recognizable relationship among identifiers.

Another decision element not elaborated on above:

* having a part of an identifier reflect the identifier's origin "minter".
* that is, a part of an identifier that namespaces the opaque ID.
* this element is referrenced to as the `shoulder`.

### Consequences

The NMDC schema specifies legal identifiers for all of its classes. All data instances/records that are intended for upload into the NMDC metadata store must have an `id` field that follows this specification, which is discussed below. 

NMDC offers a central identifier minting endpoint in order to save data contributors the trouble of hand-crafting `id`s, but it will require an update before it generates `id`s following the pattern specified below.

The possibility of decentralized (or offline) minting of `id`s  by trusted organizations has also been anticipated. `id` component 3 below (the shoulder) is used to indicate the organization that minted an `id`. LBL, which hosts the `id` minting endpoint will use one shoulder value. If another organization, like JGI or EMSL, needs to bulk-create `id`s outside of the  central identifier minting endpoint, they would use different shoulders, to be determined by the NMDC schema and metadata team.

No matter where they are minted, all NMDC `id` values must match this abstract pattern:

```
nmdc:<type-code>-<shoulder>-<blade><.version><_locus>
```


The abstract pattern has six parts, delimited by hyphens (unless otherwise specified):

1. `nmdc`: All NMDC identifiers will begin with this static prefix.

2. `<typecode>`: An alphabetical code with a 1:1 correspondence to a class from the NMDC schema. Answers the question "of what class is the data record that bears this `id`"? Must consist of 1 to 6 lower case letters, although a minimum of 3 letters is suggested. The *type code* portion of an NMDC `id` must match the regular expression `[a-z]{1,6}`.

3. `<shoulder>`: A code that indicates what organization minted the identifier. Shoulder values must be zero to six lower case letters, flanked by one digit on either side. Answers the question "what organization minted this `id`"? The central identifier endpoint, hosted at LBL, uses the shoulder 00. Should organizations like JGI or EMSL need to mint identifiers in bulk, they would be assigned other shoulders, so that `id` values aren't reused. The *shoulder* portion of an NMDC `id` must match the regular expression `[0-9][a-z]{0,6}[0-9]`.

4. `<blade>`: The fully unique part of the identifier under a given type code and shoulder namespace. The _shoulder_ and _blade_ together make up the _key_ of the identifier. The blade is an alphanumeric string of open-ended length with at least one character, following the regular expression: `[A-Za-z0-9]+`.

5. `<.version>`: Differentiates multiple iterations of a workflow. The delimiter used to separate the *version* from the *blade* and everything before it is a dot (`.`). The *version* is a potentially repeating alphanumeric pattern with a minimum length of 1 character. The *version* portion of an NMDC `id` must match the regular expression `(\.[A-Za-z0-9]+)*`.

6. `<_locus>`: Indicates the contig on which a genomic feature is found, along with its start and end coordinates. Delimited from the rest of the `id` by an underscore (`_`). The *locus* part, if present, must have at least one character from the set off uppercase letters, lower case letters, digits, underscores (`_`), dots (`.`) and hyphens (`-`). The regular expression that the locus will follow is: `_[A-Za-z0-9_\.-]+`.

The per-part regular expression described above can be composed into one complete regular expression. Named capture groups have been used to tie in the part names.

```
^(?<prefix>nmdc):(?<typecode>[a-z]{1,6})-(?<shoulder>[0-9][a-z]{0,6}[0-9])-(?<blade>[A-Za-z0-9]+)(?<version>(\.[A-Za-z0-9]+)*)(?<locus>_[A-Za-z0-9_\.-]+)?$
```

<!-- This is an optional element. Feel free to remove. -->
## More Information

### User-facing documentation

<https://microbiomedata.github.io/nmdc-schema/identifiers/#ids-minted-for-use-within-nmdc>

### Implementation in nmdc-schema LinkML

* General settings: <https://github.com/microbiomedata/nmdc-schema/blob/v7.3.0/src/schema/nmdc.yaml#L95>
* Typecode example: <https://github.com/microbiomedata/nmdc-schema/blob/v7.3.0/src/schema/nmdc.yaml#L220>