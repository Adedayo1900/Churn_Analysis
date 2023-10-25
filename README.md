# Churn_Analysis
This Project is a Churn Analysis for United Bank For Europe.

![WhatsApp Image 2023-10-25 at 02 41 18](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/3bf5c80b-adb2-4c1b-a4cd-c8ef6e57ede4)

# Overview
United Bank for Europe (UBE) is a top bank with presence in Europe. The organisation values customer retention and seeks to establish reasons for loss of customers. The banking industry is very competitive and it is generaly more expensive to to find new customers than to keep existing customers. High churn rate means losing customes and thereby translate to losing revenue. The bank would like to identify core reasons why already existing customers stop doing business with them.

# Business Question
Identify key reason for churn rate.

![WhatsApp Image 2023-10-25 at 02 41 24](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/2227715a-2b93-429b-9d7b-9e58dba4ff86)

# Available Data
The Data given consist of UBE's customer base, age, location and whether they have exited the bank or not.

DATASET: [Customer-Churn-Records.csv](https://github.com/Adedayo1900/Churn_Analysis/files/13059105/Customer-Churn-Records.csv)

The Data contains 10000 rows and 19 columns.
Most of this columns are straightforard and easy to understand however, columns like

'IsActiveMember' represent customers who are active and had performed a transaction on their account(s) in the past one year.

'Exited' represent customers who had closed their account(s).

'Complain' represent those that had complained about their account(s) or services rendered to them.

Columns such as 'IsActiveMember', 'Complain' and 'Exited' had two unique values ('1', '0') to represent ('Yes','No') respectively

# Analysis
# Tool used for this analysis
- SQL
- Power BI

# Hypothesis
- I hypothesized that tenure had an effect on churn rate and that older customers (longer tenure) were less likely to leave with the assumption that they did be loyal to the brand.
- I hypothesized that satisfaction_score had an effect on churn rate and those that rate and gave the bank a higher satisfaction score will be less likely to exit or stop doing business with the bank
- I hypothesized that number of products had an effect on churn rate and those who used more products will feel some sort of loyalty to the brand.
- I hypothesized that being an active member had an effect on churn rate and that those that werent active might have loss interest in the bank and might have started banking with competition and would have churned.
- I hypothesized that customers who have complained are more likely to churn if complains are not well handled.

# Data Anaysis
First, i needed to know the number of customers, number of churn (customers who have exited the business) and churn_rate (number of churns/ number of customers). 

###Number_of_customer###
```
SELECT COUNT(customerid) AS total_customer
FROM churn
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/fc8f83de-64e5-4aa0-ab95-e72e7241d799)

The data contains the record of 10000 customers.

###Number_of_churn###
```
SELECT COUNT(exited) AS no_of_churn
FROM churn
WHERE exited = 1
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/73ea876c-0e69-4c74-9e40-019df8b6120a)

Out of 10000 record, 2038 customers had exited the bank and have churned. This represents a high number of churn and is not good for the bank. 

###Churn_rate###
```
SELECT CAST(COUNT(exited)*100 AS float) /(SELECT COUNT(customerid) AS total_customer
FROM churn) AS Churn_rate
FROM churn
WHERE exited = 1
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/9f1049ce-e781-409b-bef3-b490c19e2a7f)

This gives a churn rate of 20.38%, derived by diving Number of churn by total customers and multiplying by 100.
## Analyse churn rate by tenure

```
WITH a AS (SELECT tenure, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY tenure),

b AS (SELECT tenure, COUNT(customerid) AS total
From churn
GROUP BY tenure)

SELECT a.tenure AS tenures, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.tenure = b.tenure
ORDER BY a.tenure
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/02d113ba-1230-4e67-8e7b-5d31027a5808)

Upon analysis, i noticed that there is no remarkable difference between churn rate by tenure. Hence, tenure has no effect on whether a customer will exit the bank.
## Analyse churn rate by Satisfation score
```
WITH a AS (SELECT sat_score, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY sat_score),

b AS (SELECT sat_score, COUNT(customerid) AS total
From churn
GROUP BY sat_score)

SELECT a.sat_score AS satisfactory_score, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.sat_score = b.sat_score
ORDER BY satisfactory_score ASC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/b1c96f53-eddc-4355-81c9-05de59763211)

Also, one would have assumed that customers who gave the bank a low satisfaction rating would be more likely to exit the bank, but from the analysis above, there is no remarkable difference in churn rate by customer satisfactory score.
## Analyse churn rate by Card_type
```
WITH a AS (SELECT card_type, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY card_type),

b AS (SELECT card_type, COUNT(customerid) AS total
From churn
GROUP BY card_type)

SELECT a.card_type AS card_types, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.card_type = b.card_type
ORDER BY churn_rate DESC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/16050558-1e4e-4756-8061-340f94f5dbcf)

There is also no remarkable difference in churn rate by card types, signifying that card_type isnt the reason for the high churn rate
## Analyse churn rate by Number of products used by customer
```
WITH a AS (SELECT num_of_product, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY num_of_product),

b AS (SELECT num_of_product, COUNT(customerid) AS total
From churn
GROUP BY num_of_product)

SELECT a.num_of_product AS num_of_products, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.num_of_product = b.num_of_product
ORDER BY num_of_products DESC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/6903a711-2727-4720-9f84-a0a9dd6db6b3)

There is significant difference in churn rate by number of products. Those who use higher number of products rendered by the bank (3-4 products) were more likely to exit the bank. 
## Analyse churn rate by Active_members
```
WITH a AS (SELECT active_memeber, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY active_memeber),

b AS (SELECT active_memeber, COUNT(customerid) AS total
From churn
GROUP BY active_memeber)

SELECT a.active_memeber AS Active_members, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.active_memeber = b.active_memeber
ORDER BY Active_members ASC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/263b4e07-5585-4f20-884b-0d1580b53a56)

The output above shows that non_active members were more likely to exit the bank. 
## Analyse churn rate by complainers
```
WITH a AS (SELECT complained, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY complained),

b AS (SELECT complained, COUNT(customerid) AS total
From churn
GROUP BY complained)

SELECT a.complained AS complainers, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.complained = b.complained
ORDER BY complainers ASC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/fce75d99-6f18-4dec-8e6c-2f1bdf37a0f3)

99.5% of customers who raised complains about services rendered exited the bank.
## Analyse churn rate by Active_members and complainers
```
WITH a AS (SELECT active_memeber, complained, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY active_memeber, complained),

b AS (SELECT active_memeber, complained, COUNT(customerid) AS total
From churn
GROUP BY active_memeber, complained)

SELECT b.active_memeber, b.complained, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
FULL JOIN b
ON a.active_memeber = b.active_memeber
AND a.complained = b.complained
ORDER BY churn_rate ASC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/f46a0c17-65d3-4733-82c8-6900c7b230fd)

Further analysis showed that active customers with no complain do not exit the bank, however, active and non-active customers who have raised a complain at some point were very likely to exit the bank, as 99% of them exited the bank.
## Analyse churn rate by location
```
WITH a AS (SELECT location, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY location),

b AS (SELECT location, COUNT(customerid) AS total
From churn
GROUP BY location)

SELECT a.location AS locations, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
INNER JOIN b
ON a.location = b.location
ORDER BY churn_rate DESC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/aaee56ce-9d83-4bf0-a8a3-82c9ee2ecc04)

Churn_rate was higher in Germany compared to other locations.
## Analyse churn rate by Age group
```
WITH a AS (SELECT trunc(age, -1) AS Age_bins, COUNT(exited) AS churn_number
From churn
WHERE exited= 1
GROUP BY trunc(age, -1)),

b AS (SELECT trunc(age, -1) AS Age_bins, COUNT(customerid) AS total
From churn
GROUP BY trunc(age, -1))

SELECT b.Age_bins AS Age_groups, (CAST(a.churn_number AS FLOAT)/b.total)*100 AS churn_rate
FROM a
FULL JOIN b
ON a.Age_bins = b.Age_bins
ORDER BY Age_groups ASC
```
![image](https://github.com/Adedayo1900/Churn_Analysis/assets/130705411/7c5e2d5c-752e-4d81-8687-8f7f4f679370)

Age was binned into age grades, with Ages

10-19 represented as 10

20-29 represented as 20

30-39 represented as 30

40-49 represented as 40

50-59 represented as 50

60-69 represented as 60

70-79 represented as 70

80-89 represented as 80

90-99 represented as 90
Churn_rate was higher in older adults with the age of 50-59 (56%), 60-69 (35.2%) and 40-49 (30.8%).

# Recommendation
Based on the analysis above, we can conclude that 99.5% of those that complained about services rendered exited the bank. Also further anlysis showed that out of the 2038 customers that exited the bank, 2034 had once complained. Complaint serves as a form of feedback in business. The banking industry is very complex and is usually riddled with lots of complain by customers, however, what matters to customers is how well their complains are handled and resolved.  This analysis exposes that the bank doesn't handle customers complain well and complaints are not well resolved. The bank needs to put effort into complaint handling and resolution so as to reduce churn. The analysis showed that all active customers that have not raised a complain have not exited the bank, however 99% of active and non-active customer who once complained have left the bank.

Also, for a bank who hasnt quite got its complaint resolution well, customers with high number of products would most likely exit the bank as they might have more complains (while using the different products) which will not be resolved. This might serve as the reason for high churn rate amongst customers who use more number of products.

The bank also needs to focus on its older adult customers between the ages of 40-69 as they have been shown to exit the bank than any other age group. The bank should try to introduce more valuable products targeted at this age group to increase their loyalty to the brand.

Lastly there is a need to focus on the branches in germany as customers in germany are two times more likely to exit the bank than those in other locations as revealed by the analysis. 
