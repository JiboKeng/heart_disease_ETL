{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "cb1eb48b-755d-4082-926e-3597206c880d",
   "metadata": {
    "jp-MarkdownHeadingCollapsed": true
   },
   "source": [
    "# UCI Heart Disease Data Multi-source Parser\n",
    "\n",
    "##  Project Overview\n",
    "This project provides a robust, modular Python pipeline for extracting and standardizing clinical heart disease data from four international research sites:\n",
    "* **Cleveland Clinic Foundation**\n",
    "* **Hungarian Institute of Cardiology, Budapest**\n",
    "* **University Hospital, Zurich, Switzerland**\n",
    "* **VA Medical Center, Long Beach, CA**\n",
    "\n",
    "The primary challenge addressed is the inconsistent data formatting across sites, where record lengths vary between 76 and 90 attributes.\n",
    "\n",
    "##  Technical Implementation\n",
    "The core logic is built using a **single-responsibility function** approach to ensure maintainability and scalability. Key features include:\n",
    "\n",
    "* **Custom Stream Parsing:** Uses Python's `with open()` context manager to read raw `.data` streams, segmenting records based on the clinical `name` anchor.\n",
    "* **Schema Standardization:** Implements a strict validation and slicing logic that standardizes all incoming records to the \"Golden 76\" attribute format, discarding metadata noise from 90-column datasets.\n",
    "* **Data Integrity:** Automatically injects a `source_dataset` identifier as the final column of each record before concatenation to preserve data provenance without shifting clinical feature indices.\n",
    "\n",
    "\n",
    "##  Pipeline Flow\n",
    "1. **Ingestion:** Reads raw text files from the UCI Machine Learning Repository.\n",
    "2. **Validation:** Loops through records to audit lengths, identifying and filtering anomalies (records < 76 columns).\n",
    "3. **Transformation:** Slices records to 76 attributes and appends the source site ID.\n",
    "4. **Consolidation:** Vertically concatenates site-specific DataFrames into a single `heart_disease_master.csv`.\n",
    "\n",
    "##  Quick Start\n",
    "To generate the master dataset, ensure you have `pandas` and `numpy` installed, then run:\n",
    "\n",
    "```python\n",
    "from heart_parser import parse_clinical_site\n",
    "import pandas as pd\n",
    "\n",
    "# Define files\n",
    "files = ['cleveland.data', 'hungarian.data', 'switzerland.data', 'long-beach-va.data']\n",
    "\n",
    "# Process and Combine\n",
    "all_dfs = [parse_clinical_site(f) for f in files]\n",
    "master_df = pd.concat(all_dfs, ignore_index=True)\n",
    "master_df.to_csv('heart_disease_master.csv', index=False)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.13.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
