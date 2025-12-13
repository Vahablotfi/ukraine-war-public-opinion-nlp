# Public Opinion Shifts During the Ukraine War (2022–2025)

## NLP Analysis of U.S. Online Discussions Around Five Key Events

---

## 1. Introduction

Since the start of the Ukraine War in February 2022, public opinion in the United States has shifted in noticeable ways. In the early weeks, there was a strong sense of solidarity with Ukraine across social media, major news outlets, and political speeches. As the war continued, casualties increased, and U.S. politics moved into new phases, the tone from both political leaders and everyday users online appeared to change.

The motivation for this project came from my own curiosity. I remember the unity and support shown for Ukraine at the beginning of the war, and I wanted to understand how much these attitudes evolved over time. I also wondered whether changes in public sentiment followed the same pattern as political messaging in the U.S., especially once politicians began discussing cutting support or pushing for different strategies to end the conflict. This project aims to see whether similar shifts happened among regular social media users.

To explore this, the project examines **how U.S. public opinion shifted across five major events**:

1. **Battle for Kyiv (Feb–Mar 2022)**  
2. **Kherson Liberation (Nov 2022)**  
3. **2023 Stalemate / War Fatigue Narrative**  
4. **Trump Election Victory (Nov 2024)**  
5. **Trump–Zelenskyy White House Meeting (Feb 2025)**  

Each of these events marks an important point in the military, political, or humanitarian timeline of the war. By analyzing Reddit posts from U.S.-based communities during these periods, this project aims to measure:

- changes in sentiment toward Ukraine  
- shifts in discussion topics  
- signs of polarization in conversations about the war  
- how much public opinion aligned or diverged from political messaging  

Overall, this work helps build a clearer understanding of how a long-lasting international conflict influences public attitudes in the United States, especially during a time of political division.

---

## 2. Data Sources

This project combines several types of data to study public opinion across the five events. Each source plays a different role in helping identify relevant posts and understanding how discussions changed over time.

### 2.1 Social Media Platforms

#### **Reddit**

Reddit was used as the main data source for public discussions because:

- comments are longer and more expressive compared to other platforms  
- different subreddits have clear political or thematic orientations  
- data is more accessible for academic projects  
- many users openly share opinions, emotions, and reactions to news events  

Data was collected from subreddits such as r/politics, r/worldnews, r/conservative, and r/UkrainianConflict.

#### **Twitter/X (optional)**

Twitter/X was considered for additional data, but full API access is limited. If time allows, supplemental tweets may be added later.

---

### 2.2 Event Windows

To capture before-and-after sentiment changes, each event includes a pre-event and post-event window:

- **Event 1: Battle for Kyiv** — Feb 20 to Mar 20, 2022  
- **Event 2: Kherson Liberation** — Oct 25 to Nov 25, 2022  
- **Event 3: 2023 Stalemate / War Fatigue** — May 1 to Aug 31, 2023  
- **Event 4: Trump Election Victory** — Oct 20 to Nov 20, 2024  
- **Event 5: Trump–Zelenskyy White House Meeting** — Feb 15 to Mar 15, 2025  

These windows were chosen to:

- capture peak conversation periods  
- avoid unrelated long-term drift  
- ensure there is enough Reddit activity to analyze  

---

### 2.3 External Reference Sources

To understand how each event was described and framed by media and public interest, several outside sources were used:

#### **News Headlines**

Headlines from BBC, Reuters, and AP News were collected for each event. These helped identify:

- common military terms  
- humanitarian impact language  
- political framing  
- key people, places, and actions  

#### **Google Trends**

Google Trends provided search topics and queries that were trending during each event window. These results helped identify:

- what people were searching for  
- public attention spikes  
- emerging keywords like “Ghost of Kyiv,” “sanctions,” or “Kherson”  

#### **Wikipedia Event Summaries**

Wikipedia summaries were used to extract consistent factual vocabulary such as:

- city names  
- battle names  
- key dates  
- political actors  

This helped avoid missing important proper nouns.

---

### 2.4 Keyword Dataset

The final keyword dataset is stored at:

`data/processed/ukraine_project_keywords_events1to5.csv`

This dataset is used to filter Reddit posts for each event and is a core part of the analysis pipeline.

---

## 3. Methods

### 3.1 Data Collection

Reddit posts for each event were collected using:

- **Pushshift API**, which provides historical Reddit data archives  
- **Keyword matching**, based on the event-specific keyword lists  
- **Subreddit filters**, focusing on political or global news communities such as:  
  - r/politics  
  - r/worldnews  
  - r/conservative  
  - r/UkrainianConflict  

For each event, the data includes:

- a **pre-event window** (7–10 days before the event)  
- a **post-event window** (7–10 days after the event)  

This structure allows us to compare how sentiment and discussion topics changed around each key moment.

---

### 3.2 Keyword Extraction Methodology

A multi-step approach was used to build the keyword lists for the five events.

#### **Sources Used**

- News headlines from BBC, Reuters, AP News  
- Google Trends search topics and queries  
- Wikipedia summaries  
- Political vocabulary from U.S. media  
- Common Ukraine War hashtags  

Using multiple sources helps ensure that keywords:

- capture different types of discussions (military, humanitarian, political)  
- reflect real public search behavior  
- match the language used on Reddit and other platforms  

---

### Keyword Categorization

Keywords were organized into categories to make filtering and interpretation easier.

#### **Proper Nouns**
Cities, leaders, and organizations (e.g., *Kyiv*, *Kherson*, *Zelensky*, *Putin*, *NATO*).  
These terms help anchor discussions to specific places or people.

#### **Conflict / Military Vocabulary**
Words related to combat or battlefield developments  
(e.g., *airstrike*, *shelling*, *retreat*, *counteroffensive*).  
These help identify posts discussing military events.

#### **Humanitarian / Emotional Terms**
Words describing civilian impact or emotional reactions  
(e.g., *refugees*, *death toll*, *evacuees*, *fleeing*).  
These terms support sentiment analysis.

#### **Geopolitical / Policy Terms**
Vocabulary connected to diplomacy, sanctions, foreign aid, or political decisions  
(e.g., *sanctions*, *aid freeze*, *U.S. support*, *NATO burden*).  
These terms capture politically framed discussions.

#### **Political Discourse Keywords**
Especially relevant for events involving U.S. elections or shifts in political messaging  
(e.g., *“stop sending money,” “America First,” “peace deal”*).  
These help analyze partisan trends.

---

### 3.3 Preprocessing

Before running NLP tasks, all Reddit comments were cleaned using:

- lowercasing  
- removing punctuation, URLs, usernames, and special characters  
- tokenization  
- stopword removal  
- lemmatization  
- filtering out non-English comments  

Other filters included:

- removing duplicates  
- removing very short comments  
- removing likely bot content  

This created a clean dataset ready for sentiment and topic modeling.

---

### 3.4 Sentiment Analysis

Sentiment analysis was performed using:

- **VADER**, a sentiment tool tailored for social media text  

Sentiment scores were aggregated at:

- the **post level**  
- the **subreddit level**  
- the **event window level**  

Comparisons focus on:

- **before vs. after each event**  
- **left-leaning vs. right-leaning subreddits**  

This helps identify shifts in support, criticism, or polarization.

---

### 3.5 Topic Modeling

Topic modeling used:

- **Latent Dirichlet Allocation (LDA)** for interpretable themes  
- event-specific corpora to compare topic differences across time  

Future improvements might include:

- **BERTopic**  
- **transformer-based clustering**  

Topic modeling highlights how themes of discussion shifted even when sentiment did not.

---

## (Sections 4–7 will be added once results are produced.)