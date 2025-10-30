# Weather ETL Implementation Plan

This plan outlines the technical strategy for building the Weather $\text{ETL}$, as defined in `docs/requirements.md`. It details the tools, specific $\text{Bash}$ logic, and critical decisions for each phase.

-----

## 1\. Phase 1: Extraction ($\mathbf{R1}$)

| Field | Detail |
| :--- | :--- |
| **Tool** | `curl` |
| **Strategy** | The script will use `curl -s wttr.in/casablanca` to fetch the raw $\text{ASCII}$ $\text{art}$ weather report. The **`-s`** (silent) flag is a critical strategic decision to suppress progress meters, ensuring a clean text output for parsing. |
| **Challenge** | The primary technical challenge of this project is that the source data is unstructured $\text{ASCII}$ art, not a structured format like $\text{JSON}$. All parsing logic must be robust against this text-based format. |
| **Output** | The raw string output from `curl` will be piped (`|`) directly into the Transformation phase. |

-----

## 2\. Phase 2: Transformation & Parsing ($\mathbf{R2}$)

This is the most complex phase, relying on standard $\text{Unix}$ tools to extract data from the unstructured text block.

| Data Point | Strategy & Pattern | $\text{Bash}$ Tools Used |
| :--- | :--- | :--- |
| **Date Fields** | We will use `date +'%Y\t%m\t%d'` to get the date pre-formatted with the required tab separators. | `date` |
| **Observed Temp** (`OBS_TEMP`) | **Source:** The line containing the city name and the current temperature (e.g., `Weather report: Casablanca, ... +20°C`). **Pattern:** We will use `grep` to find the relevant line and a combination of `sed` or `awk` to isolate the temperature string (e.g., `+20°C`) and extract only the number (e.g., `20`). | `grep`, `sed`, `awk` |
| **Forecast Temp** (`FC_TEMP`) | **Source:** The second "Noon" forecast (the one for tomorrow). **Pattern:** A robust strategy is to use `grep -A 2 "Noon"` (to get all "Noon" blocks), pipe that to `sed -n '5p'` (to get the 5th line, which is the temp for the second "Noon" block), and then pipe that to `sed` to extract the final numerical temperature. | `grep`, `sed` |
| **Final Transformation** | The extracted variables (`DATE_STR`, `OBS_TEMP`, `FC_TEMP`) will be combined into a single, tab-separated string: `DATA_LINE="$DATE_STR\t$OBS_TEMP\t$FC_TEMP"`. | `echo`, $\text{variables}$ |

-----

## 3\. Phase 3: Load ($\mathbf{R3}$)

| Field | Detail |
| :--- | :--- |
| **Target File** | A persistent log file: `data/weather_report.tsv`. |
| **Critical Logic (Header)** | The script must implement the "header check" logic: \<ul\>\<li\>Define `TARGET_FILE=".../data/weather_report.tsv"`.\</li\>\<li\>Check if `[ ! -f "$TARGET_FILE" ]; then ...`\</li\>\<li\>If the file does not exist: Write the header: `echo -e "year\tmonth\tday\tobs_temp\tfc_temp" > "$TARGET_FILE"`\</li\>\<li\>In all cases: Append the data line using the append operator: `echo -e "$DATA_LINE" >> "$TARGET_FILE"`.\</li\>\</ul\> |
| **Cron Consideration** | The script must use an **absolute path** to the `$TARGET_FILE` (e.g., derived from `SCRIPT_DIR=$(...)`) to ensure it runs correctly from $\text{cron}$, which executes jobs from an arbitrary working directory. |

-----

## 4\. Phase 4: Automation ($\mathbf{R4}$)

| Field | Detail |
| :--- | :--- |
| **Strategy** | The entire logic (Phases 1-3) will be contained in a single executable shell script: `scripts/run_etl.sh`. |
| **Tool** | `crontab` |
| **Entry** | A `crontab -e` entry will be created to run the script at noon: `0 12 * * * /bin/bash /path/to/your/project/scripts/run_etl.sh` (The user must provide the final absolute path). |