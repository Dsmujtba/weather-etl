# Project Requirements: Daily Weather ETL (Proof-of-Concept)

## Overview

The goal is to create an automated **Extract, Transform, and Load ($\text{ETL}$)** process using shell scripts to build a historical report of weather forecast accuracy.

---

## R1: Extraction

**User Story:** As a data engineer, I want to automatically fetch raw weather data from a single source every day at a specific time.

| Acceptance Criteria | Description |
| :--- | :--- |
| **THEN:** The system **SHALL** fetch data from the `wttr.in` web service. | Define the external data source. |
| **THEN:** The system **SHALL** request data for a single station: **"Casablanca, Morocco"**. | Define the location parameter. |
| **THEN:** The system **SHALL** use a shell command (e.g., `curl`) for extraction. | Define the primary tool for data retrieval. |
| **THEN:** The raw text output from `curl wttr.in/casablanca` **SHALL** be used as the source for parsing. | Define the raw data input format. |

---

## R2: Transformation & Parsing

**User Story:** As an analyst, I need specific data points extracted from the raw text to compare observed weather to the forecast.

| Acceptance Criteria | Description |
| :--- | :--- |
| **THEN:** The system **SHALL** find and extract the **actual (observed) temperature** for the current day at noon. | Data Point 1 (Observed). |
| **THEN:** The system **SHALL** find and extract the **forecasted temperature** for the **following day** at noon. | Data Point 2 (Forecast). |
| **THEN:** The system **SHALL** get the current date, separated into **year, month, and day**. | Data Point 3 (Date Metadata). |
| **THEN:** All temperature values **SHALL** be in **Celsius** and stored as plain numbers (no units). | Define data type and cleaning rule. |

---

## R3: Load

**User Story:** As an analyst, I want this data saved in a single, cumulative, tabular log file so I can analyze trends over time.

| Acceptance Criteria | Description |
| :--- | :--- |
| **THEN:** The system **SHALL** save the data to a single, persistent log file (e.g., `weather_report.tsv`). | Define the target persistence. |
| **THEN:** The file format **SHALL** be tabular (e.g., **tab-separated**). | Define the file structure. |
| **THEN:** The script **SHALL** **append** the new daily record, not overwrite the file. | Define the $\text{I/O}$ mechanism (`>>`). |
| **THEN:** The file **SHALL** contain exactly **5 columns**, in this order: `year`, `month`, `day`, `obs_temp`, `fc_temp`. | Define the schema. |
| **THEN:** The file **SHALL** contain a **header row**. This header row **SHALL** be written exactly once, only if the file does not already exist. | Define the header logic. |

---

## R4: Automation & Scheduling

**User Story:** As a data engineer, I want this entire $\text{ETL}$ process to run automatically without manual intervention.

| Acceptance Criteria | Description |
| :--- | :--- |
| **THEN:** The entire $\text{ETL}$ logic ($\text{Extract}$, $\text{Transform}$, $\text{Load}$) **SHALL** be contained within a single shell script. | Define the packaging mechanism. |
| **THEN:** This script **SHALL** be scheduled to run automatically every day at **12:00 $\text{PM}$ (noon)** local time. | Define the frequency and time. |
| **THEN:** The scheduling **SHALL** be implemented using **$\text{cron}$**. | Define the automation tool. |

---

## R5: Out of Scope (for this POC)

The following items are explicitly excluded from the current Proof-of-Concept scope:

* Multiple locations or a list of locations.
* Multiple forecast sources.
* Different update frequencies (e.g., hourly).
* Other weather metrics (e.g., wind speed, precipitation, visibility).