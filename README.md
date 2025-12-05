# PowerBI-Credit-Card-Analytics
 A Power BI dashboard built using SQL-based customer and transaction data to analyze weekly credit card performance metrics. Provides insights into revenue, transactions, customer segments, and delinquency trends.

## Credit Card Financial Dashboard
** Overview**

A Power BI dashboard built using SQL-based credit card transaction and customer data to analyze weekly performance metrics, revenue, customer insights, and delinquency trends.

## Project Objective

To build an interactive weekly dashboard that provides real-time insights into credit card revenue, transactions, customer segments, and KPIs.





## Dataset Information

Source: SQL database (imported from CSV files)

Tables: cust_detail, cc_detail



## Tools & Technologies Used

Power BI

Power Query

SQL Server/MS SQL

DAX Measures

Data Modelling

## Data Cleaning & Transformation 
Data type formatting

Calculated columns (Age Group, Income Group)

Applying business rules



## DAX Measures
DAX Measures Used

**1. Age Group Classification**
AgeGroup =

SWITCH (

    TRUE(),
    
    'public cust_detail'[customer_age] < 30, "20-30",
    
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    
    'public cust_detail'[customer_age] >= 60, "60+",
    
    "Unknown"
    
)

**2. Income Group Classification**
IncomeGroup =

SWITCH (

    TRUE(),
    
    'public cust_detail'[income] < 35000, "Low",
    
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Medium",
    
    'public cust_detail'[income] >= 70000, "High",
    
    "Unknown"
    
)

**3. Week Number Calculation**
week_num2 =

WEEKNUM ( 'public cc_detail'[week_start_date] )

**4. Revenue Calculation**
Revenue =

'public cc_detail'[annual_fees] +

'public cc_detail'[total_trans_amt] +

'public cc_detail'[interest_earned]

**5. Current Week Revenue**
Current_week_Reveneue =

CALCULATE (

    SUM ( 'public cc_detail'[Revenue] ),
    
    FILTER (
    
        ALL ( 'public cc_detail' ),
        
        'public cc_detail'[week_num2] = MAX ( 'public cc_detail'[week_num2] )
        
    )
    
)

**6. Previous Week Revenue**
Previous_week_Reveneue =

CALCULATE (

    SUM ( 'public cc_detail'[Revenue] ),
    
    FILTER (
    
        ALL ( 'public cc_detail' ),
        
        'public cc_detail'[week_num2] =
        
            MAX ( 'public cc_detail'[week_num2] ) - 1
            
    )
    
)

## Dashboard Pages
![Credit_Card_Report_page-0001](https://github.com/user-attachments/assets/fee5ad85-b5fc-4cc0-8b18-72f6ff2c8023)

![Credit_Card_Report_page-0002](https://github.com/user-attachments/assets/c96ec3cb-d257-4654-a825-5f31a86a4443)


**Project Insights — Week 53 **
## Week-over-Week (WoW) Changes
Revenue increased by 28.8%


Total transaction amount increased by 35%


Total transaction count increased by 3%


Customer count increased by 12%



## Year-to-Date (YTD) Overview
Total revenue: 56.5M


Total interest earned: 8M


Total transaction amount: 45.5M


Revenue contribution by gender:


Male: 30.9M


Female: 25.6M


Blue & Silver credit cards contribute 93% of all transactions


Top contributing states (68% total): Texas (TX), New York (NY), California (CA)


Activation rate: 57.5%


Delinquent rate: 6.06%
