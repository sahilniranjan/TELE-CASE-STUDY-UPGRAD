# Telecom Churn Case Study
## Problem Statement
## Overview

In the telecom industry, customers are able to choose from multiple service providers and actively switch from one operator to another. In this highly competitive market, the telecommunications industry experiences an average of 15-25% annual churn rate. Given the fact that it costs 5-10 times more to acquire a new customer than to retain an existing one, customer retention has now become even more important than customer acquisition.

For many incumbent operators, retaining high profitable customers is the number one business goal.

To reduce customer churn, telecom companies need to predict which customers are at high risk of churn.

In this project, we will analyse customer-level data of a leading telecom firm, build predictive models to identify customers at high risk of churn and identify the main indicators of churn.

## Understanding and Defining Churn

In the telecom sector, there are two primary payment methods: postpaid (where consumers pay a monthly or annual fee after using the services) and prepaid (where users pay/recharge with a set amount in advance and then use the services).

In the postpaid model, customers typically contact the current operator to cancel the services when they want to switch to another operator, so we can immediately identify this as a case of churn.

However, in the prepaid model, customers who want to switch to another network can just stop using the services without giving any prior notice, and it can be difficult to tell whether someone has actually churned or is merely not using them at the moment (for example, someone may be travelling abroad for a month or two with the intention of returning to using the services later).

Therefore, predicting churn for prepaid clients is typically more important (and complex), and the term "churn" needs to be defined precisely.  Additionally, prepaid plans are more popular in India and Southeast Asia than postpaid plans are in North America and Europe.

The Indian and Southeast Asian markets are the foundation of this endeavour.


## Churn Definitions 

Churn can be characterised in a number of ways, including:

1- Customers who have not used any revenue-generating services over a specific time period, such as mobile internet, outbound calls, SMS, etc. The use of aggregate metrics is another option. For example, "customers who have generated less than INR 4 per month in total/average/median revenue."

The biggest flaw in this definition is that some consumers just use the services to receive calls or SMSs from their wage-earning counterparts; in other words, they don't produce any income but nevertheless use the services. For instance, many people in rural locations only get calls from their siblings who work in cities.


2- Customers that have not made any calls, used the internet, or made any other outgoing or incoming usage over a period of time (usage-based churn).

This definition may have the drawback that it may be too late to take corrective measures to retain the client once they have stopped utilising the services for a period. For instance, if we use a "two-months zero usage" definition of churn, projecting churn may be meaningless because the client will have already gone to a different operator by that point.

In this project, churn will be defined according to the **usage-based** concept.


3- High value Churn

Approximately 80% of revenue in the Indian and Southeast Asian markets comes from the top 20% of clients (referred to as high-value consumers). Therefore, if we can lower the high-value client churn, we can lower large revenue leakage.

In this project, we will exclusively forecast churn on high-value customers and identify high-value customers based on a certain indicator (discussed below).


4- Understanding the Data and the Business Objective

The dataset includes customer-level data for the months of June, July, August, and September across a four-month period. The corresponding month codes are 6, 7, 8, and 9. 

Using the data (features) from the first three months, the business goal is to estimate the churn in the most recent (i.e., ninth) month. Understanding the normal consumer behaviours during turnover can help with this assignment.

5- Recognising Client Behaviour During Churn

clients typically take some time to decide whether to switch to a rival business (this is especially true for high-value clients). In churn prediction, we presum that the client lifetime comprises three stages:


1. The positive phase: The customer is content with the service and acts normally throughout this time.

2. The 'activity' phase: In this stage, the customer's experience starts to suffer; for instance, he or she receives an alluring offer from a rival, pays unfair fees, is dissatisfied with the level of service, etc. The client typically behaves differently during this time than during the "good" months. Additionally, as some remedial actions (such matching the competitor's offer/improving the service quality etc.) can be implemented at this stage, it is vital to identify high-churn-risk clients during this period.

3. The 'churn' phase: The consumer is considered to have churned during this time. Based on this stage, we define churn. It's also crucial to remember that we cannot predict using this data at the time of the action months, which is the time of prediction. We therefore remove all data associated with this phase after classifying churn as 1/0 based on this phase.


The first two months in this scenario are the "good" phase, the third month is the "action" phase, and the fourth month is the "churn" period because we are working over a four-month window.


## Dictionary of Datasets

Downloading the dataset is possible [here] (https://drive.google.com/file/d/1SWnADIda31mVFevFcfkGtcgBHTKKI94J/view).

A data dictionary has been posted. The definitions of acronyms can be found in the data dictionary. IC (incoming), OG (outgoing), T2T (telecom operator to telecom operator), T2O (telecom operator to another operator), RECH (recharge), and others are some of the common ones.

It is implied that qualities with the suffixes 6, 7, 8, or 9 correspond to the corresponding months.


# Preparation of Data

For this issue, it's critical to perform the data preparation stages listed below:

1. Create fresh features:
The ability to distinguish between excellent and bad models using good features makes this one of the most crucial steps in the data preparation process. We will leverage our knowledge of business to derive characteristics that could serve as key churn indicators.


2. Remove high-value clients:
We simply need to forecast turnover for high-value customers, as was already mentioned. As an example of a high-value client, consider: Those who have recharged with a sum more than or equal to X, where X represents the 70th percentile of the typical recharge sum over the first two months (the favourable phase).

3. "Tag churners and remove churn phase attributes":
Now, based on the fourth month, tag the churned consumers as follows: Those who are in the churn phase but have not made any calls (incoming or outgoing) OR even accessed mobile internet once. To identify churners, we must use the following attributes:

Volume 2 GB_9 - Volume 3 MB_9, Total_ic_mou_9, and Total_og_mou_9


All the attributes pertaining to the churn phase (all attributes with "_9," "_10," etc.) must be removed after labelling churners.


## Modelling

Create models to foresee churn. The predictive model we'll create will have two functions:

1. It will be used to forecast the churn phase, or whether a high-value customer would leave in the near future. Knowing this allows the business to take appropriate action, such as offering customised programmes or discounts on recharges.

2. It will be utilised to find significant factors that are reliable churn predictors. These factors might also reveal the factors influencing customers' decisions to transfer networks.


In some circumstances, a single machine learning model can accomplish both of the aforementioned objectives. But because there are so many features in this case, we should try a dimensionality reduction method like PCA before creating a predictive model. We can apply any classification model after PCA.

We will try to use ways to tackle class imbalance because the rate of churn is normally modest (between 5 and 10%; this is known as class-imbalance). 

To create the model, we might consider the following steps:

1. Perform data preprocessing (convert columns to the proper formats, deal with missing values, etc.).
2. Perform appropriate exploratory analysis to glean valuable insights (whether immediately applicable to business or ultimately applicable for modeling/feature engineering).
3. Create fresh features.
4. Use PCA to lessen the number of variables.
5. Train several models, fine-tune model hyperparameters, etc. (adequate procedures should be used to handle class imbalance).
6. Use the right evaluation measures to assess the models. Remember that effectively identifying churners is more crucial than precisely identifying non-churners; use an evaluation metric that matches this business objective.
7. Lastly, pick a model based on an assessment metric.


Only one of the two objectives will be accomplished by the aforementioned model—predicting clients who will leave. The aforementioned methodology cannot be used to pinpoint the critical churn factors. Because PCA typically produces components that are difficult to read, this is the case.

As a result, we will create a new model with the primary goal of identifying key predictor features that aid in the understanding of churn indicators by the business. A model from the tree family or one based on logistic regression is a solid option for determining important variables. When using logistic regression, we shall take multi-collinearity into account.

Use plots, summary tables, or other visual displays to highlight the most crucial predictors after you've determined their significance.


Finally, based on our observations, suggest methods for controlling client attrition.

