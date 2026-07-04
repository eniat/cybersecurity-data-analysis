# CSVI Vulnerability Risk Analysis

Notebook-based vulnerability risk analysis using supplied CSVI data across six UK sectors and fifty organisations.

The project cleans and validates CVE data, scores organisation-level exposure, identifies at-risk organisations, clusters CVEs by severity and exploitation likelihood, and highlights common CVE patterns across organisations.

## What this project demonstrates

- Vulnerability risk prioritisation using CVSS, EPSS, KEV, and patch availability.
- Data cleaning and validation on inconsistent security datasets.
- Organisation and sector-level risk scoring.
- KMeans clustering for CVE grouping.
- Co-occurrence analysis to find common vulnerability bundles.
- Technical reporting and stakeholder-focused risk communication.

## Tech stack

Python, Jupyter Notebook, pandas, NumPy, matplotlib, scikit-learn

## Repository structure

```text
.
├── analysis.ipynb       # Main analysis notebook
├── csvi_data.json       # Sector, organisation, and CVE exposure data
├── csvi_cves.json       # CVE attributes used for scoring
├── SCC-354-Report.pdf   # Full written report and stakeholder brief
└── README.md
```

## Report

The repository includes `SCC-354-Report.pdf`, which documents the full analysis behind the notebook.

The report explains the objective, data cleaning decisions, exploratory analysis, risk scoring, at-risk organisation rules, KMeans clustering, CVE co-occurrence results, stakeholder recommendations, and limitations.

Use the notebook to inspect and reproduce the analysis. Use the report to understand the reasoning, evidence, and final conclusions.

## Dataset

The analysis uses two JSON files:

- `csvi_data.json` maps sectors and organisations to their listed CVEs.
- `csvi_cves.json` provides CVE attributes such as CVSS, EPSS, KEV, attack vector, attack complexity, and patch availability.

## Methodology

### 1. Data preparation

The notebook loads the JSON files and converts the nested sector/organisation structure into a flat organisation-CVE dataset.

Cleaning steps include:

- validating CVE IDs;
- checking duplicate CVE records;
- normalising patch, KEV, and attack-vector values;
- converting CVSS and EPSS to numeric fields;
- tracking missing or invalid CVE attribute records.

### 2. Risk scoring

Each organisation-CVE row is given a priority score:

```text
priority = 0.5 * (CVSS / 10) + 0.5 * EPSS
```

This balances technical severity with exploitation likelihood.

Organisation-level metrics include CVE count, total priority score, KEV count, known patch coverage, and unpatched-known rate.

### 3. At-risk organisation detection

Organisations are flagged if they exceed the 95th percentile for at least one risk dimension:

- high exposure;
- high KEV burden;
- weak patch hygiene.

This avoids relying on a single risk metric.

### 4. CVE clustering

KMeans clustering is applied to CVE-level CVSS and EPSS values.

The notebook uses inertia and silhouette score to choose the cluster count, then identifies the cluster with the strongest combined severity and exploitation likelihood.

### 5. Co-occurrence analysis

A CVE co-occurrence matrix identifies vulnerabilities that repeatedly appear together across organisations.

This shows where fixing a small set of common CVEs could reduce exposure across multiple sectors.

## Key findings

- Retail has the highest exposure volume and highest severity baseline.
- Energy has lower exposure but the highest KEV rate and weak patch hygiene.
- Finance has lower KEV exposure but poor patch hygiene.
- Manufacturing and Water show high KEV rates but stronger remediation posture.
- Several CVEs appear across many organisations, making them strong remediation targets.
- Combining CVSS and EPSS gives a better operational priority view than CVSS alone.

## How to run

```bash
git clone <repo-url>
cd <repo-name>
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate

pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `analysis.ipynb` and run the cells from top to bottom.

## Outputs

The notebook produces data quality checks, CVSS/EPSS distributions, KEV and patch summaries, sector comparisons, at-risk organisation rankings, KMeans visualisations, a top-priority CVE shortlist, and a CVE co-occurrence heatmap.

The PDF report turns these outputs into a structured written analysis and stakeholder brief.

## Limitations

- Results depend on the quality and completeness of the supplied data.
- Missing CVSS or EPSS values reduce scoring and clustering confidence.
- The priority score uses equal CVSS/EPSS weighting, which may not fit every risk appetite.
- Co-occurrence shows shared presence, not causality.
- KMeans is interpretable but sensitive to the selected cluster count.

## Context

This project was completed in an academic setting using supplied vulnerability data. The focus is practical vulnerability management: cleaning imperfect data, prioritising remediation, and communicating risk clearly.

## Usage Notice

This repository is provided for portfolio and review purposes only.

All rights are reserved. No permission is granted to copy, redistribute, submit, or reuse this work, in whole or in part, for academic coursework, assessment, or commercial purposes.

Where this repository relates to university coursework, it is shared only to demonstrate my own technical work and should not be used by other students as a submission or solution.
