---
layout: page
permalink: /data-description/
title: Data Description
description: Description and additional resources for each dataset
nav: false
nav_order: 3
---

## MIMIC-III (Medical Information Mart for Intensive Care III)

A large, freely-available database containing de-identified health-related data associated with over 40,000 critical care patients. It provides a comprehensive view of patient data, including demographics, vital signs, laboratory results, medications, and more.

- **Key Features**:
  - Over 58,000 hospital admissions.
  - Data spanning from 2001 to 2012.
  - Includes ICU data such as prescriptions, procedures, and diagnostic codes.

## Resources

- [Detailed Documentation](https://mimic.mit.edu/docs/iii/)
- [GitHub Code Resource](https://github.com/MIT-LCP/mimic-code)

### Quick Links

- [Database Schema](https://pi.cs.oswego.edu/~jmiles3/mimic/assets/MIMIC-ER-DIAGRAM.jpg): By Joseph Miles
- [Notebooks](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iii/notebooks): Examples of data extraciton and analysis in R markdown and jupyter Notebooks
- [Tutorials](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iii/tutorials): Focues on explaining concepts to new users

### Tools

- [Bloatectomy](https://github.com/MIT-LCP/bloatectomy) ([paper](https://github.com/MIT-LCP/bloatectomy/blob/master/bloatectomy_paper.pdf)) - A python based package for removing duplicate text in clinical notes
- [Medication categories](https://github.com/mghassem/medicationCategories) - Python script for extracting medications from free-text notes
- [MIMIC Extract](https://github.com/MLforHealth/MIMIC_Extract) ([paper](https://doi.org/10.1145/3368555.3384469)) - A python based package for transforming MIMIC-III data into a machine learning friendly format
- [FIDDLE](https://github.com/MLD3/FIDDLE) ([paper](https://doi.org/10.1093/jamia/ocaa139)) - A python based package for a FlexIble Data-Driven pipeLinE (FIDDLE), transforming structured EHR data into a machine learning friendly format

### Papers

- [DRGCoder](https://aclanthology.org/2023.emnlp-demo.34.pdf): An explainability-enhanced clinical claim coding system for the early prediction of medical severity DRGs (MS-DRGs).
  - Introduces novel multi-task Transformer model for MS-DRG prediction
  - Allow visualizations of salient words for better explainability
  - Identifies diseases for comparision among other discharge summaries
- [Adapted Large Language Models Can Outperform Medical Experts in Clinical Text Summarization](https://arxiv.org/pdf/2309.07430) ([Code](https://github.com/StanfordMIMI/clin-summ))
  - four distinct clinical summarization task (radiology reports, patient questions, progress notes, and doctor-patient dialogue)
  - 10 physicians evaluated the summary completeness, correctness, and conciseness. [ equivalent(46%) or superior(36%) compared to summaries from medical experts.]
- [GraphCare: Enhancing Healthcare Predictions with Personalized Knowledge Graphs](https://arxiv.org/pdf/2305.12788) - ([Code](https://github.com/pat-jj/GraphCare))
  - Mortality Prediction, Length of Stay Prediction, Readmission Prediction, Drug Recommendation
  - Explictly uses MIMIC-III and MIMIC-IV datasets

---

## MIMIC-IV (Medical Information Mart for Intensive Care IV)

It is the latest iteration of the MIMIC database, featuring updated data from the Beth Israel Deaconess Medical Center (BIDMC). This dataset improves upon MIMIC-III with more structured and expanded records, offering even greater research potential.

- **Key Features**:
  - Covers data from 2008 to 2019.
  - Improved modularity and linkage of data tables for better usability.
  - Includes high-resolution waveform data for advanced analysis.

## [Modules](https://mimic.mit.edu/docs/iv/modules/)

- Core
  - [HOSP](https://mimic.mit.edu/docs/iv/modules/hosp/): Hospital level data. Collected from hospital Electronic Health Record Systems. (patients, admissions, labs, medication, and billing information).
  - [ICU](https://mimic.mit.edu/docs/iv/modules/icu/): Data collected from Clinical Information Systems in the ICU.
- [ED](https://mimic.mit.edu/docs/iv/modules/ed/): Data Related to Emergency Department.
- [CXR](https://mimic.mit.edu/docs/iv/modules/cxr/): Provides lookup tables for linking patients with MIMIC-CXR (links patients chest x-rays to the MIMIC-IV modules).
- [ECG](https://mimic.mit.edu/docs/iv/modules/ecg/): Provides waveforms data. Provides lookup tables to link with other MIMIC-IV modules.
- [Note](https://mimic.mit.edu/docs/iv/modules/note/): Contains De-identified free-text clinical notes for hospitalized patients.

## Resources

- [Detailed Documentation](https://mimic.mit.edu/docs/iv/)
- [GitHub Code Resource](https://github.com/MIT-LCP/mimic-code)
- [Creation Process](https://www.nature.com/articles/s41597-022-01899-x/figures/1)


### Important Links

- [Mimic-iv Tables Overview](https://slideslive.com/embed/presentation/38931965) by Alistair Johnson
- [Reporduction of a study in MIMIC-IV](https://slideslive.com/embed/presentation/38932058) by Alistair Johnson
        - ([Code](https://github.com/alistairewj/mimic-iv-aline-study))
- [Notebook](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv/notebooks/tableone.ipynb): Shows how to use the data in MIMIC-IV

---

## Extra

### Papers we can look into

- [MedDialog](https://aclanthology.org/2020.emnlp-main.743.pdf): Large-scale Medical Dialogue Datasets
  - Contains Chinese dataset with 3.4 million conversations and English Dataset with 0.26 million conversations between patients and doctors
  - Could be useful in medical dialogue systems
- [Development and validation of a machine-learning model for predicting the risk of death in sepsis patients with acute kidney injury](https://www.cell.com/heliyon/fulltext/S2405-8440(24)06016-X)
  - Explictly uses MIMIC-III and MIMIC-IV datasets
- [Irregular Time Series Papers]({{ site.baseurl }}/irregular-time-series-papers)

### Additional Resources

- [Standardized Data: The OMOP Common Data Model](https://www.ohdsi.org/data-standardization/): The Observational Medical Outcomes Partnership (OMOP) Common Data Model (CDM) is an open community data standard, designed to standardize the structure and content of observational data and to enable efficient analyses that can produce reliable evidence.
  - [Tutorials on OMOP Common Data Model](https://www.youtube.com/watch?v=vHMkBaHJrDA)
  - [The Book of OHDSI](https://ohdsi.github.io/TheBookOfOhdsi/index.html)
- [Collection of Graph-based Deep Learning Literatures](https://github.com/naganandy/graph-based-deep-learning-literature)
