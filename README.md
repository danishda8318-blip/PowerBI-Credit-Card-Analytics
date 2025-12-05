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

-- SQL Query to create and import data from csv files:

-- 0. Create a database 

CREATE DATABASE ccdb;


-- 1. Create cc_detail table

CREATE TABLE cc_detail (

    Client_Num INT,
    
    Card_Category VARCHAR(20),
    
    Annual_Fees INT,
    
    Activation_30_Days INT,
    
    Customer_Acq_Cost INT,
    
    Week_Start_Date DATE,
    
    Week_Num VARCHAR(20),
    
    Qtr VARCHAR(10),
    
    current_year INT,
    
    Credit_Limit DECIMAL(10,2),
    
    Total_Revolving_Bal INT,
    
    Total_Trans_Amt INT,
    
    Total_Trans_Ct INT,
    
    Avg_Utilization_Ratio DECIMAL(10,3),
    
    Use_Chip VARCHAR(10),
    
    Exp_Type VARCHAR(50),
    
    Interest_Earned DECIMAL(10,3),
    
    Delinquent_Acc VARCHAR(5)
    
);




-- 2. Create cc_detail table

CREATE TABLE cust_detail (

    Client_Num INT,
    
    Customer_Age INT,
    
    Gender VARCHAR(5),
    
    Dependent_Count INT,
    
    Education_Level VARCHAR(50),
    
    Marital_Status VARCHAR(20),
    
    State_cd VARCHAR(50),
    
    Zipcode VARCHAR(20),
    
    Car_Owner VARCHAR(5),
    
    House_Owner VARCHAR(5),
    
    Personal_Loan VARCHAR(5),
    
    Contact VARCHAR(50),
    
    Customer_Job VARCHAR(50),
    
    Income INT,
    
    Cust_Satisfaction_Score INT
    
);

select* from cust_detail

-- 3. Copy csv data into SQL (remember to update the file name and file location in below query)

-- copy cc_detail table

COPY cc_detail

FROM 'C:\credit_card.csv' 

DELIMITER ',' 

CSV HEADER;




select*from cc_detail

INSERT INTO cc_detail -- data inserted into cc_detail from credit_card

SELECT*FROM credit_card;








BULK INSERT cc_detail

FROM 'C:\credit_card.csv'

WITH (

    FIRSTROW = 2,              -- header skip
    
    FIELDTERMINATOR = ',',     -- column separator
    
    ROWTERMINATOR = '\n',      -- new line
    
    TABLOCK
    
);






-- copy cust_detail table

COPY cust_detail

FROM 'D:\customer.csv' 

DELIMITER ',' 

CSV HEADER;



select*from customer;

INSERT INTO cust_detail -- data inserted into cust_detail from customer

SELECT*FROM customer;

select*from cust_detail;


SELECT @@SERVERNAME;

-- If you are getting below error, then use the below point:  

   -- ERROR:  date/time field value out of range: "0"
   
   -- HINT:  Perhaps you need a different "datestyle" setting.
   

-- Check the Data in Your CSV File: Ensure date column values are formatted correctly and are in a valid format that

PostgreSQL can recognize (e.g., YYYY-MM-DD). And correct any incorrect or missing date values in the CSV file. 

   -- or
-- Update the Datestyle Setting: Set the datestyle explicitly for your session using the following command:

SET datestyle TO 'ISO, DMY';


-- Now, try to COPY the csv files!



-- 4. Insert additional data into SQL, using same COPY function


-- copy additional data (week-53) in cc_detail table


COPY cc_detail

FROM 'D:\cc_add.csv' 

DELIMITER ',' 

CSV HEADER;


select*from cc_detail

INSERT INTO cc_detail -- data inserted into cc_detail from credit_card

SELECT*FROM cc_add;






-- copy additional data (week-53) in cust_detail table (remember to update the file name and file location in below query)

COPY cust_detail

FROM 'D:\cust_add.csv' 

DELIMITER ',' 

CSV HEADER;

INSERT INTO cust_detail -- data inserted into cust_detail from customer

SELECT*FROM cust_add;



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


## Project Insights — Week 53 ##
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
