# Lead Scoring & Pipeline Manager

This is a new project: a web application for analyzing CRM leads, scoring their priority, and managing a sales pipeline.

## Project Purpose

The project is created for sales managers and CRM teams who need to quickly understand which leads require attention first. The application helps users upload a lead list, calculate lead scores, estimate conversion probability, and view the sales pipeline in an interactive dashboard.

## Main Features

- upload a CSV file with leads;
- use the built-in sample dataset from `sample_leads.csv`;
- calculate a lead score for each lead;
- assign lead priority: High, Medium, or Low;
- calculate conversion probability;
- generate pricing or next-step recommendations;
- filter leads by manager, pipeline stage, industry, budget, and conversion probability;
- show KPI blocks with lead count, average score, high-priority leads, and total pipeline value;
- display a lead table for CRM review;
- show charts for pipeline stage distribution and manager performance;
- highlight top opportunities;
- display a detailed view for the selected lead;
- show a Trello-style Data QA Board with CSV errors, warnings, and follow-up tasks;
- let clients describe problems in plain text, analyze the message, and send an automatically prepared task to a Trello To Do list through the Trello API;
- support Bitrix24-style CRM fields, including `bitrix_owner`;
- include a health-check server for deployment availability checks.

## Technologies

- Python 3.12;
- Streamlit for the web interface;
- Pandas for data processing and analysis;
- CSV as the input data format;
- Trello API for creating To Do cards from client problem reports;
- built-in Python HTTP server for health checks.

## Project Structure

- `app.py` - main Streamlit application file;
- `sample_leads.csv` - sample data for testing;
- `requirements.txt` - project dependencies;
- `runtime.txt` - Python version for deployment;
- `.streamlit/` - Streamlit configuration.

## CSV Format

Recommended columns:

```text
lead_id,company,manager,industry,source,region,stage,budget,deal_value,last_activity_days,meetings_booked,email_open_rate,employees,bitrix_owner
```

Required fields:

- `lead_id`;
- `company`;
- `manager`;
- `industry`;
- `source`;
- `region`;
- `stage`;
- `budget`;
- `deal_value`;
- `last_activity_days`;
- `meetings_booked`;
- `email_open_rate`;
- `employees`.

Optional field:

- `bitrix_owner`.

## Data QA Board

The application includes a Trello-style board for data analysis issues. After a CSV file is loaded, the board displays:

- `Errors` - missing required columns, empty required values, and invalid numeric values;
- `Warnings` - duplicate lead IDs, unknown pipeline stages, negative values, or invalid email open rate range;
- `To Do` - follow-up tasks generated from the dataset, such as inactive leads or weak email engagement.

This feature helps users understand whether the CSV file is correct before making CRM or sales decisions.

## Trello API Integration

The project includes a client problem reporting form. A user writes a problem in plain text, and the application acts like a small support bot:

- analyzes the problem message;
- detects a category, such as CSV/Data Import, CRM/Leads, Dashboard/UI, API/Integration, or Performance;
- assigns a priority;
- creates a clear Trello card title;
- writes a structured card description with the original client message and suggested next step;
- sends the card to the Trello To Do list;
- returns the created Trello card link in the interface.

Example result:

```text
Problem was added to Trello: https://trello.com/c/exampleCard
```

Required Trello configuration:

```text
TRELLO_API_KEY
TRELLO_TOKEN
TRELLO_LIST_ID
```

These values can be stored as environment variables or Streamlit secrets. If Trello is not configured, the application keeps the submitted problem locally in the current session and shows a warning.

The Trello integration uses this API endpoint:

```text
POST https://api.trello.com/1/cards
```

## CRM and API Notes

The project uses CRM-style data and workflow logic. It works with leads, managers, pipeline stages, deal values, priorities, and Bitrix24-style owner fields.

At the current stage, CRM lead data is imported through CSV files. The project also has Trello API integration for support/problem tasks. A future version can add direct CRM API integration, for example Bitrix24 API or another CRM service.

The project already includes a small health-check API endpoint for deployment monitoring:

```text
/health
```

It returns a simple `ok` response when the application health-check server is enabled.

## Local Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the application:

```bash
streamlit run app.py
```

After launch, the application opens in the browser as an interactive dashboard.

## Deployment

The project is suitable for deployment on Streamlit Cloud. Deployment requires `app.py`, `requirements.txt`, `runtime.txt`, and the project data files.
