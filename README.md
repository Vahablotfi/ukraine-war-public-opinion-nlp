# Public Opinion on the Ukraine War (2022–2025)

## A Reddit-Based NLP Analysis

## Overview

This project explores how online public discourse surrounding the Ukraine War evolved between 2022 and 2025, using Reddit comments as a proxy for public opinion. The motivation for this analysis came from a personal observation: while early reactions to the invasion were marked by strong unity and support for Ukraine, later discussions—especially in the United States—appeared more divided and politically charged.

The core goal of the project is to examine whether shifts in U.S. political messaging were reflected in everyday online discussions. To do this, I applied Natural Language Processing (NLP) techniques to large-scale Reddit data, focusing on sentiment, dominant discussion topics, and explicit stances toward ending the war versus continuing military support.

Rather than treating the war as a single continuous period, the analysis is structured around key military and political events to better capture short-term shifts in discourse.

---

## Key Events Analyzed

The analysis focuses on five anchor events that represent major turning points in the war and its public framing:

1. **Battle for Kyiv** (Feb–Mar 2022)  
2. **Kherson Liberation** (Nov 2022)  
3. **2023 Stalemate / War Fatigue Narrative**  
4. **U.S. Presidential Election** (Nov 2024)  
5. **Trump–Zelenskyy White House Meeting** (Feb 2025)

---

## Data

Due to access and policy limitations with Reddit’s official API, historical data was sourced from a large Reddit dataset available on Kaggle.

🔗 **Kaggle Dataset:**  
[Public Opinion Russia Ukraine War](https://www.kaggle.com/datasets/asaniczka/public-opinion-russia-ukraine-war-updated-daily?resource=download)

The dataset contains several gigabytes of Reddit comments and metadata collected via Reddit’s official API. Due to size constraints, the raw data is not included in this repository and must be downloaded separately from Kaggle.

- **Platform:** Reddit  
- **Dataset size:** ~5 GB of comments  
- **Time span:** 2014–2025 (analysis focused on 2022–2025)  
- **Data type:** Reddit comments with timestamps, subreddit names, and post-level metadata  

Raw data is stored locally in `data/raw/` and excluded from version control due to size constraints. All derived datasets used in analysis are saved in `data/processed/`.

---

## Methods

The project follows an event-based NLP pipeline that includes:

- Event-specific data extraction using predefined time windows  
- Manual classification of **U.S.-focused vs non-U.S.-focused subreddits**  
- **Sentiment analysis** using VADER  
- **Topic modeling** using Latent Dirichlet Allocation (LDA)  
- **Stance analysis** distinguishing:
  - support for ending the war through negotiations
  - support for continued fighting or military aid  
- Conditional aggregation strategies to address data imbalance  
- Comparative visualizations across events and subreddit types  

The full methodology, results, and interpretation are documented in detail in **REPORT.md**.

---

## Repository Structure

.
├── README.md
├── REPORT.md
├── requirements.txt
├── data/
│ ├── raw/ # raw data (not tracked)
│ └── processed/ # processed datasets used in analysis
└── notebooks/
├── 00_data_audit.ipynb
├── 01_event_extraction.ipynb
├── 02_sentiment_analysis.ipynb
├── 03_topic_modeling.ipynb
├── 04_topic_labeling.ipynb
├── 05_visualization.ipynb
└── 06_stance_analysis.ipynb

---

## Results at a Glance

Some high-level findings from the analysis include:

- Sentiment toward the war became more negative over time, especially during U.S. political milestones.  
- Discussion topics shifted from battlefield developments to domestic political debates and aid considerations.  
- U.S.-focused subreddits showed stronger sentiment and stance shifts than non-U.S.-focused subreddits.  
- Conditional stance analysis revealed growing support for ending the war in later events, particularly within U.S.-centered discussions.

These patterns suggest that as the war became prolonged, public discourse increasingly reflected domestic political concerns rather than purely military developments.

---

## Full Report

📄 **For detailed methodology, figures, and interpretation, see:**  
👉 [`REPORT.md`](REPORT.md)

---

## Notes

This project was completed as part of the **MIT Emerging Talent Certificate in Computer and Data Science**. The analysis reflects online discourse on Reddit and does not claim to represent public opinion in a survey-based sense. Findings should be interpreted as indicative of how narratives and sentiment evolved within online communities over time.
