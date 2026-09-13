# Student Feedback Analysis System

An end-to-end analysis of student feedback collected across Ardent Software Services' training programs, internships, and workshops — identifying a prioritized, evidence-backed scope of improvement in training delivery.

Built as part of the Intern Development Team project under the Directorate of Technology Services, Ardent Computech Pvt. Ltd.

## Project Overview

Ardent collects student feedback at the end of its programs but previously had no centralized way to analyze it across cohorts. This project consolidates that feedback and applies statistical analysis, classical machine learning, and text mining to answer one question: **which aspects of the student experience most need attention, and how confident can we be in that conclusion?**

## What This Project Does

- Cleans and consolidates raw feedback exports into a single structured dataset
- Computes descriptive statistics and correlations across five feedback dimensions
- Trains a classical ML model to identify which factors best predict low satisfaction
- Segments students into feedback profiles using k-means clustering
- Runs sentiment analysis and keyword extraction on open-ended feedback
- Produces a ranked "Scope of Improvement" list with supporting evidence, affected segments, and priority levels

Only classical, interpretable methods are used throughout (regression, clustering, TF-IDF, lexicon-based sentiment) — no deep learning or generative/LLM-based approaches, per project scope.

## Folder Structure

```
Student_Feedback_Analysis_System/
├── data/
│   ├── feedback_raw.xlsx         
│   └── feedback_cleaned.csv      
├── notebooks/
│   ├── 01_data_cleaning.ipynb             
│   ├── 02_eda_statistics.ipynb           
│   ├── 03_ml_modeling_clustering.ipynb    
│   └── 04_sentiment_text_analysis.ipynb  
├── outputs/
│   └── figures/                  
├── report/
│   ├── Scope_of_Improvement_Report.docx
├── .gitignore
└── README.md
```

## Data

Each record represents one student's feedback and includes: Respondent ID, Program/Domain, Batch/Cohort, Institution, Delivery Mode, Gender, Academic Year/Status, five 1–5 rating dimensions (Trainer Effectiveness, Curriculum Relevance, Pace of Delivery, Support & Mentorship, Platform/Infrastructure), Overall Satisfaction, Would Recommend (Yes/No), and Open Feedback (free text).

The raw data file is excluded from version control (see `.gitignore`); only the cleaned dataset is tracked, since it's the reproducible starting point for all analysis.

## How to Run

1. **Install dependencies**
   ```bash
   pip install pandas numpy scikit-learn scipy matplotlib seaborn nltk openpyxl
   ```

2. **Download required NLTK resources** (one-time, needed for Notebook 04)
   ```python
   import nltk
   nltk.download('stopwords')
   nltk.download('punkt')
   nltk.download('punkt_tab')
   nltk.download('vader_lexicon')
   ```

3. **Run the notebooks in order** — each phase builds on the cleaned output of the previous one:
   ```
   01_data_cleaning.ipynb            →  produces data/feedback_cleaned.csv
   02_eda_statistics.ipynb           →  descriptive stats, correlation, visualizations
   03_ml_modeling_clustering.ipynb   →  regression model, k-means clusters
   04_sentiment_text_analysis.ipynb  →  sentiment scores, keyword extraction
   ```
   All notebooks load `data/feedback_cleaned.csv` directly — run Notebook 01 first if this file doesn't exist yet.

## Methodology Summary

| Phase | What Happens |
|---|---|
| 1. Data Cleaning | Missing-value imputation (median for ratings), categorical normalization, duplicate and range validation |
| 2. Statistical Profiling | Descriptive statistics, segmentation by Program/Batch/Institution, correlation analysis |
| 3. ML Modeling | Logistic regression to predict low satisfaction; k-means clustering into feedback profiles |
| 4. Text Mining | Lexicon-based sentiment scoring (VADER) and TF-IDF keyword extraction on open feedback |

## Key Findings

- **Pace of Delivery** and **Platform/Infrastructure** are consistently flagged as the weakest, most inconsistent, and most complained-about dimensions — confirmed independently by descriptive statistics, the predictive model, and text mining (132 comments explicitly mention feeling "rushed").
- **Trainer Effectiveness** and **Support & Mentorship** are the organization's clearest strengths, and the strongest statistical drivers of overall satisfaction — these should be protected, not deprioritized.
- Four distinct student feedback profiles were identified via clustering, ranging from "Highly Satisfied" to "Dissatisfied," with the latter needing broad, cross-dimensional support rather than a single fix.
- Full findings, evidence, and the ranked Scope-of-Improvement list are in `reports/Scope_of_Improvement_Report.docx`.

## Assumptions & Limitations

- Missing rating values (under 2% per dimension) were imputed using the median, appropriate for ordinal 1–5 scale data.
- Records with no open-ended feedback were treated as blank and excluded from text analysis, not fabricated.
- "Low satisfaction" for predictive modeling was defined as an Overall Satisfaction score of 3 or below — a judgment call made to yield a reasonably balanced classification target, not a fixed organizational standard.
- Segment-level findings (by Program/Batch/Institution) are based on varying sample sizes; findings from smaller segments are noted as lower-confidence in the report.
- Sentiment analysis uses a lexicon-based method (VADER), which was found to under-detect politely phrased criticism — reported Negative-sentiment figures should be read as a conservative lower bound. See the report's limitations section for details.

## Team

Built by the Intern Development Team, Ardent Software Services, under the Directorate of Technology Services.
