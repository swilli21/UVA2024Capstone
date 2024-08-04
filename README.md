# Introduction
Google and Deloitte working together to improve provider search

Healthcare organizations are often bogged down with administrative tasks and processes related to documentation, care navigation, and member engagement. 
Deloitte, Google Cloud, and healthcare leaders are piloting MedLM’s capabilities to make it easier for care teams to discover information from provider directories and benefits documents. MedLM offers flexibility to healthcare organizations and their different needs. Healthcare organizations are exploring the use of AI for a range of applications, from basic tasks to complex workflows.
The development of these models has been informed by specific healthcare and life sciences customer needs, such as answering a healthcare provider’s medical questions and drafting summaries. 

This project aims to test whether using MedLM for the automated summarization of structured and semi-structured medical data will lead to more efficient and accurate extraction of patient information. By creating comprehensive and concise summaries that capture essential details about a patient's medical history, current state, and treatment protocols, contributing to enhanced patient care and management.

# Data
The Medical Information Mart for Intensive Care (MIMIC) database provided critical care data for over 2.3 million Radiology Reports and 40,000 patient care records of deidentified data.

# Ethics 
- Responsible AI: 
Safety best practices 
Large language models (LLMs) can translate language, summarize text, generate creative writing, generate code, power chatbots and virtual assistants, and complement search engines and recommendation systems. At the same time, as an early-stage technology, its evolving capabilities and uses create potential for misapplication, misuse, and unintended or unforeseen consequences. Large language models can generate output that you don't expect, including text that's offensive, insensitive, or factually incorrect.

Link - https://cloud.google.com/vertex-ai/generative-ai/docs/learn/responsible-ai

- Data Usage : 
The collection of patient information and creation of the research resource was reviewed by the Institutional Review Board at the Beth Israel Deaconess Medical Center, who granted a waiver of informed consent and approved the data sharing initiative.

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

# Model - MedLM prompts

MedLM-large is a foundation model for medical question answering and summarization. You can access the models using the Vertex AI MedLM API.
To interact with the MedLM models, you send natural language instructions, also called prompts, that tell the model what you want it to generate. However, LLMs can sometimes behave in unpredictable ways.


# Method

-- Interact with MedLM API using: Vertex AI SDK for Python

-- Install Google Cloud AI Platform and authorize Google Cloud Client

-- Use Big Query to connect to the database

-- (optional) Use NLP techniques to extract keywords

-- (optional) Once list of potential keywords are generated, use TF-IDF to assign respective weights

-- Create SQLDatabase instance and SQLDatabaseChain

-- Define your natural language query

-- Generate SQL and execute query/question out

-- Initialize Google Cloud aiplatform client that will be used to create and send requests to the MedLM model to summary rag text from the generated sql.

# Evaluation

### Metric: Cosine Similarity
- Cosine similarity works best when the rag text and MedLM output are semantically related. 

Cosine Calculation: Once we have the vectors, we calculate the cosine of the angle between them. The cosine value ranges from -1 to 1:
- 1: The vectors are perfectly aligned (identical).
- 0: The vectors are orthogonal (completely different).
- -1: The vectors are perfectly opposite.

Interpretation: A higher cosine similarity score indicates a greater degree of similarity between the model's output and the rag text. If they are discussing completely different topics, the similarity score will be low, even if the words are similar.

### Metric: Bleu (Recall-Oriented Understudy for Gisting Evaluation) and Rogue Scores

- Bleu score is a metric that measures how similar machine-translated text is to a set of human-created reference translations. It focuses on precision
  
Interpretation: The score is a number between 0 and 1, with higher scores indicating closer matches to the reference text. A score of 0 means there is no overlap between the machine-translated text and the reference translation, while a score of 1 indicates perfect overlap.

- ROUGE focuses on recall, measuring how well the generated text covers the key information present in the rag text. 

ROUGE variants: each variant calculates the recall of the generated text compared to the reference text.
- ROUGE-N (measuring the overlap of n-grams)
  - ROUGE-1 compute the precision, recall, and F1-score of the matching n-grams.
  - ROUGE-2 precision is the ratio of the number of 2-grams
- ROUGE-L (measuring the longest common subsequence)

Interpretation: A higher ROUGE score indicates that the MedLM output captures more of the important information from the rag text.

# Results
- Aim:
  1. Streamlined Analysis
  2. detailed summaries of medical records 
  3. qualtity of patient care with MedLM

#### Cosine Similarity

| Weights       | Fine Tuned    |  Base Model   |
| ------------- | ------------- | ------------- |
| Keyword       | 0.7291460037  | 0.7106310129  |
| Vector        | 0.77877474    | 0.5431788563  |
| Multimodal    | Content Cell  | Content Cell  |

#### Bleu and Rouge Scores
###### Keyword Weights
|      Model    | Bleu          |  Rouge-1      |  Rouge-2      |  Rouge-L      |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Fine Tuned    | 8.796e-232    |'r':0.29, 'p':0.111, 'f':0.16 |r':0, 'p':0, 'f':0 |'r': 0.23, 'p':0.08, 'f':0.12 |
| Base Model    | 8.231e-232    |'r':0.23, 'p':0.08, 'f':0.12 |r':0, 'p':0, 'f':0 |'r': 0.23, 'p':0.08, 'f':0.12 |

###### Vector Weights
|      Model    | Bleu          |  Rouge-1      |  Rouge-2      |  Rouge-L      |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Fine Tuned    | 9.947e-232    |'r':0.45, 'p':0.15, 'f':0.22 |r':0, 'p':0, 'f':0 |'r':0.36, 'p':0.12, 'f':0.18 |
| Base Model    | 9.947e-232    |'r':0.45, 'p':0.15, 'f':0.22 |r':0, 'p':0, 'f':0 |'r':0.36, 'p':0.12, 'f':0.18 |

###### Multimodal Weights
|      Model    | Bleu          |  Rouge-1      |  Rouge-2      |  Rouge-L      |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Fine Tuned    | 9.157e-232    |'r':0.583, 'p':0.103, 'f':0.1245 |r':0, 'p':0, 'f':0 |'r': 0.416, 'p':0.074, 'f':0.125 |
| Base Model    | 9.157e-232    |'r':0.583, 'p':0.103, 'f':0.1245 |r':0, 'p':0, 'f':0 |'r': 0.416, 'p':0.074, 'f':0.125 |

Where: 'r' - recall score ; 'p' - precision ; 'f' - f1 score

<img width="382" alt="Screenshot 2024-08-03 at 7 29 27 PM" src="https://github.com/user-attachments/assets/3ee1e808-3033-4382-a36a-88a608670890">


# Sponsor : Deloitte
<img width="487" alt="Screenshot 2024-07-04 at 6 58 27 PM" src="https://github.com/swilli21/UVA2024Capstone/assets/51794492/c4c38a31-5999-41e2-ae69-df09738bb175">

# Reference
- https://cloud.google.com/blog/topics/healthcare-life-sciences/introducing-medlm-for-the-healthcare-industry
- https://cloud.google.com/vertex-ai/generative-ai/docs/medlm/medlm-prompts
- https://cloud.google.com/sdk/docs/install-sdk#linux
- https://api.python.langchain.com/en/latest/sql/langchain_experimental.sql.base.SQLDatabaseChain.html
- https://www.freecodecamp.org/news/what-is-rouge-and-how-it-works-for-evaluation-of-summaries-e059fb8ac840/


