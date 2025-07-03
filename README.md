# Percentage-of-Reached-Company — README

## What this repo does
This repository tackles a single business question:

> **“What fraction of the client’s companies have we already contacted?”**

The Jupyter notebook **`Percentage of Reached Company.ipynb`** contains the full analysis.  
It ingests two data sources:

| File | Rows × Cols | Key columns |
|------|-------------|-------------|
| **`Total_df1`** (customer list) | 16 502 × 6 | `Com Name`, `Category 1`, … |
| **`merge_df`** (our outreach)   | 3 943 × 3  | `Company Name`, `Job Opening`, `LinkedIn Url` |

### Analysis steps
1. **Clean & normalise text**  
   * remove suffixes such as *Inc, Ltd, Corp*  
   * collapse punctuation / whitespace  
2. **Detect mis-filed job-titles** in the customer’s `Com Name` field  
   *≈ 75 % are job titles, 25 % are true companies*  
3. **Run tiered fuzzy matching** with *rapidfuzz*  
   * four similarity scorers; best score retained  
   * thresholds ≥ 70 %, 85 %, 95 % for low / medium / high confidence  
4. **Calculate penetration**  
   * % of client companies matched at each confidence tier  
   * optional cross-check on job-title alignment  
5. **Highlight next actions** — e.g. fixing swapped columns, lowering thresholds, rerunning after canonicalisation.

## Quick results

| Confidence tier | Match rule | % of client companies already reached |
|-----------------|-----------|----------------------------------------|
| **Strict**      | Name ≥ 85 % & Title ≥ 75 % | *x %* |
| **Very strict** | Name ≥ 95 % & Title ≥ 90 % | *y %* |
| **All matches** | Name ≥ 70 %               | *z %* |

*Replace **x/y/z** with live figures after running the notebook on fresh data.*

## How to reproduce
```bash
# 1  Create an environment
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt    # rapidfuzz, pandas, notebook …

# 2  Place the raw CSVs
data/
 ├─ Total_df1.csv
 └─ merge_df.csv

# 3  Launch the notebook
jupyter notebook "Percentage of Reached Company.ipynb"
