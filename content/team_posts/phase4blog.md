---
title: "Project - Phase IV"
date: 2025-06-13
draft: false
description: "Proof of Concept - Starting Deployment"
slug: "phase4post"
tags: ["project", "Setup"]
authors:
  - "katherineahn"
  - "anoushkaabroal"
  - "arthurhuang"
  - "shivenajwaliya"
showAuthorsBadges: false
---

# Project Reflection

After five weeks, we have put together our platform Care Compass. We have spent our time improving and implementing our machine learning models, as well as creating our tables and updating our API Layer so that the backend integrates seamlessly with the front end. Our platform provides users with future predictions of healthcare values, information about similar healthcare systems, and country information in our country profile. Our UI has been updated throughout this process, and is now user-friendly with streamlit resources such as buttons, drag-and-drop, and sliders. Below, we articulate a bit more in-depth about our work processes, and we include screenshots of some of the front end pages.

# REST API Matrix
We cleaned up the backend by auditing and reorganizing our REST API routes to match the final structure of the application. We removed several unused country routes that were never called by the frontend, and deleted outdated comparison routes, since their functionality was now handled through our ML-based endpoints. We also moved the recommendation route from its own recommendations blueprint into the country blueprint to simplify the structure—there was no need for an entire folder just for one route. After making these changes, we updated the REST API matrix to reflect the current set of endpoints. [Phase 3 Blog Post](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase3post/#rest-api) updated accordingly. 


# ML Models 

## Autoregressive Model: 

The autoregressive model was a new addition during this phase which was aimed at replacing the linear regression model from phase 3, which was a poor fit for the World Health Organization data we were trying to predict, which is time series data. The autoregressive model is accessible by two of the user person types. The first being the student user, who is able to interact with the autoregressive model on their country comparator page which allows them to see a graph of the historic and predicted values of up to three countries for one of the given W.H.O datasets (live births, general practitioners per 10,000 population, and total expenditure). Specifically the user selects up to three different countries at the top of the page. The user then has the option to see a table for The second is the policy maker user who interacts with the model on the features over time page which allows them to input a country, date, and returns a graph of all the countries values (historic and predicted) up to that point for a given dataset. In order to test the accuracy and reliability of our model, we cross validated the model and calculated the mean squared error, r^2, and residuals of the model for each of our datasets, which can be seen in the images below. For the general practitioners model, the r^2 of 0.88 demonstrates that the regression model fits the variation of the data well. This is further supported by the mean squared error of 0.04 which shows very little difference between the model predicted values and the actual values on average. In terms of residuals, the residuals over order graph seen below does not seem to demonstrate any significant patterns/trends within the data, which is an indication that the model does not possess autocorrelation. This is the same conclusion with the model for live births, that has a r^2 of 0.79, a MSE of 0.007, and a residuals over order graph without pattern. However, the total expenditure model possessed an r^2 of 0.14 and had an mse of 314, which indicates that the autoregressive model is not a great representation of total expenditure. Based on these analyses, it can be concluded that the autoregressive model can be considered a “good” model for predicting general practitioners and live births over time, but not for total expenditure.

### General Practitioners Analysis:
![genprac_value](genprac_values.png)
![genprac_graph](genprac_graph.png)

###Live Births Analysis:
![livebirths_value](livebirths_values.png)
![livebirths_graph](livebirths_graph.png)

###Total Expenditure Analysis:
![expenditure_value](expenditure_values.png)
![expenditure_graph](expenditure_graph.png)

## Cosine Similarity Model:

The cosine similarity model that we worked on during phase 3 is now fully implemented. The user is now able to rank their priority of the healthcare factors: Prevention, Risk Environment, Health Systems, International Norms Compliance, Rapid Response, and Detection & Reporting. The user can drag-and-drop to rank these factors, and then use sliders to fine tune their ranking weights. Each factor also has a little information tooltip, to explain what each factor really means (a bit of information on the subcategories in the ghs_index). A route is used to get the cosine similarity information after the calculations are put into a dataframe. The visualizations include a bar chart that has the top five countries and their similarity score, as well as a gradient map that is colored based on how similar the country’s healthcare system is to the given country. This model is also used in the Country Profile page, where the sidebar has the top five similar countries based on healthcare factors.  


# Data Models

Our data model has continued to get altered and tweaked alongside our code to power such an interactive platform that is Care Compass. We have created a relational database so as to present the ecosystem of our project, displaying how key components interrelate. Below you can see the relational database in question:

![image](relationalDatabase.png)

The core entities of our application are the Users, along with UserRoles, and Countries. The Users/UserRoles tables store information regarding the id, name, user archetype, email, and associated country. The Country table is a quite central node, linking all health metrics and additional country information in order to ensure everything is tied to the correct country. Further looking at the tables referencing the Country table, this is exclusively done using a country’s code; some tables you can see are the CountryArticles and CountryInformation tables, which house generated qualitative data, as well as six feature variables our app tracks over time (infant mortality, life expectancy, etc.), along with GHS index (country healthcare score). As you can see the six feature variables have the same format, storing country, year, and value so a future edit to simplify our data model could be to consolidate that into one feature table, with a FK id that indicates which feature variable it is. As you can see, the overall score differs from the feature variables as it aggregates six other factors like prevention, rapid response, etc. Moving back down, our last table is the Favorites table; which has a relationship with CountryArticles and Users. How the Favorites table works is when a user clicks to ‘favorite’ an article under a CountryProfile, this articleID is added to the table which can then be retrieved later on; further, an article can be ‘unfavorited’. 

Along the side, you may see there are a few tables used for storing information regarding our ML models. So, for our regression model we store our hyperparameters per country and the training/testing datasets; and for the cosine similarity we store the weights of each health-related factor of the GHS Index.

# Screenshots of Front-End
Below are the country comparator components, including a table of the features and tracking the feature over time on a graph
![image](country_comparator_table.png)
![image](country_comparator_graph.png)

Below are the images of our country profile page, including information about the country, as well as mock data powered articles  
![image](profile_info.png)
![image](profile_articles.png)

Below are the screenshots of the features over time page, which display the graph based on the chosen button of the given features
![image](feat_over_time_before.png)
![image](feat_over_time_after.png)

Below are the parts of our Customize Your Move page, where you can choose your country, make your rankings, tweak the ranking weights with sliders, view your rankings, and view your cosine similarity scores as a bar chart or a gradient map. 
![image](custom_move_drag.png)
![image](custom_move_sliders.png)
![image](custom_move_rankings.png)
![image](custome_move_bar.png)
![image](custome_move_map.png)



## Healthcare System Customization Tool

Our Customize Your Move tool helps you discover countries with healthcare systems that match your personal priorities through an intuitive 3-step process.

![Full Interface](custom_move_full.png)

### How It Works

**Step 1:** Select your current country  
**Step 2:** Drag and rank 6 healthcare factors by importance (Prevention, Health System, Rapid Response, etc.)  
**Step 3:** Fine-tune each factor with sliders (0-10 scale)

Click "Find My Matches" to see your personalized results!

![Selection Interface](custom_move_selection.png)

### View Your Results

**Bar Chart**
![Bar Chart](custom_move_bar.png)
See your top 5 matching countries with similarity scores

**World Map**
![World Map](custom_move_map.png)
Visualize global matches with color-coded similarity scores


**Learn More**
![Learn Section](custom_move_learn.png)
Understand the methodology behind the Global Health Security Index

**Visit our github repository to view our project in its full depth:**
[Github Repository for Project](https://github.com/RemainingDelta/Care-Compass) 

