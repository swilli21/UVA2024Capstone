# Introduction
This project aims to test whether using MedLM for the automated summarization of structured and semi-structured medical data will lead to more efficient and accurate extraction of patient information. By creating comprehensive and concise summaries that capture essential details about a patient's medical history, current state, and treatment protocols, contributing to enhanced patient care and management.

# Data
The Medical Information Mart for Intensive Care (MIMIC)-III database provided critical care data for over 2.3 million Radiology Reports and 40,000 patient care records of deidentified data.

### MIMIC-III
The dataset contains 26 tables of deidentified health-related data associated with over forty thousand patients who stayed in critical care units of the Beth Israel Deaconess Medical Center between 2001 and 2012. The database includes information such as demographics, vital sign measurements made at the bedside, laboratory test results, procedures, medications, caregiver notes, imaging reports, and mortality (including post-hospital discharge) . Of the 26 tables, five tables are used to define and another five tables are dictionaries for cross-referencing codes against their respective definitions with the remaining tables containing data associated with patient care, such as physiological measurements, caregiver observations, and billing information.

- Link: https://physionet.org/content/mimiciii/1.4/

### MIMIC-IV

This datset is an update to MIMIC-III, which incorporates contemporary data and improves on numerous aspects of MIMIC-III. MIMIC-IV adopts a modular approach to data organization, highlighting data provenance and facilitating both individual and combined use of disparate data sources. MIMIC-IV is intended to carry on the success of MIMIC-III and support a broad set of applications within healthcare.
## hosp
The hosp module contains data derived from the hospital wide EHR. These measurements are predominantly recorded during the hospital stay, though some tables include data from outside the hospital as well (e.g. outpatient laboratory tests in labevents). Patient demographics (patients), hospitalizations (admissions), and intra-hospital transfers (transfers) are recorded in the hosp module.

## icu
The icu module contains data sourced from the clinical information system at the BIDMC: MetaVision (iMDSoft). MetaVision tables were denormalized to create a star schema where the icustays and d_items tables link to a set of data tables all suffixed with "events". Data documented in the icu module includes intravenous and fluid inputs (inputevents), ingredients for the aforementioned inputs (ingredientevents), patient outputs (outputevents), procedures (procedureevents), information documented as a date or time (datetimeevents), and other charted information (chartevents). All events tables contain a stay_id column allowing identification of the associated ICU patient in icustays, and an itemid column allowing identification of the concept documented in d_items. Additionally, the caregiver table contains caregiver_id, a deidentified integer representing the care provider who documented data into the system. All events tables (chartevents, datetimeevents, ingredientevents, inputevents, outputevents, procedureevents) have a caregiver_id column which links to the caregiver table.

- Link: https://physionet.org/content/mimiciv/2.2

## Evaluation Dataset 
There are 4 tables in the dataset that can be grouped into two categories  discharge summaries and free text radiology reports associated with radiography imaging.  Discharge summaries are long form narratives which describe the reason for a patient’s admission to the hospital, their hospital course, and any relevant discharge instructions. Free-text radiology reports are semi-structured and usually follow a consistent template for a given imaging protocol. 

- Link: https://physionet.org/content/mimic-iv-note/2.2/

# Model



# Evaluation



# Results
- Aim: Streamlined Analysis and detailed summaries of medical records and qualtity of patient care with MedLM

# Reference


# Sponsor : Deloitte
<img width="487" alt="Screenshot 2024-07-04 at 6 58 27 PM" src="https://github.com/swilli21/UVA2024Capstone/assets/51794492/c4c38a31-5999-41e2-ae69-df09738bb175">

