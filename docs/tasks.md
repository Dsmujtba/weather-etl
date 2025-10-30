# Weather ETL Task List

This is the official, chronological checklist for building the Weather ETL. It is derived *only* from the approved `plan.md`.

## Phase 1: Project Setup
- [ ] Create the project directory structure: `mkdir -p weather_etl/{docs,scripts,data}`
- [ ] Create the empty project files:
    - `touch docs/requirements.md`
    - `touch docs/plan.md`
    - `touch docs/tasks.md`
    - `touch scripts/run_etl.sh`
    - `touch data/.gitkeep`
    - `touch README.md`
- [ ] Make the main script executable: `chmod +x scripts/run_etl.sh`
- [ ] Add the `#!/bin/bash` shebang to the top of `scripts/run_etl.sh`.

## Phase 2: Extraction & Transformation (R1, R2)
- [ ] **In `scripts/run_etl.sh`:** Add the `curl -s wttr.in/casablanca` command and store its output in a variable (e.g., `RAW_REPORT`).
- [ ] **In `scripts/run_etl.sh`:** Add the `grep/sed` logic to parse the **Observed Temp (`obs_temp`)** from the `$RAW_REPORT` variable. Store it in a new variable (e.g., `OBS_TEMP`).
- [ ] **In `scripts/run_etl.sh`:** Add the `grep/sed` logic to parse the **Forecast Temp (`fc_temp`)** from the `$RAW_REPORT` variable. Store it in a new variable (e.g., `FC_TEMP`).
- [ ] **In `scripts/run_etl.sh`:** Add the `date` command to get the formatted date string and store it in a variable (e.g., `DATE_STR`).
- [ ] **In `scripts/run_etl.sh`:** Combine the three variables into a single, tab-separated string (e.g., `DATA_LINE="$DATE_STR\t$OBS_TEMP\t$FC_TEMP"`).
- [ ] **Test:** Add `echo "$DATA_LINE"` to the script, run it (`./scripts/run_etl.sh`), and verify it prints a single, correctly formatted line (e.g., `2025	10	30	21	23`).

## Phase 3: Load (R3)
- [ ] **In `scripts/run_etl.sh`:** Add the logic to define the script's directory (e.g., `SCRIPT_DIR=$(...)`) and the absolute path to the target file (e.g., `TARGET_FILE="$SCRIPT_DIR/../data/weather_report.tsv"`).
- [ ] **In `scripts/run_etl.sh`:** Implement the "header check" logic (`if [ ! -f "$TARGET_FILE" ]; then ...`).
- [ ] **In `scripts/run_etl.sh`:** Add the `echo` command *inside* the `if` block to write the header (`"year\tmonth\tday\tobs_temp\tfc_temp"`) to the `$TARGET_FILE`.
- [ ] **In `scripts/run_etl.sh`:** Add the final `echo` command *after* the `if` block to append the `$DATA_LINE` to the `$TARGET_FILE`.
- [ ] **Test (Header):**
    1.  Delete `data/weather_report.tsv` if it exists.
    2.  Run `./scripts/run_etl.sh`.
    3.  Verify `weather_report.tsv` contains the header *and* one data line.
- [ ] **Test (Append):**
    1.  Run `./scripts/run_etl.sh` a *second* time.
    2.  Verify `weather_report.tsv` contains the header and *two* identical data lines.

## Phase 4: Automation (R4)
- [ ] **On your machine:** Run `crontab -e` to edit your cron file.
- [ ] **In the crontab editor:** Add the line `0 12 * * * /bin/bash /path/to/your/weather_etl/scripts/run_etl.sh`, making sure to use the *correct absolute path* to your script.
- [ ] **Test (Optional but Recommended):** Change the cron entry to run 1-2 minutes in the future (e.g., if it's 11:15, set to `17 11 * * *`). Wait for it to run and check if `data/weather_report.tsv` has a new line.
- [ ] **Cleanup:** Set the cron entry back to `0 12 * * *` and save.

## Phase 5: Documentation
- [ ] **In `README.md`:** Write a brief project description.
- [ ] **In `README.md`:** Add "How to Run" instructions (manual execution).
- [ ] **In `README.md`:** Add the "Automation" instructions (the `crontab -e` line).
- [ ] **In `docs/`:** Copy the final `requirements.md` and `plan.md` into their respective files.
```eof

---
