Project Requirements: Daily Weather ETL (Proof-of-Concept)

1. Overview

The goal is to create an automated Extract, Transform, and Load (ETL) process using shell scripts to build a historical report of weather forecast accuracy.

R1: Extraction

User Story: As a data engineer, I want to automatically fetch raw weather data from a single source every day at a specific time.

Acceptance Criteria:

WHEN: The process runs

THEN: The system SHALL fetch data from the wttr.in web service.

THEN: The system SHALL request data for a single station: "Casablanca, Morocco".

THEN: The system SHALL use a shell command (e.g., curl) for extraction.

THEN: The raw text output from curl wttr.in/casablanca SHALL be used as the source for parsing.

R2: Transformation & Parsing

User Story: As an analyst, I need specific data points extracted from the raw text to compare observed weather to the forecast.

Acceptance Criteria:

WHEN: The raw text is processed

THEN: The system SHALL find and extract the actual (observed) temperature for the current day at noon.

THEN: The system SHALL find and extract the forecasted temperature for the following day at noon.

THEN: The system SHALL get the current date, separated into year, month, and day.

THEN: All temperature values SHALL be in Celsius and stored as plain numbers.

R3: Load

User Story: As an analyst, I want this data saved in a single, cumulative, tabular log file so I can analyze trends over time.

Acceptance Criteria:

WHEN: The data is transformed

THEN: The system SHALL save the data to a single, persistent log file (e.g., weather_report.tsv).

THEN: The file format SHALL be tabular (e.g., tab-separated).

THEN: The script SHALL append the new daily record, not overwrite the file.

THEN: The file SHALL contain exactly 5 columns, in this order: year, month, day, obs_temp, fc_temp.

THEN: The file SHALL contain a header row. This header row SHALL be written exactly once, only if the file does not already exist.

R4: Automation & Scheduling

User Story: As a data engineer, I want this entire ETL process to run automatically without manual intervention.

Acceptance Criteria:

WHEN: The project is deployed

THEN: The entire ETL logic (Extract, Transform, Load) SHALL be contained within a single shell script.

THEN: This script SHALL be scheduled to run automatically every day at 12:00 PM (noon) local time.

THEN: The scheduling SHALL be implemented using cron.

R5: Out of Scope (for this POC)

Multiple locations or a list of locations.

Multiple forecast sources.

Different update frequencies (e.g., hourly).

Other weather metrics (e.g., wind speed, precipitation, visibility).