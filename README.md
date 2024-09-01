| [**Home page**](https://vks5639.github.io/My-Portfolio/) | [**Resume**](https://github.com/vks5639/My-Portfolio/blob/master/Vikash_Singh_Resume.pdf) | [**Portfolio Projects**](#portfolio-projects) | [**Study Projects**](#study-projects) | [**Certificates**](#certificates) | [**Contacts**](#contacts)

# Vikash Kumar Singh
## About me
<img src="https://i.imgur.com/n5Ez0vv.png" width="300" height="400"> 

Hello, I’m Vikash Kumar Singh. I graduated with a Master of Information Systems Management from Carnegie Mellon University. I am a passionate and results-driven Quality Assurance Engineer with a strong foundation in automation testing, backed by a Master’s degree in Information Systems Management from Carnegie Mellon University. I specialize in developing robust test frameworks and optimizing database performance, ensuring the reliability and efficiency of complex systems. With hands-on experience in tools like Selenium, TestNG, and various CI/CD pipelines, I am committed to delivering high-quality software solutions. Let's connect if you're looking for someone who can elevate your QA processes!

Skills:
- Programming: Java, Python, R, SQL, NoSQL, MongoDB, XML, HTML, JSON, Gherkin
- Analytics and Visualization: Pandas, NumPy, Matplotlib, Tableau, SAS, Stata
- Testing and Automation Tools: Selenium WebDriver, JUnit, TestNG, Appium, Cucumber, Rest-Assured, Jenkins, Maven, TestRail, Jira, Postman, SOAPUI, Sauce Labs, Page Object Model (POM), Behavior Driven Development (BDD)
- Development and Management Tools: Git, GitHub, Figma, Eclipse, IntelliJ
- Testing Expertise: Manual Testing, Automation Testing, Regression Testing, Performance Testing, API Testing, A/B Testing, Agile/Scrum

For more information please check my [**Resume**](https://github.com/vks5639/My-Portfolio/blob/master/Vikash_Singh_Resume.pdf).

This repository was created to showcase my analytical and technical skills (Excel, Python, SQL, Power BI, Tableau, and others).
## Contents
* [About me](#about-me)
* [Portfolio Projects](#portfolio-projects)
  - [API Testing using Postman]()
  - [Database Testing and Automation Project](https://github.com/vks5639/My-Portfolio/blob/master/Database_Testing.md)
     
* [Study Projects](#study-projects)
  - [Telling Stories With Data](#telling-stories-with-data)
  - [Window Functions SQL Analytics for Northwind Traders](#window-functions-sql-analytics-for-northwind-traders)
  - [Amazon Stock Market Analysis](#amazon-stock-market-analysis)
  - [Identifying Customers Likely to Churn for a Telecommunications Provider](#identifying-customers-likely-to-churn-for-a-telecommunications-provider)
  - [Analyzing COVID RNA Sequences](#analyzing-covid-rna-sequences)
  - [Finding Heavy Traffic Indicators on I-94](#finding-heavy-traffic-indicators-on-I-94)
  - [Analyzing Startup Fundraising Deals from Crunchbase](#analyzing-startup-fundraising-deals-from-crunchbase)

* [Certificates](#certificates)
* [Contacts](#contacts)

## Portfolio Projects
This section contains a list of projects with brief descriptions.

###  Database Testing and Automation Project
**Description:** This project involved comprehensive database testing using MySQL, focusing on schema validation, stored procedures, triggers, and automated testing scripts. The goal was to ensure the accuracy, performance, and reliability of database operations, specifically within a local development environment. <br>

**Technologies Used:** Technologies Used:
  * Validated the database schema, ensuring correct table presence, naming conventions, column count, data types, sizes, nullability, and keys.
  * TestNG: Used for automating the testing process, writing and executing test cases.
  * Java (JDBC): For database connectivity and executing SQL commands from Java applications.
  * Maven: Managed dependencies and build automation for the project
    
**Key Features and Components:**
  ### Schema Testing
  * Stored Procedures: Developed and tested various stored procedures like SelectAllCustomers, SelectAllCustomersByCity, and GetCustomerShipping, which performed specific tasks and returned multiple results.
  * Stored Functions: Created reusable SQL functions such as CustomerLevel for calculating and returning values based on specific business logic.

  ### Stored Procedures and Functions
  * MySQL: Database management for creating and managing databases, tables, stored procedures, and triggers.
  * TestNG: Used for automating the testing process, writing and executing test cases.
  * Java (JDBC): For database connectivity and executing SQL commands from Java applications.
  * Maven: Managed dependencies and build automation for the project

  ### Trigger Implementation
  * Implemented triggers to automate data integrity checks and business logic execution upon insertions, updates, and deletions in the database.
  * BEFORE INSERT Trigger: Ensured data consistency in WorkCenterStats by updating total capacity before inserting into WorkCenters.
  * AFTER UPDATE Trigger: Logged changes in sales data in SalesChanges table whenever an update occurred in the sales table.

  ### Automated Testing
  * TestNG Framework: Created automated test scripts for verifying the functionality of stored procedures, functions, and triggers. This included:
  * Testing the existence of stored procedures.
  * Comparing results from stored procedures against direct SQL queries to validate consistency.
  * Verifying triggers' execution by simulating database events and checking expected outcomes.
  
**Outcome:** Successfully automated the testing process for database operations, leading to a more efficient development cycle and reliable database performance. The implementation of triggers and stored procedures enhanced the security, maintainability, and modularity of the database system. <br>

**Skills Gained:**
* Advanced SQL programming and database management.
* Automated testing with TestNG.
* Java JDBC integration for database operations.
* Maven project management and dependency handling.


## Study Projects
### Telling Stories With Data
**Website:** [Telling Stories With Data](https://vks5639.github.io/TSWD-Portfolio/) <br>
**Description:** Welcome to my "Telling Stories With Data" portfolio from Carnegie Mellon University, where I transform complex datasets into clear, engaging narratives through innovative visualizations. My work, including a comprehensive final project, demonstrates my ability to reveal insightful stories hidden within data. If my approach resonates with you, I'd be delighted to explore how my skills can contribute to your team. <br>
**Skills:** Python, Pandas, NumPy, Matplotlib, Tableau, Data Analysis, Data Visualization <br>
**Status:** Completed in 2024

### Window Functions SQL Analytics for Northwind Traders
**Description:** Suppose, I am a Data Analyst at Northwind Traders, an international gourmet food distributor. Management is looking to me for insights to make strategic decisions in several aspects of the business. The projects focus on:

Evaluating employee performance to boost productivity,
Understanding product sales and category performance to optimize inventory and marketing strategies,
Analyzing sales growth to identify trends, monitor company progress, and make more accurate forecasts,
And evaluating customer purchase behavior to target high-value customers with promotional incentives.

Using the PostgreSQL window functions on the Northwind database, I will provide these essential insights to management, contributing significantly to the company's strategic decisions. <br>
**Code:** [Northwind](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/northwind.ipynb) <br>
**Original dataset:** [Northwind Database](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/northwind.sql) <br>
**Skills:** Advanced SQL and Database Management, Analytical and Critical Thinking, Data Manipulation and Analysis
, Performance Metrics and KPI Development, Sales and Marketing Analytics, Trend Identification and Forecasting, Strategic Decision-Making <be>

### Amazon Stock Market Analysis
**Description:** In the business world, there are few places that generate more daily data than the stock market. Analysts can use this data to explore and explain the past as well as provide insights about the future. But where does one start? Using data visualizations to communicate knowledge and information to make wise data-driven decisions is a valuable skill for any business professional.

In today's business landscape, the stock market churns out massive data daily. My project focuses on mastering data visualization to dissect this information effectively. By creating concise visual representations, I aim to uncover valuable insights for informed decision-making in the dynamic world of finance. <br>
**Report:** [Final Report](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/Amazon%20Stock%20Market%20Analysis.xlsx) <br>
**Original dataset:** [Amazon Stock Market Data](https://www.kaggle.com/datasets/varpit94/amazon-stock-data?resource=download) <br>
**Skills:** Excel, Analytical and Critical Thinking, Data Manipulation and Analysis
, Data Visualization, Marketing Analytics, Trend Identification <be>

### Identifying Customers Likely to Churn for a Telecommunications Provider
**Description:** In this project, I will:

•	Explore the different types of descriptive statistics.

•	Learn how to identify the appropriate statistic to use in various scenarios.

•	Apply these statistics to analyze real-world data.

•	Enhance and support the analysis using suitable visualizations.

Specifically, I will work with data from a telecommunications provider to identify customers likely to churn. Through detailed exploration and analysis, I will use statistical methods and visual tools to uncover insights about customer behavior.
Churn Rate=(Number of customers who churned/Total number of customers)∗100
The above metric can present us with a good overview of the percentage of user churn. As a business, of course, the goal is to minimize churn. <br>
**Report:** [Final Report](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/telcom_customer_data.xlsx) <br>
**Original dataset:** [Customer Churn Prediction](https://www.kaggle.com/c/customer-churn-prediction-2020/data) <br>
**Skills:** Excel, Analytical and Critical Thinking, Data Manipulation and Analysis
, Data Visualization, Marketing Analytics, Trend Identification <br>

### Analyzing Retail Sales
**Description:** In this project, I acted as an analyst for a chain of retail stores to study sales performance over the past few years. I prepared and profiled the data using VLOOKUP() and other Excel functions, then used PivotTables to aggregate and reshape the data to answer key questions. I conducted variance and trend analyses to identify strong-performing years and categories, and utilized statistical methods to analyze average order values. Using What-If Analysis tools, I helped management plan for various scenarios. My findings revealed that Q4 is the most profitable period, prompting further investigation into specific categories and months driving these profits. The project concluded with a comprehensive report in Excel, providing actionable insights and recommendations for strategic planning.

**Report:** [Final Report](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/superstore2.xlsx) <br>
**Original dataset:** [Retail Sales](https://community.tableau.com/s/question/0D54T00000CWeX8SAL/sample-superstore-sales-excelxls) <br>
**Skills:** Data preparation and cleaning, Data analysis and profiling using PivotTables, Variance and trend analysis, Statistical analysis of KPIs, What-If scenario planning, Generating actionable insights, and Creating comprehensive Excel reports. <br>

### Analyzing COVID RNA Sequences
**Description:** In this project, I delve into the RNA sequences of COVID, focusing on two significant variants: Delta and Omicron. RNA, a vital nucleic acid, serves as the genetic blueprint for COVID, facilitating its cellular entry and replication. By leveraging data from the National Institutes of Health (NIH), I dissect the metadata for each COVID RNA sequence to unravel insights into these variants.<br>
**Code:** [covid_genome](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/covid_genome.ipynb) <br>
**Original dataset:** [ncbi_datasets](https://drive.google.com/file/d/1S2ZDjdRkY78kZxBtc9YNUh0mByTHXQ23/view) <br>
**Skills:** analytical thinking, data cleaning, data analysis, data vizualization, presentations<br>
**Hard skills:** MS PowerPoint, Python: Pandas, NumPy, Mathplotlib, Seaborn. <br>
**Results:** Identified exact mutation points by finding alignments and mismatches between any two RNA sequences. Color coding resulted in visually differentiating alignments from mismatches, such as insertions, deletions, and substitutions, making it easier to interpret and analyze genetic variations.

### Finding Heavy Traffic Indicators on I-94
**Description:** In this analysis, we'll examine data related to traffic heading west on the I-94 Interstate. Our objective is to identify several factors that contribute to congestion on I-94. Potential factors include weather conditions, time of day, and day of the week, among others. <br>
**Code:** [I-94 Traffic](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/i94traffic.ipynb) <br>
**Original dataset:** [Metro_Interstate_Traffic_Volume](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/Metro_Interstate_Traffic_Volume.csv) <br>
**Skills:** analytical thinking, data cleaning, data analysis, data vizualization.<br>
**Hard skills:** Excel, Pivot Tables, Formulas, Functions, Charts, Dashboards, Slices, Pivot Charts.<br>
**Results:** In this project, my aim was to identify indicators of heavy traffic on the I-94 Interstate highway. Through the analysis, I identified two main types of indicators:
* Time Indicators:
   * Traffic tends to be heavier during warm months (March–October) in contrast to cold months (November–February).
   * Heavy traffic is typically observed on business days compared to weekends.
   * Rush hours on business days usually occur around 7 AM and 4 PM.
* Weather Indicators:
   * Heavy traffic is associated with specific weather conditions, including shower snow, light rain, and snow, and proximity thunderstorms with drizzle.
   * These findings provide valuable insights into the factors influencing traffic patterns on the I-94 Interstate highway.
   * Understanding these indicators can aid in traffic management and infrastructure planning to improve overall transportation efficiency and safety. <br>

### Analyzing Startup Fundraising Deals from Crunchbase 
**Description:** As part of my project, I undertook an in-depth analysis of startup fundraising deals sourced from Crunchbase.com. Leveraging the techniques acquired in pandas, I thoroughly explored the dataset to unravel trends, patterns, and noteworthy observations within the realm of startup financing. This endeavor not only honed my skills in data analysis but also provided valuable insights into the dynamics of fundraising rounds in the startup ecosystem. <br>
**Code:** [crunchbase](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/crunchbase.ipynb) <br>
**Original dataset:** [crunchbase_investments](https://github.com/vks5639/My_Portfolio/blob/main/Portfolio%20Projects/crunchbase-investments.csv) <br>
**Skills:** exploratory analysis, analytical thinking, , data vizualization<br>
**Hard skills:** data cleaning, data analysis, Python, Pandas, SQL, Excel, Dashboards <br>
**Results:** I gained a comprehensive understanding of the startup investments dataset. This exploration paved the way for insightful analysis, where I extracted valuable insights into fundraising rounds and identified notable trends in startup financing.

## Certificates
* [Infosys Certified Selenium Advance Automation Tester](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Selenium%20Advance%20Tester.pdf) - Infosys
* [Infosys Certified Selenium Basic Automation Tester](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Selenium%20Basic.pdf) - Infosys
* [Infosys Certified DevOps Professional](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Devops%20Professional.pdf) - Infosys
* [Registered Product Owner](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Registered%20Product%20Owner-3988192.pdf) - Scrum Inc.
* [Agile Developer](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Agile%20Developer.pdf) - Infosys
* [Python Associate](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Python%20Associate.pdf) - Infosys
* [Tableau for Data Scientists](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/CertificateOfCompletion_Tableau%20for%20Data%20Scientists.pdf) - Linkedin Learning
* [Querying Databases with SQL and Python](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Vikash-Kumar-Singh--Querying-Databases-with-SQL-and-Python.pdf) - Dataquest
* [Introduction to Data Analysis in Excel](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Vikash-Kumar-Singh--Introduction-to-Data-Analysis-in-Excel.pdf) - Dataquest
* [Azure Data Factory](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Azure%20Data%20Factory.pdf) - Linkedin Learning
* [Preparing Data in Excel](https://github.com/vks5639/My-Portfolio/blob/master/Certifications/Vikash-Kumar-Singh--Preparing-Data-in-Excel.pdf) - Dataquest



## Contacts
* Linkedin: [https://www.linkedin.com/in/vikash-ks/](https://www.linkedin.com/in/vikash-ks/)
* Email: vikashsingh5639@gmail.com
* Phone: 412-844-0218
