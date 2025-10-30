Weather ETL Project

This project is a simple, automated ETL (Extract, Transform, Load) process written in Bash shell script. It fetches daily weather data from wttr.in, parses the observed and forecasted temperatures, and loads them into a cumulative TSV (Tab-Separated Values) log file.

This project fulfills the requirements of the "Practice Project: Introduction" scenario.

Project Structure

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


How to Run

1. Manual Execution

You can run the ETL process manually at any time.

Clone the repository (if applicable).

Make the script executable:

chmod +x scripts/run_etl.sh


Run the script:

./scripts/run_etl.sh


After running, you can check the output file:

cat data/weather_report.tsv


2. Automated Execution (Cron)

This script is designed to be run automatically by a cron job every day at noon.

Open your crontab editor:

crontab -e


Add the following line to the file. You must replace /path/to/your/project with the absolute path to the weather_etl directory on your machine.

# Run the weather ETL script every day at 12:00 PM (noon)
0 12 * * * /bin/bash /path/to/your/project/weather_etl/scripts/run_etl.sh


Save and exit the editor. The cron job is now active.
