# Public Opinion on the Ukraine War: A Reddit-Based NLP Analysis (2022–2025)

## 1. Introduction

Since the start of the war in Ukraine in February 2022, public opinion in the United States has gone through noticeable shifts. Early reactions were marked by strong solidarity with Ukraine, widespread emotional responses, and bipartisan political support. Over time, however, the war became prolonged, casualties mounted, and U.S. domestic politics increasingly shaped how the conflict was discussed—especially as debates around military aid, NATO involvement, and peace negotiations became more polarized.

This project began with a personal curiosity about whether these political shifts were reflected in everyday public discourse. I remembered the strong unity during the first weeks of the invasion and wondered how closely public sentiment followed changes in U.S. political messaging, particularly during later events such as the 2024 election and the 2025 White House meeting involving Ukraine.

The core goal of this project is to examine **how public opinion about the Ukraine War evolved over time**, using Reddit comments as a proxy for online public discourse. The analysis focuses on five key events that represent military, political, or narrative turning points:

- **Battle for Kyiv** (Feb–Mar 2022)
- **Kherson Liberation** (Nov 2022)
- **2023 Stalemate / War Fatigue Narrative**
- **U.S. Presidential Election** (Nov 2024)
- **Trump–Zelenskyy White House Meeting** (Feb 2025)

Using natural language processing (NLP) techniques, the project examines changes in:

- sentiment toward the war,
- dominant discussion topics,
- polarization between U.S.-focused and non-U.S.-focused subreddits,
- and stances toward ending the war versus continuing military support.

---

## 2. Data Sources and Constraints

### 2.1 Reddit API Limitations

The original plan for this project was to collect Reddit data directly using Reddit’s official API. However, access limitations, approval delays, and policy restrictions made sustained, large-scale data collection impractical within the project timeline. As a result, relying exclusively on live API scraping was not a feasible option for this analysis.

### 2.2 Kaggle Reddit Dataset

As an alternative, a large Reddit dataset related to the Ukraine War was sourced from Kaggle. This dataset contained approximately **5 GB of Reddit comments**, spanning from **2014 to mid-2025**, with particularly dense coverage between **2022 and 2025**.

**Key advantages of this dataset include:**

- Large volume of comments  
- Rich metadata (timestamps, subreddit names, and post-level context)  
- Strong coverage for later-stage war events, especially 2024–2025  

**Key limitations include:**

- Uneven coverage across events  
- Sparse U.S.-focused discussion for early events (2022–2023), limiting direct comparisons for those periods  

The raw dataset was stored locally in `data/raw/` and intentionally excluded from version control due to its size.

---

## 3. Data Audit and Event Coverage

### 3.1 Temporal Coverage

A full audit of the raw dataset was conducted using **Notebook `00_data_audit.ipynb`** to verify temporal coverage. The dataset spans the following time range:

- **Earliest timestamp:** November 2014  
- **Latest timestamp:** July 2025  

This confirmed that all five target events fall within the available data window and can be analyzed using a consistent source.

### 3.2 Comment Volume by Event

After defining event-specific time windows around each key event, the following comment volumes were observed:

| Event | Approx. Number of Comments |
| ------ | ---------------------------- |
| Battle for Kyiv | ~2,940 |
| Kherson Liberation | ~795 |
| Stalemate / War Fatigue | ~9,955 |
| Trump Election | ~359,000 |
| White House Meeting | ~241,000 |

A strong imbalance in comment volume is evident, particularly between early war events (2022–2023) and later political events (2024–2025). This imbalance represents a key analytical constraint and is explicitly accounted for in later stages of the analysis through conditional metrics, normalization strategies, and cautious interpretation.

---

## 4. Event Extraction and Subreddit Classification

### 4.1 Event-Based Extraction

In **Notebook `01_event_extraction.ipynb`**, Reddit comments were filtered into five event-specific datasets using predefined time windows around each key event. These windows were designed to capture discussion immediately before and after each event in order to observe short-term opinion shifts.

Each event dataset was saved as a separate CSV file in the `data/processed/` directory, forming the foundation for subsequent sentiment, topic, and stance analyses.

### 4.2 U.S.-Focused vs Non-U.S.-Focused Subreddits

To better isolate U.S. public opinion, subreddits were manually classified into two broad categories:

- **U.S.-focused subreddits** (e.g., `politics`, `neoliberal`, `Conservative`)
- **Non-U.S.-focused or global subreddits** (e.g., `worldnews`, `europe`, `ukraine`)

A binary indicator variable, **`is_us_focused`**, was added to each comment to enable comparative analysis between these two groups.

An important limitation emerged at this stage: **early war events (Events 1–3)** contained little to no U.S.-focused subreddit activity. This constraint is explicitly acknowledged and considered in later interpretation, particularly when comparing early wartime sentiment to later, more politically driven discussions.

---

## 5. Sentiment Analysis

### 5.1 Method

Sentiment analysis was conducted using **VADER (Valence Aware Dictionary and sEntiment Reasoner)**, a rule-based sentiment model specifically designed for short, informal text such as social media comments. VADER produces a **compound sentiment score** for each comment, ranging from **−1 (very negative)** to **+1 (very positive)**.

Each Reddit comment in the event-specific datasets was assigned a compound sentiment score, forming the basis for all subsequent sentiment aggregation and comparison.

### 5.2 Aggregation Strategy

To capture sentiment trends at different levels of analysis, sentiment scores were aggregated in several ways:

- **Subreddit-level means**, to observe how sentiment varied across communities  
- **Event-level means**, to summarize overall sentiment around each key event  
- **Weighted averages**, where subreddit-level sentiment was weighted by the number of comments contributed by each subreddit  

In addition, a **net sentiment score** was computed to capture the overall emotional direction of discussion for each event, balancing positive and negative contributions rather than relying on raw averages alone.

This multi-level aggregation approach helps reduce the influence of small or highly active subreddits and provides a more stable representation of broader discourse patterns.

### 5.3 Key Observations

Several high-level patterns emerged from the sentiment analysis:

- **Early war events (Events 1–3)** showed largely neutral to mildly negative sentiment, dominated by non-U.S.-focused subreddits.  
- **Later events (Events 4–5)** exhibited stronger negative sentiment, particularly around major U.S. political milestones.  
- **U.S.-focused subreddits** demonstrated sharper sentiment shifts compared to global subreddits, suggesting a stronger reaction to domestic political framing than to battlefield developments alone.  

These results suggest that sentiment surrounding the Ukraine War became increasingly shaped by U.S. political context over time.

---

## 6. Topic Modeling

### 6.1 Method

To identify dominant discussion themes surrounding each event, **Latent Dirichlet Allocation (LDA)** topic modeling was applied **separately for each event-specific dataset** (Notebook `03_topic_modeling.ipynb`). Modeling events independently avoided topic dilution caused by large differences in comment volume across events.

Several preprocessing steps were applied prior to modeling:

- **Custom text cleaning**, including lowercasing, removal of URLs, punctuation, and stopwords  
- **Light English-language filtering**, to reduce non-English content that introduced noise in earlier iterations  
- **Adaptive document-frequency thresholds**, where smaller events used lower minimum document-frequency values to ensure sufficient vocabulary coverage  

For each event, five topics were extracted, with each topic represented by its most probable terms.

### 6.2 Results

Topic modeling revealed **clear narrative and thematic shifts over time**:

- **Early war events (2022)** were dominated by discussions of military actions, border crossings, humanitarian aid, and NATO expansion.  
- **Mid-war discourse (2023)** shifted toward narratives of stalemate, war fatigue, the Wagner Group, internal Russian dynamics, and concerns about corruption or effectiveness.  
- **Late-stage events (2024–2025)** increasingly focused on U.S. politics, elections, military aid debates, peace negotiations, burden-sharing with Europe, and the strategic costs of continued support.  

To improve interpretability and ensure consistency across events, topics were **manually labeled** based on their dominant terms in `04_topic_labeling.ipynb`. These labels were later used in visualization and interpretation, allowing for clearer comparison of discourse evolution across time.

---

## 7. Stance Analysis: Ending the War vs Continuing Support

While sentiment analysis captures emotional tone, it does not directly indicate *policy preference*. A comment can be negative in tone while still supporting continued military aid, or positive while advocating for negotiations. To address this limitation, a dedicated stance analysis was conducted to distinguish between calls to **end the war through negotiations** and support for **continued fighting or military aid to Ukraine**.

### 7.1 Motivation

The central research question of this project is whether public opinion shifted in parallel with U.S. political discourse. In recent years, political debate increasingly framed the war in terms of *ending the conflict* versus *continuing support until Ukraine regains territory*. Stance analysis was introduced to directly measure this distinction in public discourse, rather than inferring it indirectly from sentiment alone.

### 7.2 Stance Lexicon Construction

Rather than manually inventing stance keywords, candidate phrases were extracted directly from the dataset using frequent n-grams observed across events. These phrases were then manually reviewed and categorized into three stance groups:

- **End-war stance** (e.g., “end war”, “peace deal”, “peace talks”, “negotiated settlement”)
- **Continue-fighting stance** (e.g., “support Ukraine”, “military aid”, “send weapons”, “join NATO”)
- **Neutral or unclear** (comments without explicit stance indicators)

This data-driven approach ensured that stance detection reflected *language actually used by Reddit users*, rather than externally imposed terminology.

### 7.3 Comment-Level Classification

Each comment was assigned a stance label based on the presence of stance-related phrases. Comments without explicit stance language were labeled as **neutral_or_unclear**. Because stance expressions are relatively rare compared to general discussion, the majority of comments fell into the neutral category.

### 7.4 Conditional Stance Aggregation

To avoid dilution by neutral comments, stance proportions were calculated **conditionally**, using only comments that expressed a clear stance. For each event, the following metric was computed:

- Percentage of end-war vs continue-fighting stances *among stance-bearing comments only*

### 7.5 Event-Level Results

Across events, a clear shift emerged:

- Early events (2022–2023) showed limited explicit stance expression, reflecting uncertainty and informational discussion.
- By late events (2024–2025), stance-bearing comments increased substantially.
- The **end-war stance grew more prevalent over time**, particularly during U.S. political milestones such as the 2024 election and the 2025 White House meeting.

This pattern aligns with broader political discourse emphasizing negotiation, burden-sharing, and conflict fatigue.

### 7.6 U.S.-Focused vs Non-U.S.-Focused Comparison

Stance analysis was further split by subreddit classification using the `is_us_focused` variable:

- For early events, U.S.-focused stance data was largely absent, limiting direct comparison.
- For later events, **U.S.-focused subreddits consistently showed a higher proportion of end-war stances** compared to non-U.S. subreddits.
- Non-U.S. subreddits retained relatively stronger support for continued fighting, though the gap narrowed over time.

This divergence suggests that U.S. domestic political considerations increasingly shaped public discourse, especially as the war intersected with electoral politics.

### 7.7 Stance Index

To summarize stance directionality, a stance index was computed as:

stance_index = pct_continue_fighting − pct_end_war

Negative values indicate dominance of end-war stances, while positive values indicate support for continued fighting. The stance index became increasingly negative in later events, reinforcing evidence of growing war-fatigue and negotiation-oriented discourse.

### 7.8 Limitations

Several limitations apply to this analysis:

- Stance detection relies on explicit phrasing and likely underestimates implicit positions.
- Early events suffer from sparse U.S.-focused data.
- Reddit users are not representative of the general population.

Despite these constraints, stance analysis provides a valuable complementary perspective to sentiment and topic modeling, enabling a more direct examination of policy-relevant opinion shifts.

---

## 8. Visualization and Comparative Analysis

Visualizations play an important role in this project by turning large amounts of text data into patterns that are easier to understand and compare. Rather than presenting figures as isolated results, each visualization was designed to support comparisons across events, sentiment, subreddit type, and stance.

### 8.1 Event-Level Sentiment Trends

Event-level sentiment plots summarize the average emotional tone of Reddit discussions around each key event.

Two broad patterns stand out:

- **Early events (2022–2023)** show sentiment values close to neutral. This suggests that discussions during the early stages of the war were often informational or uncertain, rather than strongly emotional.
- **Later events (2024–2025)** display noticeably more negative sentiment, especially around U.S. political milestones such as the presidential election and the White House meeting.

This shift supports the idea that as the war dragged on and became more connected to U.S. domestic politics, emotional reactions intensified.

### 8.2 Why Net Sentiment Is Useful

In addition to average sentiment scores, the analysis includes a **net sentiment metric** that captures the overall balance between positive and negative comments.

Net sentiment is helpful because:

- It smooths out noisy fluctuations from mixed reactions.
- It makes it easier to see the *direction* of public mood rather than small score changes.
- It allows clearer comparisons between events with very different numbers of comments.

The net sentiment plot shows a clear downward trend over time, reinforcing the observation that discussions became more critical and pessimistic as the conflict continued.

### 8.3 U.S.-Focused vs Non-U.S.-Focused Sentiment

To better isolate U.S. public opinion, sentiment results were split using the `is_us_focused` variable introduced during event extraction.

Several patterns emerge:

- Early events have **very limited U.S.-focused participation**, which restricts direct comparison for 2022–2023.
- In later events, U.S.-focused subreddits show **stronger sentiment shifts** than non-U.S. subreddits.
- The largest divergence appears during the 2024 election and the 2025 White House meeting.

This suggests that U.S. domestic political context played a significant role in shaping emotional responses to the war.

### 8.4 Conditional Stance Visualizations

Stance visualizations focus on **conditional percentages**, meaning that only comments explicitly expressing a stance are included.

This approach is important because:

- Most Reddit comments are neutral or informational.
- Including neutral comments would hide meaningful differences between opposing positions.

By conditioning on stance-bearing comments, the plots reveal:

- A steady increase in **end-war stances** over time.
- A declining share of **continue-fighting stances**, especially in later events tied to U.S. politics.

These trends are difficult to see using sentiment analysis alone and highlight the value of stance-based analysis.

### 8.5 Comparing U.S. and Non-U.S. Stance Patterns

Side-by-side comparisons between U.S.-focused and non-U.S.-focused subreddits show clear structural differences:

- U.S.-focused subreddits consistently display a higher share of end-war stances in 2024–2025.
- Non-U.S.-focused subreddits remain more supportive of continued fighting, although the gap narrows over time.

This comparison supports the interpretation that U.S. public discourse increasingly reflected domestic political debates about aid, elections, and negotiation strategies.

### 8.6 Why Visualization Matters in This Project

Overall, visualization is essential for making sense of the results in this project. It helps:

- Address strong data imbalance across events,
- Compare discussions across different subreddit types,
- Highlight stance shifts that are not obvious from sentiment scores alone.

Rather than making causal claims, the visual results provide clear and interpretable evidence of how online public discourse evolved alongside major political and military developments over time.

---

## 9. Discussion and Interpretation

The combined results of this project show how public discourse surrounding the Ukraine War evolved over time, and how shifts in U.S. political messaging were reflected in online discussions. By integrating sentiment analysis, topic modeling, and stance analysis across five key events, the analysis reveals consistent patterns in how the war was framed, debated, and emotionally processed on Reddit.

### 9.1 Evolution of Public Sentiment Over Time

One of the clearest findings is that public sentiment became more negative as the war progressed. Early discussions during the Battle for Kyiv were relatively neutral, with many comments focused on information sharing, uncertainty, and immediate reactions to the invasion. As the conflict extended into later years, sentiment scores declined, especially around politically charged moments such as the 2024 U.S. election and the 2025 White House meeting.

This trend suggests that prolonged conflict, rising costs, and growing political debate may have contributed to increasing frustration and fatigue in public discourse. Rather than reflecting a single turning point, sentiment appears to shift gradually as the war becomes more embedded in domestic political narratives.

### 9.2 Topic Shifts Reflect Changing Narratives

Topic modeling reinforces this temporal shift in framing. Early topics focused heavily on military developments, borders, NATO expansion, and immediate humanitarian concerns. During the stalemate period, discussions increasingly centered on Wagner Group activity, internal Russian dynamics, and perceptions of momentum or lack thereof.

By late 2024 and early 2025, topics shifted decisively toward political and economic themes: elections, aid debates, burden-sharing, peace negotiations, and U.S. leadership. This progression highlights how the war transitioned from an external military crisis to a recurring issue in U.S. domestic politics.

### 9.3 U.S.-Focused vs Non-U.S.-Focused Discourse

A key objective of this project was to distinguish U.S.-focused public opinion from broader global discussions. The analysis shows that early events lacked meaningful U.S.-focused participation, limiting conclusions about American public opinion in 2022–2023.

However, once U.S.-focused subreddits became active in later events, clear differences emerged. U.S.-focused discussions showed stronger sentiment shifts and a higher prevalence of end-war stances compared to non-U.S.-focused subreddits. This suggests that U.S. public discourse may be more sensitive to domestic political incentives, election cycles, and economic considerations than global discussions.

### 9.4 Interpreting Stance Toward Ending the War

Stance analysis adds an important layer beyond sentiment. While sentiment captures emotional tone, stance analysis directly addresses *what people want to happen*.

Conditional stance results show a steady increase in end-war preferences over time, particularly in U.S.-focused subreddits. This pattern aligns with the growing presence of phrases related to peace deals, negotiations, and aid skepticism in later events.

Importantly, this does not necessarily imply opposition to Ukraine itself. Instead, it suggests increasing public openness to ending the conflict through negotiation, especially as the war becomes prolonged and politically costly.

### 9.5 Alignment Between Political Rhetoric and Public Opinion

Taken together, the results suggest partial alignment between political rhetoric and online public discourse. As U.S. political leaders increasingly debated aid levels and negotiation strategies, similar themes became more visible in Reddit discussions—particularly in U.S.-focused communities.

However, the data also shows that public discourse is not monolithic. Non-U.S.-focused subreddits remained more consistently supportive of continued fighting, highlighting differences in perspective based on geographic and political context.

### 9.6 What This Analysis Does—and Does Not—Claim

This project does not claim that Reddit users represent the general population, nor that observed shifts are causal. Instead, it demonstrates how large-scale online discussions reflect evolving narratives, priorities, and political contexts.

Within those limits, the findings suggest that public opinion—as expressed in online discourse—became more polarized, more political, and increasingly focused on ending the conflict over time, especially within U.S.-centered spaces.

---

## 10. Limitations

While this project provides meaningful insights into how public discourse around the Ukraine War evolved over time, several limitations should be acknowledged.

### 10.1 Data Source Constraints

The analysis relies entirely on Reddit data, which represents a specific subset of online users rather than the general population. Reddit users tend to be younger, more politically engaged, and more active in online discussions than average citizens. As a result, the findings should be interpreted as reflecting *online discourse* rather than public opinion in a strict survey-based sense.

Additionally, the original plan to collect data directly through Reddit’s official API was not feasible due to access limitations and approval constraints. As a result, a large Kaggle dataset was used instead. While this dataset offered broad coverage and rich metadata, it also imposed constraints on sampling control and subreddit selection.

### 10.2 Uneven Event Coverage

A major limitation of the dataset is the uneven distribution of comments across events. Early events (2022–2023) had substantially fewer comments than later events (2024–2025), particularly in U.S.-focused subreddits. This imbalance limited the ability to directly compare early and late events on equal footing.

To address this, the analysis:

- explicitly reports comment counts per event,
- avoids over-interpreting early-event results,
- and uses conditional metrics where appropriate.

### 10.3 Subreddit-Based Geographic Approximation

U.S.-focused versus non-U.S.-focused discourse was inferred based on subreddit classification rather than user location data. While this approach captures broad differences in discussion context, it cannot guarantee that all participants in a given subreddit are based in the United States.

This limitation is particularly relevant for global subreddits such as *worldnews* or *europe*, where discussions may mix perspectives from multiple countries.

### 10.4 Sentiment and Stance Classification Limits

Sentiment analysis using VADER provides a useful high-level signal but cannot fully capture sarcasm, irony, or complex rhetorical strategies common in political discussions. Similarly, stance analysis relies on keyword and phrase matching, which may miss implicit or nuanced positions.

The initial stance analysis produced a high proportion of neutral classifications, highlighting the difficulty of detecting intent in short-form text. Conditional stance analysis helped address this issue but still reflects only explicit stance expressions.

### 10.5 Topic Modeling Interpretability

Topic modeling results depend on modeling choices such as the number of topics, document-frequency thresholds, and text preprocessing steps. While adaptive thresholds and manual topic labeling improved interpretability, topics should be viewed as *patterns of discussion* rather than definitive categories.

Different modeling choices could yield alternative but equally plausible topic structures.

### 10.6 Temporal and Contextual Bias

Finally, public discourse is shaped by external events beyond those explicitly modeled here, including economic conditions, media framing, and unrelated political developments. As a result, observed changes in sentiment or stance cannot be attributed solely to the Ukraine War itself.

---

Overall, these limitations highlight the complexity of studying public opinion through online discourse. Rather than weakening the analysis, acknowledging these constraints provides important context for interpreting the findings responsibly.

---

## 11. Conclusion

This project set out to examine how public discourse around the Ukraine War evolved over time, with a particular focus on whether shifts in U.S. political messaging were reflected in online public opinion. Using Reddit comments as a proxy for public discourse, the analysis combined sentiment analysis, topic modeling, and stance detection across five key events between 2022 and 2025.

Several clear patterns emerged from the analysis.

First, public discussion of the Ukraine War changed substantially as the conflict moved from its early invasion phase into a prolonged and politically contested issue. Early events were dominated by military developments, humanitarian concerns, and uncertainty. Over time, especially during the 2024 U.S. election and the 2025 White House meeting, discussions increasingly centered on political leadership, financial aid, burden-sharing, and potential exit strategies.

Second, sentiment analysis revealed that emotional tone was not static across events. While early discussions were often neutral to slightly negative, later events showed stronger and more polarized sentiment, particularly in U.S.-focused subreddits. Net sentiment scores highlighted that political milestones in the United States coincided with sharper emotional reactions than battlefield developments alone.

Third, topic modeling confirmed that attention shifted from operational and battlefield topics toward domestic political debates in the United States. Late-stage topics emphasized elections, aid spending, NATO commitments, and peace negotiations, reflecting how the war became increasingly embedded in U.S. political discourse rather than being discussed solely as a foreign conflict.

Fourth, stance analysis provided additional insight beyond sentiment alone. Although most comments did not explicitly express a clear stance, conditional analysis of stance-bearing comments showed a growing preference for ending the war over continuing military engagement, particularly during later events. This trend was more pronounced in U.S.-focused subreddits, suggesting a divergence between U.S. domestic discourse and broader global discussions.

Taken together, these findings suggest that prolonged conflicts can undergo a narrative transformation in public discourse. What begins as a largely humanitarian and military issue may gradually become reframed as a domestic political question, shaped by elections, leadership changes, and policy debates. In the case of the Ukraine War, this shift appears especially visible in U.S.-centered online discussions.

While the results should be interpreted with care due to data and methodological limitations, the project demonstrates how NLP techniques applied to large-scale social media data can help track evolving public narratives and identify points where political discourse and public sentiment begin to diverge or converge.

---

## 12. Future Work

This project provides a snapshot of how public discourse around the Ukraine War evolved over time, but several extensions could deepen and refine the analysis.

First, future work could improve stance detection by moving beyond keyword-based methods toward supervised or semi-supervised models. Manually labeled training data or transformer-based classifiers could better capture nuanced positions that are not explicitly expressed through clear stance phrases, reducing the large neutral category observed in this study.

Second, expanding the dataset—particularly for early war events—would allow for more balanced comparisons across time. Additional historical Reddit data or complementary sources such as Twitter/X, news comments, or forum discussions could help address the uneven coverage and strengthen conclusions about early public reactions.

Third, the analysis could be extended to track individual subreddits or user-level trajectories over time. Examining how specific communities changed their tone or stance across multiple events would provide a more granular understanding of polarization and narrative shifts.

Fourth, cross-national comparisons represent a promising direction. Rather than focusing primarily on U.S.-focused versus non-U.S.-focused subreddits, future studies could incorporate language-based or geolocation-based methods to compare discourse across countries more systematically.

Finally, the analytical pipeline developed in this project could be adapted to other prolonged geopolitical conflicts. Applying the same combination of sentiment analysis, topic modeling, and stance detection to different case studies would help assess whether the patterns observed here are unique to the Ukraine War or reflect broader dynamics in public responses to long-running conflicts.

Overall, this project establishes a flexible framework for studying evolving public opinion and highlights how computational social science methods can be used to bridge political events and everyday online discourse.
