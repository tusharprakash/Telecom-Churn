# Problem Statement:

###
In the telecom industry, customers are able to choose from multiple service providers and actively switch from one operator to another. In this highly competitive market, the telecommunications industry experiences an average of 15-25% annual churn rate. Given the fact that it costs 5-10 times more to acquire a new customer than to retain an existing one, customer retention has now become even more important than customer acquisition.

For many incumbent operators, retaining high profitable customers is the number one business goal.

To reduce customer churn, telecom companies need to predict which customers are at high risk of churn.

The data can be found [here](https://drive.google.com/file/d/1SWnADIda31mVFevFcfkGtcgBHTKKI94J/view?usp=sharing)
###

# Business Objective:

###
Understanding and Defining Churn
There are two main models of payment in the telecom industry - postpaid (customers pay a monthly/annual bill after using the services) and prepaid (customers pay/recharge with a certain amount in advance and then use the services).

In the postpaid model, when customers want to switch to another operator, they usually inform the existing operator to terminate the services, and you directly know that this is an instance of churn.

However, in the prepaid model, customers who want to switch to another network can simply stop using the services without any notice, and it is hard to know whether someone has actually churned or is simply not using the services temporarily (e.g. someone may be on a trip abroad for a month or two and then intend to resume using the services again).

Thus, churn prediction is usually more critical (and non-trivial) for prepaid customers, and the term ‘churn’ should be defined carefully.  Also, prepaid is the most common model in India and southeast Asia, while postpaid is more common in Europe in North America.

 This project is based on the Indian and Southeast Asian market.
###
 
## Definitions of Churn

###
There are various ways to define churn, such as:

**Revenue-based churn:** Customers who have not utilised any revenue-generating facilities such as mobile internet, outgoing calls, SMS etc. over a given period of time. One could also use aggregate metrics such as ‘customers who have generated less than INR 4 per month in total/average/median revenue’.

The main shortcoming of this definition is that there are customers who only receive calls/SMSes from their wage-earning counterparts, i.e. they don’t generate revenue but use the services. For example, many users in rural areas only receive calls from their wage-earning siblings in urban areas.

**Usage-based churn:** Customers who have not done any usage, either incoming or outgoing - in terms of calls, internet etc. over a period of time.

A potential shortcoming of this definition is that when the customer has stopped using the services for a while, it may be too late to take any corrective actions to retain them. For e.g., if you define churn based on a ‘two-months zero usage’ period, predicting churn could be useless since by that time the customer would have already switched to another operator.

**In this project, you will use the usage-based definition to define churn.**

**High-value Churn:** In the Indian and the southeast Asian market, approximately 80% of revenue comes from the top 20% customers (called high-value customers). Thus, if we can reduce churn of the high-value customers, we will be able to reduce significant revenue leakage.

In this project, you will define high-value customers based on a certain metric (mentioned later below) and predict churn only on high-value customers. 

## Understanding the Business Objective and the Data 

The dataset contains customer-level information for a span of four consecutive months - June, July, August and September. The months are encoded as 6, 7, 8 and 9, respectively.

The business objective is to predict the churn in the last (i.e. the ninth) month using the data (features) from the first three months. To do this task well, understanding the typical customer behaviour during churn will be helpful.
###

# Methodology and Observations

###
The data given is highly imbalanced with only 8% churn cases. Moreover, there are many different attributes spread across each of the month. To handle outliers in each of the columns, we set a floor and ceiling of 3 standard deviations. 

We define *churn* as zero talktime minutes for incoming and outgoing calls in the 9th month (for which churn is being defined) and zero minutes of internet usage for 2g and 3g networks. More specifically, the following columns have a value of 0:

total_ic_mou_9

total_og_mou_9

vol_2g_mb_9

vol_3g_mb_9

We drop all the other 9th month columns as that the month for which pediction needs to be made.

In EDA, we compare how various attributes of the user behaviour vary in the months prior to the month for which churn has to be predicted. We see certain discernable dissimilarities between behavior of churners vs. non-churners in the month leading up to the prediction month. 

We derive a few new fields like *'Days since the last recharge'* and *'Percentage change between totol recharge amount on 8th month and the average of total recharge amounts between 6th and 7th months'*

For dimensioanlity reduction we use PCA, and on the components we try the following algorithms:

Logistic Regression

Random Forest

XGBoost

Since the data has a high class imbalance, we use SMOTE oversampling to correct the class imbalance. We use grid search for hyperparameter tuning for the models.

Since we want to capture all the churners (or as many as possible), we use *Recall* as the scoring metric for hyperparameter tuning.
###

# Conclusions:

###

AUC of Logistic Regression, Random Forest and XGBoost with SMOTE and PCA is quite comparable. Logistic Regression can be used if company wants to predict churner at the cost of Non-churners also being classified as churners as this model is giving high Recall but less precision.
Random Forest model is giving good Recall and Precision so we suggest this model for predictions.

**The most important predictor variables to predict churn**

Local incoming calls for 8th Month, decrease significantly for churners.

Total incoming minutes of usage change between good and action months, are significant to detect churners.

Total amount of recharge changes significantly for churners in action month.

Total recharge amount in action phase changes significantly for churners

Total outgoing minutes of usage drop significantly for churners.

###

