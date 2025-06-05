---
title: "Project - Phase III"
date: 2025-06-05
draft: false
description: "Proof of Concept - Starting Deployment"
slug: "phase3post"
tags: ["project", "Setup"]
authors:
  - "katherineahn"
  - "anoushkaabroal"
  - "arthurhuang"
  - "shivenajwaliya"
showAuthorsBadges: false
---
# Project Updates  
Following the submission of Phase II, our group continued working on refining our data models and first ML model, as well as our second ML model. Further, on the back-end side of things, we worked to develop a REST API for our project. Finally, on the front-end, we built an initial structure for the majority of the pages of our app.

# Data Models 
After receiving feedback on our data models, and continuing to clarify the operations of our app, we were able to edit our data models, along with their corresponding diagrams. Thinking deeper into what data should be stored and why, we created user and user role entities, as well as edited the attributes attached to align more with our machine learning models (this required creating a new entity for each ML model). Additionally, we implemented the areas of which the machine learning data will be stored, including the various features over time. The ER diagrams, relational diagram, and DDL were updated in [blog post 2](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase2post/#er-diagrams-relational-diagram-sql-ddl). We currently have the following tables in our SQL:

- Country - Holds basic country metadata
- CountryInfo - Weak entity to Country, stores qualitative country data
- OverallScore - Holds score for each major factor (1-6) for a particular country (FK)
- Users - Holds basic user metadata
- Factors - Holds factorID for identification of each of the 6 major factors, as well as their scores
- Comparator - Enables country to country comparisons
- UserScore - Acts as a bridge table connecting Users to OverallScore for a country
- regression_weights - ML model to hold calculated weights for regression (i.e. slope, intercept, mse, r^2)
- cosine_similarity - ML model table to hold the provided weights by the users 
- train_test - ML model table to hold train and test data for regression model, depending on country


Additionally, our current data – for drafting purposes – includes some randomly generated data, however, our final application data will be sourced from our various datasets (mainly from WHO and the GHS Index). 

# Machine Learning Models
In Phase II, we began to explore a linear regression machine learning model for the student and policymaker personas. For the linear regression machine learning model, we utilize the following features: 
Live births per 1000
Infant mortality rate 
General practitioners per 10,000
Life expectancy in years
Total health expenditure 
Number of impoverished households after out of pocket healthcare payments
For phase 3 of the project, we improved upon this regression model by using train_test_split to split our datasets into training and testing sets which were then used to calculate the line of best fit as well as the mean squared error and coefficient of determination. This regression model was then transfered from the jupyter notebook to ml python script in the ml folder, and was connected to the the feature_over_time front end page via routes. This model is now accesible on the front page and can be fit to any of the 6 datasets for any country inputted by the user.

Further, we began working on our second machine learning model, the personalized country matching for future residents. The purpose of this model was to match users’ healthcare priorities with the countries that align most closely along key health system factors. This cosine similarity model is based off of the healthcare scores and subcategories from the GHS index. This model is complete, we simply need to account for the weights with simple math operations for multiplying each individual subcategory. The model is able to use input data to query, so we simply need to display the data in a more user-friendly format. 

# REST API
In regards to developing our REST API, we created various routes so as to connect our web app to our models and database. Some of these include GET, PUT, POST, and DELETE routes for countries, as well as routes especially for our machine learning models. 


Below is a matrix of our thus far created REST APIs
... insert image ...

Finally, we built out an initial structure for most of our front end pages, as well as personalized  our web page colors and font. 

Putting all this together, the primary focus of our group was to get our linear regression model up and running on the Students’ Country Comparator page. 








