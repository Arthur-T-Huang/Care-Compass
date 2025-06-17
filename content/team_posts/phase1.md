---
title: "Project - Phase I"
date: 2025-05-20
draft: false
description: "Proof of Concept"
slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "katherineahn"
  - "anoushkaabroal"
  - "arthurhuang"
  - "shivenajwaliya"
showAuthorsBadges: false
---

# Project Description:
With an expansive network of public health knowledge at our fingertips, evaluating and comparing healthcare systems across countries can be a complex and overwhelming process. Our project aims to simplify this by evaluating complex public health data and providing users access to succinct, intuitive insights, making it more easily accessible and understandable to a broader audience of users.

We are building an application that helps users explore and evaluate the healthcare systems of different countries based on **six core factors** from the Global Health Security Index:
- Prevention
- Detection and Reporting
- Rapid Response
- Health System
- Compliance with International Norms
- Risk Environment

We are utilizing the Global Health Security Index, which provides a standardized scoring system across six core healthcare factors to generate a composite assessment for each country. Rather than allowing users to customize weights directly, we leverage this index to implement a cosine similarity–based recommendation system. Users express their healthcare priorities through interactive sliders, which are converted into a preference vector. This vector is compared against normalized country data to identify nations with the most similar healthcare profiles. Additionally, users can optionally select a country of origin. When enabled, recommendations blend similarity to that country with the user’s stated preferences to deliver context-aware results tailored to both familiarity and healthcare priorities.

The resulting country matches are visualized through ranked comparisons, interactive heat maps, and a side-by-side comparison tool for exploring differences in key health indicators. In addition, users can track how selected indicators have changed over time and view predictive forecasts for future performance, offering a forward-looking perspective based on historical trends.

The application converts raw public healthcare data to insights to drive equitable improvements and informed populations regarding healthcare around the world.


# Personas/Archetypes: 
## 1. Student studying _Global Health_
**Description:** Student researchers use Care Compass to support academic projects, comparative analyses, and class presentations focused on international health systems. These users are often studying public health, policy, or global affairs, and need access to clear, credible, and visually engaging data.


**User Stories:**
- As a student, I want to compare healthcare quality and outcomes across multiple countries so I can identify and explain differences in system performance.
- As a student, I want to access key health indicators—like general practitioners per capita, infant mortality rate, and life expectancy—in one place to support my research.
- As a student, I want to generate visualizations and comparative charts so I can include them in reports or class presentations.
- As a student, I want to project future trends in health outcomes using forecasting tools so I can analyze policy impact over time.
- As a student, I want to explore detailed country profiles with curated articles and external resources for deeper understanding and citations.


## 2. Individual moving abroad
**Description:** These users are planning to move abroad and are seeking countries with healthcare systems that align with their personal values and expectations. They may be individuals, couples, or families evaluating long-term living conditions and health infrastructure abroad.


**User Stories:**
- As a relocating resident, I want to customize healthcare priorities (like prevention or emergency response) so I can get a list of countries that best fit my needs.
- As a relocating resident, I want to include my current country in the matching process so I can see which systems are both familiar and aligned with my priorities.
- As a relocating resident, I want to view maps and bar charts that visually show how countries match my preferences so I can compare options easily.
- As a relocating resident, I want to review detailed dashboards with country overviews, healthcare context, and external references so I can make an informed decision.
- As a relocating resident, I want to discover countries with similar healthcare systems so I can expand my options without sacrificing care quality.


## 3. Policymaker analyzing current healthcare institutions
**Description:** Policy makers use Care Compass to monitor and compare healthcare systems internationally. They rely on data-driven insights to shape policy, evaluate system performance, and propose strategic reforms.


**User Stories:**
- As a policymaker, I want to monitor historical trends in healthcare indicators (like expenditure or life expectancy) so I can evaluate past reform impacts.
- As a policymaker, I want to forecast future values for selected indicators so I can anticipate system needs and guide policy planning.
- As a policymaker, I want to input target scores for healthcare factors and see how long it might take to reach them so I can develop realistic improvement timelines.
- As a policymaker, I want to identify indicators that are improving or stagnating so I can focus on evidence-based areas for intervention.


# Models: 
**1. Cosine Similarity: Personalized Country Matching**
Used to match users with countries whose healthcare systems align most closely with their personal priorities. Users express their preferences across six key health system factors (e.g., prevention, response, infrastructure), and the system uses cosine similarity to calculate similarity between the user’s input and each country’s normalized profile. Also users can input their current country, which blends similarity to that country with their preferences. This allows for personalized recommendations that balance familiar healthcare systems with their desired system attributes.

Used by: Future Resident
Feature link: Ranking and possible Slider-based input → ranked list of top-matching countries

**2. Predictive Modeling: Indicator Forecasting**
Used to project future values for specific healthcare indicators (e.g., life expectancy, total health expenditures) based on historical trends. This supports forward-looking insights for both students and policymakers.

Used by: Students, Policymaker
Feature link: Time series chart + k-year prediction overlay for selected indicators


# Data Sources: 
For our data sources, we have pulled together a variety of features that will be further explored through our Machine Learning Models. We list two of the data sources below as examples of our ability to successfully access the data. 

**Data Source 1:**
Generalist medical practitioners, per 10 000 population:

The following dataset provides the number of general medical practitioners in relation to the total
population of the country for a list of countries in Europe as well as in neighboring regions.
This data can serve as a measure of the prevelance of accessabile health services in different countries and regions and can be used in conjuction with other gathered data in order to determine a numerical ranking/evaluation of a countries healthcare system.

Dataset Json:
![image](Practioner_dataframe.png)



**Data Source 2:**
Households impoverished after out-of-pocket healthcare payments

The following dataset displays the percentage of households who are classified as impoverished after
enduring out of pocket medical payments. This data can serve as a critical factor in determining whether a country's healthcare system is afforable and avaliable for the general population.

Dataset Json: 
![image](impoverished_dataFrame.png)

These are two datasets that we would consider impelemting in reference to the factors we aim to focus on, as we also plan on including additional datasets from the World Health Organization throughout the course of this project. We simply chose these datasets to highlight, as they help showcase that we can query from the WHO data warehouse. We will probably have more than 3 datasets for each key factor.     

Other datasets include:
- Life expectancy in Years, from the WHO data warehouse 
- Infant Mortality Rates, from the WHO data warehouse 
- Total Health Expenditure, from the WHO data warehouse
- Catastrophic Health Spending in all wealth classes, from the WHO data warehouse
- Percentage of children vaccinated against measles, from the WHO data warehouse
- Live births per 100 population, from the WHO data warehouse 
- Percentage of Overweight and Obese children, from the WHO data warehouse
- Life Satisfaction Score (Scale 1-10), from the WHO data warehouse
- Healthcare "Scores" for countries, based on a myriad of features from the Global Health Index 


