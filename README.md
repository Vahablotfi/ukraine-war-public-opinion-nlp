# Ukraine War Public Opinion (NLP Project)

This repository contains my Experiential Learning Opportunity 2 (ELO2) project for the MIT Emerging Talent Certificate in Computer and Data Science.

---

## Project Summary

This project analyzes how public opinion in the United States shifted across key events of the Ukraine war (2022–2025), using Natural Language Processing (NLP) techniques on Reddit and Twitter data. The focus is on:

- **sentiment** (support vs opposition),
- **topic changes over time**,
- **polarization** between left-leaning and right-leaning online communities.

---

## Key Events Analyzed

The analysis is centered on five anchor events:

1. **Battle for Kyiv** (Feb–Mar 2022) – beginning of the full invasion and initial surge of support.  
2. **Kherson Liberation** (Nov 2022) – a major Ukrainian victory and emotional high point.  
3. **2023 War Fatigue / Stalemate Narrative** – period of declining attention and growing division in U.S. opinion.  
4. **Trump Election Victory** (Nov 5, 2024) – a political turning point affecting U.S. attitudes toward the war.  
5. **Trump–Zelenskyy White House Meeting** (Feb 28, 2025) – a diplomatic confrontation that reignited public debate.

---

## Repository Structure

Planned structure of this repository:

├─ README.md # High-level overview of the project
├─ REPORT.md # Detailed markdown report
├─ requirements.txt # Python dependencies
│
├─ data/
│ ├─ raw/ # Raw data files (not tracked or with samples)
│ └─ processed/ # Cleaned / processed data
│
├─ notebooks/
│ ├─ 01_exploration.ipynb # Initial exploration & sanity checks
│ ├─ 02_sentiment.ipynb # Sentiment analysis
│ └─ 03_topics.ipynb # Topic modeling & keyword analysis
│
├─ src/
│ ├─ preprocessing.py # Text cleaning functions
│ ├─ sentiment.py # Sentiment utilities
│ └─ topics.py # Topic modeling utilities
│
└─ app/
└─ streamlit_app.py # Streamlit dashboard app

---

## Methods (Planned)

- **Data collection** from Reddit and (if possible) Twitter for each event and time window.  
- **Text preprocessing**, including:
  - tokenization  
  - lowercasing  
  - stopword removal  
  - basic normalization  

- **Sentiment analysis** using VADER, with comparisons:
  - before vs after each event  
  - left-leaning vs right-leaning communities  

- **Topic modeling** (e.g., LDA) to identify dominant themes around each event.  

- **Visualization** of:
  - sentiment over time  
  - topic distributions  
  - polarization (sentiment gap) between groups  

---

## Streamlit Dashboard

A Streamlit app will be built to allow interactive exploration of:

- selected event (Kyiv, Kherson, stalemate, Trump election, White House meeting)  
- sentiment timelines  
- topic summaries  
- example comments  

The dashboard link will be added here once deployed.

---
