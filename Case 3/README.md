# CASE 3 - CLIMATE RISK ASSESSMENT
## PROBLEM DESCRIPTION 
When we talk about "the sustainability of stock trades" we're not typically referring to the act of trading stocks in and of itself. Instead, we're usually discussing the sustainability implications of the companies behind those stocks. In essence, it's about evaluating the environmental, social, and governance (ESG) factors of the companies whose stocks are being traded. The calculation of ESG (Environmental, Social, and Governance) ratings is complex and can vary significantly between different rating agencies. Each agency usually develops its methodology, which might involve proprietary algorithms, weightings, and metrics. Because of this variation, a company might receive different ESG ratings from different agencies, even though the underlying data might be largely similar. 

_Here's a general overview of how ESG ratings might be calculated:_

### Data Collection:
* **Public Disclosures:** Review of annual reports, sustainability reports, and other public documents. 

* **Surveys and Questionnaires**: Direct engagement with companies to gather more specific or additional information. 

* **Third-party Data:** Information from non-profit organizations, news sources, and other external data providers. 

* **On-site Audits:** Some agencies might conduct audits, especially for verifying specific environmental or social practices. 

* **Metric Definition and Weighting:** Each agency will define its set of metrics for Environmental, Social, and Governance pillars. Different metrics might have different weightings based on their perceived importance.


### Scoring:
Companies are scored on individual metrics based on the data collected. These individual scores are then aggregated, often using weighted averages, to create scores for the Environmental, Social, and Governance pillars. A composite ESG score might then be computed based on the three pillar scores. 


### Normalization and Peer Comparison: 
Scores might be normalized to ensure comparability across companies and industries. Companies are often benchmarked against peers in their industry to give a relative performance rating.

### Hypothetical Formula Examples: 
1. **Carbon Emissions Score:**

$$\textrm{Carbon Score} = \frac{\textit{Company's Annual Carbon Emmissions}}{\textit{Industry Average Annual Carbon Emissions}} \times 100$$

_A score of 100 would mean the company's emissions are at the industry average. Below 100 indicates better performance (less emissions), and above 100 indicates worse performance._

2. **Board Diversity Score:**

$$\textrm{Diversity Score} = \frac{\textit{Number of Diverse Board Members}}{\textit{Total Number of Board Members}} \times 100$$

_This would give a percentage representation of diverse board members._

3. **Employee Satisfaction Score:**

Based on annual employee surveys. If 85% of employees report being satisfied or very satisfied, the Employee Satisfaction Score could be 85. 




**Hypothetical Composite ESG Score:**

Let's say we only consider three factors: Carbon Emissions, Board Diversity, and Employee Satisfaction. We decided to give 50% weight to Carbon Emissions, 25% to Board Diversity, and 25% to Employee Satisfaction. 

$$\textrm{ESG Score} = 0.5 \times \textrm{Carbon Score} + 0.25 \times \textrm{Diversity Score} + 0.25 \times \textrm{Employee Safisfaction Score}$$

**Hypothetical Rating Ranges:**

Once scores are normalized and compared against peers, rating agencies could categorize them into different ranges or tiers. A simple tier system could be: 
* 80-100: Excellent 
* 60-79: Good 
* 40-59: Fair 
* 20-39: Poor 
* 0-19: Very Poor


## GOAL
Develop your own ESG score, detail your methodology and offer sample calculations derived from real-world data you've unearthed.  


## SUGGESTED APPROACH 
* Technology exploration  
  - Explore available data sources.  
* Project Specification  
  - Define your own KPIs assessing various parameters of a company's health.
* Method - System Architecture  
  - Describe how data can be obtained, processed and aggregated, to keep your ESG score up-to-date 
* Development of Minimum Viable Concept  
  - Create Figma/Plotly design showcasing all KPIs and the final ESG score 
