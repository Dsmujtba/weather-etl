# Weather ETL Project

This project is a simple, automated $\text{ETL}$ ($\text{Extract}$, $\text{Transform}$, $\text{Load}$) process written in $\text{Bash}$ shell script. It fetches daily weather data from `wttr.in`, parses the observed and forecasted temperatures, and loads them into a cumulative $\text{TSV}$ ($\text{Tab}$-$\text{Separated}$ $\text{Values}$) log file.

This project fulfills the requirements of the "Practice Project: Introduction" scenario by demonstrating **$\text{Bash}$ scripting**, **$\text{I/O}$ redirection**, **filtering**, and **job scheduling** via $\text{cron}$.

-----

## Project Structure

```
weather_etl/
├── docs/
│   ├── requirements.md  (The project specification)
│   ├── plan.md          (The technical strategy)
│   └── tasks.md         (The step-by-step checklist)
├── scripts/
│   └── run_etl.sh       (The main ETL script)
├── data/
│   ├── weather_report.tsv (The output data file)
│   └── .gitkeep
├── .gitignore
└── README.md            (This file)
```

-----

## How to Run

### 1\. Manual Execution

You can run the $\text{ETL}$ process manually at any time to test the extraction and transformation logic.

1.  **Clone the repository** (if applicable).

2.  **Make the script executable:**

    ```bash
    chmod +x scripts/run_etl.sh
    ```

3.  **Run the script:**

    ```bash
    ./scripts/run_etl.sh
    ```

4.  **Check the output file:**

    ```bash
    cat data/weather_report.tsv
    ```

### 2\. Automated Execution ($\text{Cron}$)

This script is designed to be run automatically by a $\text{cron}$ job every day at noon ($\text{12}$:$\text{00}$ $\text{PM}$) to ensure daily data collection.

1.  **Open your $\text{crontab}$ editor:**

    ```bash
    crontab -e
    ```

2.  **Add the following line** to the file. You **must** replace `/path/to/your/project` with the **absolute path** to the `weather_etl` directory on your machine.

    ```bash
    # Run the weather ETL script every day at 12:00 PM (noon)
    0 12 * * * /bin/bash /path/to/your/project/weather_etl/scripts/run_etl.sh
    ```

3.  **Save and exit** the editor. The $\text{cron}$ job is now active.