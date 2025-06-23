---
title: "Project - Phase II"
date: 2025-05-27
draft: false
description: "Proof of Concept - Gathering Data and Designing"
slug: "phase2post"
tags: ["project", "Setup"]
authors:
  - "katherineahn"
  - "anoushkaabroal"
  - "arthurhuang"
  - "shivenajwaliya"
showAuthorsBadges: false
---
# Project Updates
[Changed again in Phase 4] We began this phase by revisiting and refining our user archetypes, making each persona more specific and realistic. We added names, contextual details, and motivations that better aligned with the updated features of our app. For example, our original traveler persona was reframed as Archibald Bridgemont, a parent relocating to Europe with his family. We expanded his feature set to include not only healthcare priority-based matching, but also an optional comparison to his current country's system—blending both familiarity and preference in the results. Similarly, Nancy the policymaker was rewritten to focus less on a single country and more on globally informed, evidence-based reform, reflecting our system’s international comparison and forecasting capabilities. Gale, the student, was also given more context and academic grounding, reinforcing his need for tools like time-series charts and visual comparisons. 

These refined personas helped us anchor our technical decisions to real, use-driven goals. Updates to the full set of personas can be viewed in our [Phase I blog post](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase1post/)

We also went back and articulated our data sources better, and found a new, more comprehensive data set as well, that changed some of the technicalities we wanted in our inital plan. Due to this, we also changed those concepts to match our new vision in our first blog post. 

This shift in data informed several important technical updates. Most notably, we moved away from our original idea of allowing users to assign custom weights to healthcare factors and instead implemented a cosine similarity model to generate personalized country recommendations based on user priorities. We also introduced a time series forecasting model that enables users to view historical trends and predict future values for key healthcare indicators.

Together, these updates helped us move from a loosely scoped concept to a more concrete, implementable system with clearly defined features tied directly to our personas.

# Proof of Concept ML Model: 
We began exploring a linear regression model that we will continue to shape in Phase III. For this example, we standardized the Antimicrobial feature, as well as the Health Index scores, in order to create a line of best fit regression plot, and a residual graph as well.
We will most likely swap out the features based on what the user is asking (see wireframes), but we decided to explore these two features with our regression model for now. The results can be seen below: 

Line of Best Fit:
![image](LineOfBestFitProof.png)

Residual Plot:
![image](residualPlotProof.png)


# Data Sources:
At the end of Phase I, we had gathered data from two datasets, households impoverished by out of pocket payments, and general practitioners per 10,000 population. For Phase II, in addition to cleaning and visualizing these two datasets, we have also gathered, cleaned, and graphed out data from 9 new datasets, as updated in our [blog post for phase 1](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase1post/#data-sources). 8 of these new datasets were pulled from the World Health Organization API, the other dataset web scraped from the Global Health Security Index, to give us that overall score we were looking for. For each of the World of Health datasets, the gatherer information was fitted into a dataframe which had distinct country and date columns, since every W.H.O dataset contained these two values. Additionally, the Global Health Index dataset contained 300+ columns of factors which were used to help calculate the overall score for a country (ex. Antimicrobial resistance, Access to potable water, etc). Some of these factors were then visualized in comparison to the the overall score of a country, and we plan on including factors with high correlation to overall score to the slider feature mentioned in the [blog post for phase 1](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase1post/#data-sources).
We have visualized a couple of the datasets in different ways as they relate to the wireframes that we plan to implement: 


## GHS Index Dataframe:
![image](Health_Score_Dataframe.png)
This dataframe represents the healthcare index scores for various countries in 2021, as well as their subcategories of what individual factors go into calculating that healthcare score index.

## Bar Chart:
![image](Healthscore_Barchart.png)
This barchart showcases the overall healthcare score for each country, as listed on the x axis. However, it seems that there are more bars than countries listed. This must be because we have two on record years of data for each country, with two different scores. This may mean we will have to cut the dataset down further to one year, and maybe even organize by continent to list less countries on the x-axis if we are to continue with the Bar Chart visualizations. 

## Line Graphs:
![image](Expenditure_Line_Graph.png)
This graph represents the health expenditure (in GDP per capita) over time for all of the given countries, as seen in the legend for the country codes. Based on this graph we can tell that there is a general upward trend over time for all of the countries, which means that health expenditure as increased at a relatively constant rate over time since the 1920s. This will be interesting for the user to predict just how much the expenditure will increase in a future year. We may have to scale down to only displaying a couple of countries at a time, just so that the user can understand the graph better. 

## Scatter Plots:
![image](Practitioners_Expenditure_Scatterplot.png)
This scatterplot is between General Practioners per 10,000 and Total Healthcare expenditure, in order to see if there is a relationship between the two. There doesn't seem to be any logical relationship across all the countries, so this might mean that a relationship between these individual World Health Organization datasets may not make sense. Since each of the categories that we have come from a different category within the original data warehouse, it could make more sense to just plot each feature over time, and explore predictions and lines of best fit with those graphs. 

![image](Antimicrobial_Resistance_Scatterplot.png)
This scatterplot explores the relationship between overall healthcare score with one of the sub categories that go into the overall healthcare score (Antimicrobial Resistance Rates). There seems to be a situation in this data, where the overall score improves between the two years 2019 and 2021, but the antimicrobial resistance rates stay the same. This means that maybe we should look to explorethe broader factors, rather than the subcategories, because they may not make a major difference when looking at the comparison between countries and years. 



# ER Diagrams, Relational Diagram, SQL DDL:

Below are localized ER diagrams for each personna: the green diagram corresponding to the mover Archibald, red diagram corresponding to the student Gale, and the blue diagram corresponding to the policymaker Nancy.

![image](moverER.png)
![image](studentER.png)
![image](policymakerER.png)

Further, the global ER model is seen here:

![image](globalER.png)

The relational diagram was further derived as:
![image](relationalDatabase.png)

## SQL DDL:
Based on our global data model, a first pass of our SQL DDL can be seen below.

<pre> 
DROP DATABASE IF EXISTS cc_database;
CREATE DATABASE cc_database;
USE cc_database;

DROP TABLE IF EXISTS Countries;
CREATE TABLE Countries
(
    region  VARCHAR(50),
    name    VARCHAR(50),
    code    VARCHAR(50) PRIMARY KEY

);

DROP TABLE IF EXISTS OverallScore;
CREATE TABLE OverallScore(
    scoreID INT AUTO_INCREMENT PRIMARY KEY,
    country          VARCHAR(31) NOT NULL,
    year             INTEGER  NOT NULL,
    overall_score    FLOAT NOT NULL,
    prevention       FLOAT NOT NULL,
    detectReport     FLOAT NOT NULL,
    rapidResp        FLOAT NOT NULL,
    healthSys        FLOAT NOT NULL,
    intlNorms        FLOAT NOT NULL,
    riskEnv          FLOAT NOT NULL,
    FOREIGN KEY (country) REFERENCES Countries(code)
  );


DROP TABLE IF EXISTS CountryInfo;
CREATE TABLE CountryInfo
(
    countryCode VARCHAR(3) NOT NULL PRIMARY KEY,
    generalInfo        VARCHAR(700),
    healthcareInfo      VARCHAR(700),
    FOREIGN KEY (countryCode) REFERENCES Countries(code)
); 

DROP TABLE IF EXISTS CountryArticles;
CREATE TABLE CountryArticles(
    id             INT AUTO_INCREMENT PRIMARY KEY,
    country_code        VARCHAR(3) NOT NULL,
    article_title  VARCHAR(255) NOT NULL,
    article_link   VARCHAR(1371) NOT NULL,
    source         VARCHAR(100) NOT NULL,
    image_name    VARCHAR(26) NOT NULL,
    FOREIGN KEY (country_code) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS UserRoles;
CREATE TABLE UserRoles
(
    roleID INT PRIMARY KEY,
    roleName VARCHAR(50)
);


DROP TABLE IF EXISTS Users;
CREATE TABLE Users
(
    id               INT AUTO_INCREMENT PRIMARY KEY,
    first_name       VARCHAR(50),
    last_name        VARCHAR(50),
    email            VARCHAR(50),
    roleID           INT,
    country          VARCHAR(50),
    
    FOREIGN KEY (country) REFERENCES Countries(code),
    FOREIGN KEY (roleID) REFERENCES UserRoles(roleID)
);


DROP TABLE IF EXISTS Favorites;
CREATE TABLE Favorites(
    id INT AUTO_INCREMENT PRIMARY KEY,
    userID  INT,
    articleID   INT, 
    FOREIGN KEY (articleID) REFERENCES CountryArticles(id),
    FOREIGN KEY (userID) REFERENCES Users(id)
);

DROP TABLE IF EXISTS RegressionFactors;
CREATE TABLE RegressionFactors 
(
    factorID INT AUTO_INCREMENT PRIMARY KEY,
    factor_code VARCHAR(50) UNIQUE NOT NULL,
    factor_name VARCHAR(100),
    who_code VARCHAR(50),  
    table_name VARCHAR(50)
);

DROP TABLE IF EXISTS RegressionWeights;
CREATE TABLE RegressionWeights
(
    id INT AUTO_INCREMENT PRIMARY KEY,
    country VARCHAR(50),
    slope FLOAT,
    intercept FLOAT,
    mse FLOAT,
    r2 FLOAT,
    factorID INT,
    userID INT,

    FOREIGN KEY (userID) REFERENCES Users(id),
    FOREIGN KEY (country) REFERENCES Countries(code),
    FOREIGN KEY (factorID) REFERENCES RegressionFactors(factorID)
);

DROP TABLE IF EXISTS AutoregWeights;
CREATE TABLE AutoregWeights 
(
    id INT AUTO_INCREMENT PRIMARY KEY,
    country VARCHAR(50),
    factorID INT,
    userID INT,
    weight_vector JSON,
    lag_years INT DEFAULT 10,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (country) REFERENCES Countries(code),
    FOREIGN KEY (factorID) REFERENCES RegressionFactors(factorID),
    FOREIGN KEY (userID) REFERENCES Users(id),
    UNIQUE KEY unique_autoreg (country, factorID, userID)
);


DROP TABLE IF EXISTS LiveBirths;
CREATE TABLE LiveBirths(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL,
    YEAR    INTEGER  NOT NULL,
    VALUE   NUMERIC(5,2) NOT NULL,

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS LifeExpectancy;
CREATE TABLE LifeExpectancy(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL,
    YEAR    INTEGER,
    VALUE   NUMERIC(5,2),

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS GenPractitioners;
CREATE TABLE GenPractitioners(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL,
    YEAR    INTEGER  NOT NULL,
    VALUE   NUMERIC(5,2) NOT NULL,

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS HealthExpend;
CREATE TABLE HealthExpend(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL,
    YEAR    INTEGER  NOT NULL,
    VALUE   NUMERIC(6,2) NOT NULL,

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS ImpoverishedHouse;
CREATE TABLE ImpoverishedHouse(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL ,
    YEAR    INTEGER  NOT NULL,
    VALUE   NUMERIC(4,2) NOT NULL,

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS InfantMortality;
CREATE TABLE InfantMortality(
    id INT AUTO_INCREMENT PRIMARY KEY,
    COUNTRY VARCHAR(50) NOT NULL,
    YEAR    INTEGER  NOT NULL,
    VALUE   NUMERIC(5,2) NOT NULL,

    FOREIGN KEY (COUNTRY) REFERENCES Countries(code)
);

DROP TABLE IF EXISTS CosineWeights;
CREATE TABLE CosineWeights(
    id INT AUTO_INCREMENT PRIMARY KEY,
    userID INT,
    preventionWeight FLOAT DEFAULT 0.0,
    detectReportWeight FLOAT DEFAULT 0.0,
    rapidRespWeight FLOAT DEFAULT 0.0,
    healthSysWeight FLOAT DEFAULT 0.0,
    intlNormsWeight FLOAT DEFAULT 0.0,
    riskEnvWeight FLOAT DEFAULT 0.0,
    FOREIGN KEY (userID) REFERENCES Users(id)
);
 </pre>

 # WireFrames:

Our current wireframes can be viewed here: 

![image](wfHomepage.jpg)
This wireframe shows the homepage of CareCompass. It introduces the different user archetypes, and the application as a whole.

![image](wfCountryComparator.jpg)
The _Country Comparator_ page is utilized by the student archetype where they are given the opportunity to choose 2-3 countries to compare 6 features' scores against one another in an organized table. Further, the student may choose to track a specific feature over time, given by a dropdown menu and visualized in a line plot (with different countries in different colors).

![image](wfSliders.jpg)
The _Sliders_ page illustrates a personalized and customizable aspect to our application, particularly geared toward a moving resident. There are 6 factors that may be considered and the sliders allow users to prioritize some over others. The result of using the sliders is a bar chart that visualizes countries' scores based on the weights provided by the user (sorted from highest score to lowest), as well as a gradient map of the scores. An additional feature of this page is for individuals to choose what country they are coming from, and whether or not they would like that country score to factor into their results.

![image](wfCountryProfile.jpg)
The _Country Profile_ is a dashboard that can be reached on its own or through links on other pages. This page is currently housed for residents to learn more about countries of interest to them. Additionally, users may see on the right side bar provided links to country profiles of similar score to that of the country of interest (country of which profile currently on).

![image](wfFavoritedArticles.jpg)
The _Favorited Articles_ page lets students and relocating residents revisit saved healthcare news and research. Displayed in a grid with titles, sources, and countries, each article can be removed from the list. Articles are favorited from individual country profile pages and appear here automatically, helping users gather real-world context for research, comparison, and informed decision-making.

![image](wfRegression.jpg)
The _Regression_ page may be utilized by the policymaker archetype as they consider how a given country's score may change over time depending on the region, certain factors, and time. 

![image](wfTargetScores.jpg)
The _Target Scores_ page illustrates a policymaker's ability to utilize CareCompass in a way to project their country's growth. Given the current 6 scores of a country, the user may input target scores and the projected time until those target scores are reached will be output in years (if applicable).



