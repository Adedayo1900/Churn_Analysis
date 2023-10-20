# Churn_Analysis
This Project is a Churn Analysis for United Bank For Europe.

# Overview
United Bank for Europe (UBE) is a top bank with presence in Europe. The organisation values customer retention and seeks to establish reasons for loss of customers. The banking industry is very competitive and it is generaly more expensive to to find new customers than to keep existing customers. High churn rate means losing customes and thereby translate to losing revenue. The bank would like to identify core reasons why already existing customers stop doing business with them.

# Business Question
Identify key reason for churn rate.

# Available Data
The Data given consist of UBE's customer base, age, location and whether they have exited the bank or not.

DATASET: [Customer-Churn-Records.csv](https://github.com/Adedayo1900/Churn_Analysis/files/13059105/Customer-Churn-Records.csv)

The Data contains 10000 rows and 19 columns.
Most of this columns are straightforard and easy to understand however, columns like

'IsActiveMember' represent customers who are active and had performed a transaction on their account(s) in the past one year.
'Exited' represent customers who had closed their account(s).
'Complain' represent those that had complained about their account(s) or services rendered to them.

# Analysis
# Tool used for this analysis
- Power BI

# Data Cleaning
The first task was to clean the data. The data contained no missing rows as this was confirmed by checking the column profile using power Query.
Some of the columns such as 'IsActiveMember', 'Complain' and 'Exited' had two unique values ('1', '0') to represent ('Yes','No') respectively. However, because they contained numbers, they were automatically given an 'integer' datatype by Power BI, hence they were converted to 'text' data type and were also changed from '1' to 'Yes' and '0' to 'No' using the power query 'Replace values' command. 
Also, the age column comprises of several ages and for proper analysis, the age had to be grouped or binned. Hence i created a conditional column to help bin the ages and this gave rise to a column 'Age Bins' which was created with the code below.

Age bins= = Table.AddColumn(#"Removed Columns", "Custom", each if [Age] <= 20 then "11-20" else if [Age] <= 30 then "21-30" else if [Age] <= 40 then "31-40" else if [Age] <= 50 then "41-50" else if [Age] <= 60 then "51-60" else if [Age] <= 70 then "61-70" else if [Age] <= 80 then "71-80" else if [Age] <= 90 then "81-90" else "91-100")

Now, my data was ready for analysis.

# Data Anaysis
First, i needed to know the number of customers, number of churn (customers who have exited the business) and churn_rate (number of churns/ number of customers). To get this, i used the DAX New Measure command with the codes below

Number_of_customer = DISTINCTCOUNT('Customer-Churn-Records'[CustomerId])

Number_of_churn = CALCULATE(DISTINCTCOUNT('Customer-Churn-Records'[CustomerId]), 'Customer-Churn-Records'[Exited]= "Yes")

Churn_rate = [Number_of_churn]/[Number_of_customer]

Second, 

