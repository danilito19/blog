---
layout: post
title: "Data Science &amp; Statistical Analysis Methodology"
modified: 2015-03-03 20:11:05 -0600
tags: [statistics, data science]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

I tend to be an extremely methodical student. I take notes, make outlines, and study by teaching myself the topics I'm learning. I also like to create step-by-step methodologies to solve problems. For example, for calculus classes I would create step-by-step outlines of how to solve derivatives, differential equations, etc. Likewise, here is a data-statistical analysis workflow I created for each time I start a new project. Of course, steps depend on the data at hand, but this helps organize topics into data munging, visualization, analysis, and even theory. 

##Data Science Methodology & Workflow  

###Preparation
* Formulate a problem and think of solutions and applications
* Create a source control repository to save all work

###Gathering Data
* Any data that we can possibly acquire and that could be relevant to the problem
* Web Scraping
* API
* Inport files

###Cleaning data
* Check for NAs or NaaN
* Check for missing data
* Consider irrelevant indicators
* Understand values for quantitative data and categories for qualitative data

###Exploratory analysis
* Plot the data with graphs and charts; consider the distribution (it will affect model you pick)
* Apply basic statistical correlation between variables, if applicable

###Visualization
* Create visualizations to show, explain, and more deeply understand your data; use tools in R, Tableau or d3
* Use visualizations to determine which predictors are relevant and consider using these as features for your model

###Statistical – Predictive modeling
* Begin formulating and implementing a statistical model for the data. Do I want to predict? Do I want to find out if there is a relationship between variables? Do I want to find clusters in data? 
* Look at a scatterplot matrix of your data
* Choose features and create new features from the variables you have - log or quadratic versions, new categorical variables from quantitative ones 
* Tip:  Use the Box-Cox test to normalize data (only for some data); recall that transforming data alters interpretation of coefficients
* Choose a model, but understand the underlying model assumptions, importance of features, over fitting, and other shortcomings of the models
* Recall that **correlation does not imply causation** and that **all models are wrong, but some are useful**  
* Evaluate yoru model. Look at R^2, adjusted R^2, plots of fitted versus residuals, and other evaluation metrics


##**I would love to hear about your data scientist workflow!**
