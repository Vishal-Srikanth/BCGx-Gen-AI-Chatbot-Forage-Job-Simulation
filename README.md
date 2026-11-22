# BCGx-Gen-AI-Chatbot-Forage-Job-Simulation-
## 10-K Financial Report Analysis & Rule-Based Chatbot: From SEC Filings to Automated Insights
An end-to-end financial data engineering and conversational AI prototype developed for the **BCGx Gen AI Job Simulation on Forage**. This project manually extracts primary financial metrics from official SEC EDGAR 10-K filings, performs multi-year growth feature engineering with Pandas, and powers an interactive, deterministic `if-elif-else` financial chatbot equipped with robust input validation and edge-case error handling.

<!-- Centered Cetrification Screenshot Preview -->
<p align="center">
  <img src="BCGx Forage GenAI Job Simulation Certification Screenshot.png" alt="BCGx Forage GenAI Job Simulation Certificate of Completion" width="600px">
</p>

  #### BCGx Forage GenAI Job Simulation Certificate of Completion
---

## Key Engineering Highlights
- **Manual SEC EDGAR Data Extraction & Structuring:** Curated core financial metrics directly from official Form 10-K filings via the SEC EDGAR database and compiled them into a clean, structured CSV dataset (`Findings from 10-k reports.csv`) built in Google Sheets.
- **Automated YoY Feature Engineering:** Utilizes grouped Pandas percentage-change operations (`groupby('Company').pct_change()`) to dynamically compute year-over-year growth rates across five primary financial indicators.
- **Deterministic Rule-Based Query Engine:** Maps 10 distinct natural language financial inquiries directly to underlying DataFrame metrics using explicit `if-elif-else` conditional routing.
- **Defensive Error & Edge-Case Handling:** Incorporates strict type checking, case-insensitive company normalization, duplicate/missing record detection, and historical baseline guards (preventing invalid YoY growth calculations for the 2023 baseline year).

## The Dataset: SEC EDGAR 10-K Filings (2023–2025)
Data was manually sourced from the [SEC EDGAR Search Filings Portal](https://www.sec.gov/search-filings) across fiscal years **2023, 2024, and 2025** for three major public corporations:
- **Microsoft Corporation (MSFT):** [SEC EDGAR CIK 0000789019](https://www.sec.gov/edgar/browse/?CIK=789019&owner=exclude)
- **Apple Inc. (AAPL):** [SEC EDGAR CIK 0000320193](https://www.sec.gov/edgar/browse/?CIK=320193&owner=exclude)
- **Tesla, Inc. (TSLA):** [SEC EDGAR CIK 0001318605](https://www.sec.gov/edgar/browse/?CIK=1318605&owner=exclude)

### Tracked Financial Metrics (in USD Millions)
- **Raw Extracted Features (5 Variables):** Total Revenue, Net Income, Total Assets, Total Liabilities, and Cash Flow from Operating Activities.
- **Engineered Growth Features (5 Variables):** Year-over-Year (%) Change in Total Revenue, Net Income, Total Assets, Total Liabilities, and Operating Cash Flow.

## Core Technical Concepts & Architecture

### 1. Data Ingestion & Preprocessing
The raw dataset created in Google Sheets and exported as `Findings from 10-k reports.csv` is loaded into a Pandas DataFrame. Baseline year `NaN` values resulting from initial lag calculations are explicitly accounted for during growth-rate computations and summary aggregations (`fillna(0)` for summary views while preserving baseline checks in the chatbot).

### 2. Multi-Year Feature Engineering
Using Pandas `groupby(['Company'])`, year-over-year percentage changes are calculated sequentially across fiscal years:
$$\text{YoY Growth (\%)} = \left(\frac{\text{Metric}_{t} - \text{Metric}_{t-1}}{\text{Metric}_{t-1}}\right) \times 100$$
This isolates each company's trajectory without cross-contaminating adjacent rows between Microsoft, Tesla, and Apple.

### 3. Conditional Rule-Based Routing (`if-elif-else`)
The `simple_chatbot(user_query, company, year)` function evaluates exact string matches against 10 supported financial questions (5 absolute metrics and 5 YoY growth metrics), routing each valid query to its corresponding DataFrame column.

### 4. Multi-Layer Validation & Error Handling
To prevent silent failures or misleading financial outputs, the chatbot enforces sequential validation guards:
- **Out-of-Scope / Misspelled Queries:** Unrecognized or misspelled questions immediately return `"Sorry, I can only provide information on predefined queries."`
- **Input Type & Normalization Guards:** Strips whitespace, performs case-insensitive company matching (`.casefold()`), and validates that `year` is passed as a strict integer (rejecting booleans or non-numeric strings).
- **Data Integrity Checks:** Verifies that exactly one record exists for the requested `(Company, Year)` pair, flagging missing entries or duplicate records before querying.
- **Baseline Year Protection:** Explicitly checks for prior-year (`year - 1`) data before returning YoY percentage changes, returning `"Year-over-year growth is unavailable..."` for 2023 queries where 2022 baseline figures do not exist.

## Financial Analysis & Results Summary
- **Microsoft:** Demonstrated consistent expansion across all three years, averaging **+10.20%** YoY revenue growth and **+12.45%** YoY net income growth.
- **Apple:** Maintained steady top-line growth (**+2.82%** average YoY revenue growth) with a strong net income rebound of **+19.50%** in 2025 following a slight contraction (**-3.36%**) in 2024.
- **Tesla:** Experienced top-line flattening in 2024 (**+0.95%**) followed by a **-2.93%** revenue contraction in 2025, alongside significant net income compression (**-32.78%** average YoY change).

## Skills learned in this simulation
AI Development, Data Collection, Data Science, Financial Analysis, Logical Reasoning, Microsoft Excel, Natural Language Processing, Python Programming

## Tech Stack
- **Language:** Python 3.10
- **Data Manipulation & Analysis:** Pandas
- **Data Sourcing & Preparation:** SEC EDGAR Database, Google Sheets (CSV)
- **Environment:** Jupyter Notebook (VS Code)
