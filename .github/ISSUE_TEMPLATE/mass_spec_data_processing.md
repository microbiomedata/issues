---
name: Metabolomics/Lipidomics/NOM Data Processing Request
about: Template for requesting data processing for Mass Spectrometry based Metabolomics, Lipidomics, or Natural Organic Matter (NOM) datasets.
title: 'Data Processing Request for [Study ID], [Data Type]'
labels: ["metab_data_processing"]
assignees: ''

---

## Study Information
- **Study ID:** _______________
- **User Project ID:** _____________
    *this should be populated at the study level in the [`emsl_project_identifiers`](https://microbiomedata.github.io/nmdc-schema/emsl_project_identifiers/) and/or [`jgi_portal_study_identifiers`](https://microbiomedata.github.io/nmdc-schema/jgi_portal_study_identifiers/) slots*
- **Source of Original Biosamples:** _______________
- **Location of Raw Data:**  _______________
- **Data Type:** ⬜ LCMS Metabolomics  ⬜ LCMS Lipidomics  ⬜ GCMS Metabolomics  ⬜ NOM

---
## Step 1: Gather user facility information
**Objective:** Gather essential sample and processing metadata from NMDC submissions, publications, etc. If the following cannot be obtained through existing data on NMDC or associated publications, reach out to PI or user facility managers to obtain the following information
- **Requestor(s):** _______________
- **Date:** _______________

### JGI
- [ ] **Request sample request excel sheet**
- [ ] **Request protocol for sample extraction**
- [ ] **Confirm standardized protocols (LCMS Metabolomics only) or request for data acquisition**

### EMSL
- [ ] **Request sample request excel sheet**
- [ ] **Request protocol for sample extraction**
- [ ] **Confirm standardized protocols (GCMS Metabolomics and LCMS Lipidomics only) or request for data acquisition***

---

## Step 2: Data Location & Access
**Objective:** Confirm raw data files are accessible

- [ ] **MASSIVE Repository**
  - Dataset ID: _______________
  
- [ ] **EMSL File System**
  - DMS search terms: _______________

- [ ] **Other Location**
  - Specify: _______________

---

## Step 3: Data Type Verification
**Objective:** Confirm data are workflow-compliant format

### LCMS Lipidomics or Metabolomics
- [ ] **MS Level confirmed as MS2**
- [ ] **Acquisition mode confirmed as DDA (Data-Dependent Acquisition)**
- [ ] **File format compatible** (.mzML, .mzXML, or .raw)

### GCMS Metabolomics
- [ ] **MS acquisition confirmed as low resolution**
- [ ] **FAMES calibration used and files located**
- [ ] **File format compatible** (.cdf)

### NOM
- [ ] **MS acquisition confirmed as high resolution and from FT-ICR**
- [ ] ** SRFA calibration used and files located** (not necessary but recommended)
- [ ] **File format compatible** (.d or .raw)

---

## Step 4: Biosample Mapping
**Objective:** Inspect biosample metadata in NMDC and match to raw data files
  
- [ ] **File to biosample mapping convention understood or mapping file acquired***
  - Source: ⬜ Publication  ⬜ Direct communication  ⬜ Deduced
  - Details: _______________
  - Number of raw data / biosample: _______________

- [ ] **Mapping established between files and NMDC biosamples**
  - Number of sample-like raw files (excluding standards, QC etc): _____
  - Mapping completeness: _____% sample-like files matched to biosamples

- [ ] **Orphaned sample-like raw files identified** (if any)
  - Example: _______________
  - Count: _______________
  - Action plan: _______________

---

## Step 5: Protocol Documentation
**Objective:** Obtain or deduce sample processing and data acquisition protocols

### Sample Processing Protocol
- [ ] **Sample preparation documented**
  - Source: ⬜ Publication  ⬜ Direct communication  ⬜ Deduced
  - Details: _______________

### Data Acquisition Protocol
- [ ] **Chromatography method documented**
  - Source: ⬜ Publication  ⬜ Direct communication  ⬜ Deduced
  - Details: _______________

- [ ] **Mass Spectrometry method documented**
  - Source: ⬜ Publication  ⬜ Direct communication  ⬜ Deduced
  - Details: _______________

---

## Final Decision

**Overall Assessment:** ⬜ GO  ⬜ NO GO  ⬜ GO WITH CONDITIONS

**Conditions/Followup Action Items (if applicable):**
1. 
2. 
3. 