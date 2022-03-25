# **Hands-on Session 1: DW&BI Demo System With PostgreSQL and Penhato**
**This repository contains the instructions of the first hands-on session in our DW&BI course.**

## **Learning Objectives**
Gain hands-on experience in building a DW&BI project and conducting OLAP analysis using PostgreSQL and Pentaho.
- Learn how to create star schema with fact tables and dimension tables using Pentaho (PDI);
- Learn how to create OLAP cube using Pentaho Schema Workbench (PSW);
- Learn how to conduct OLAP analysis using Saiku Analytics tool in Pentaho Server;
- Learn how to conduct OLAP analysis with SQL in PostgreSQL. 


## **Software Requirements**
**1. Install PostgreSQL database**

* Install PostgreSQL on Windows: 
https://www.postgresqltutorial.com/install-postgresql/
* Install PostgreSQL on Linux: 
https://www.postgresqltutorial.com/install-postgresql-linux/
* Install PostgreSQL on MacOS: 
https://www.postgresqltutorial.com/install-postgresql-macos/

**2. Install the community edition of Pentaho**
- 2.1 Pentaho Server
- 2.2 Pentaho Data Integration (PDI)
- 2.3 Pentaho Schema Workbench (PSW)

Please refer to the two pdf documents about downloading and installing Pentaho software in the Installation Guide folder:
- Pentaho Installation Guide.pdf
- Pentaho Installation Problems.pdf

I have recorded a video to introduce how to download and install the software: https://www.youtube.com/watch?v=--afzrAZjyc



## **Exercises (20 points)**

Use the sales dataset in this project. You can download the data files form the Data Source folder. the same example dataset to complete the following exercises.

### **Part1: DW&BI project with Pentaho (8 points)**
 
**1.ETL processing**

Use PDI to create a star schema and store the tables (fact and dimension tables) in PostgreSQL

(1) Create a star schema and then output these tables in PostgreSQL. **(2 points)**

The star schema includes:
 - a fact table (*sales_fact*) 
 - five dimension tables (*customer_dim, date_dim, product_dim, salesrep_dim, promotion_dim*)
 - two measures (*dollars_sold, amount_sold*)

Requirments in the date_dim table:
Please embellish the Date Dimension with the following date attributes:
-  *sales_year*: Year of date 
- *sales_quarter*: Quarter of date
- *sales_month*: Month of date
- *sales_day_of_year*: Day of year of date 
- *sales_day_of_month*: Day of month of date 
- *sales_day_of_week*: Day of week of date 
- *sales_week_of_year*: Week of year of date 
- *sales_week_of _quarter*: Week of quarter of date



**Tips:** Create individual transformation for each table, and create a job to execute all the transformations with condition checks and validations (e.g., check whether the file exists)

(2) Discover different slowly changing dimension (SCD) types. **(2 points)**
- Open up the customers_export.csv file
- Find customer 146 Elia Fawcett and change her address from "8989 N Port Washington Rd" to "8990 S Port Washington Rd";
- Add a new customer Jim Green with the following information:
"788,Jim,Green,198 Fairlawn Ln.,55401,Minneapolis,MN,US,United States of America,2,us,AMERICA,1100,samuel.green@someexample.host.com,+1 443 512 1121,,145,2001,8307,-93.26806,44.984747"
- Re-run the transformation.

Show the different results of customer_dim table when using SCD1 and SCD2, and explain the use cases of those two types. 


**Tips**: You may refer to the following wikipedia page about SCD types:
https://en.wikipedia.org/wiki/Slowly_changing_dimension



**2.OLAP Cube Generation and MDX Query**

(1) Use Pentaho Schema Workbench to generate an OLAP cube based on the star schema created in the previous step. This cube should be created on two measures in the five dimension tables. And you should create hierarchies (e.g., year-month-day) on the dimensions. **(2 points)**


(2) Execute MDX queries in Schema Workbench to analyze the OLAP cube **(1 points)**
- Search the two measures of in the date and customer dimensions with dimensional hierarchies, including year-month-day hierarchy in date_dim and country-city hierarchy in customer_dim. 
- Search the two measures of each product_id in each month of 2006. 


**Tips:**
 Please refer to the wikipedia page about MDX query language
https://en.wikipedia.org/wiki/MultiDimensional_eXpressions

**3. OLAP Analysis and Results Reporting**

Use the Saiku analytics tool to create a analysis report using the OLAP cube created in the previous step. **(0.5 points)**

Then, create a report to visualize the analysis results with charts (e.g., line chart, pie chart, and bar chart). **(0.5 points)**



**Submission Requirements**: 
- Please upload all the files of ETL processing, OLAP cube generation.
- Please show the star schema figure and give 10 rows of each table. 
- Post the MDX queries and demonstrate the results
- Please upload all the results in a single PDF file and then compress all the files into a zip file.


### **Part2: OLAP analysis with SQL query (Total: 12 points)**
In Part 1, we focus on how to building a DW&BI application with PostgreSQL and Pentaho using the MOALP model. In part 2, we will utilize a different approach to conduct the OALP analysis with a realtional databases, which is called as ROLAP modeling method.

**1. Full Star Join** 

Create a full star join by joining the fact table and dimension
tables.

(1) Please use three differnet syntax for join  **(1.5 points)**

(2) What are the surrogate key and business key in those tables? Why use the surrogate key in star schema? **(0.5 points)**


**2. Top-N Query**

Find the top-3 customers who come from USA and spend the highest amount of money in the year of 2006.

Please use two different methods:
- Use GROUP BY, ORDER BY, and LIMIT Operators **(1 points)**
- Use windows function (e.g., RANK) **(1 points)**

**3.Cube Creation**

(1) Build a OLAP cube for *sales_year* (in date_dim table) and *product_status* (in product_dim table) dimensions to calculate the total amount. **(1 points)**

(2) How many combinations of the two dimension attributes are created in the cube? Based on these combinantions of dimension attributes, create the same OLAP cube using UNION function instead of using CUBE keywords. **(1 points)**

(3) Replace the CUBE keywords to ROLLUP and GROUPING SET, compare and exaplain the results? **(1 points)** 


**4.Pivot Table (1 points)**

(1) First, calculate the total dollars sold per city of USA and per month in 2006. 

(2) Then, use the crosstab function to create a pivot table view to show the results.



**5.Roll-up Operation (1 points)**

(1) Build a OLAP cube for month, city, and product category dimensions to calculate the total amount.

(2) Roll up the results on month and city.

**6.Drill-down Operation(1 points)**

(1) Build a OLAP cube for year, promotion name, and product category dimensions to calculate the total amount.

(2) Drill down the results on year and product category.


**7.Slice Operation (1 points)**

(1) Build a OLAP cube for salesrep, quarter, and product category dimensions to calculate the total amount.

(2) Slicing the data cube on the first quarter in 2006.


**8.Dice Operation (1 points)**

(1) Build a OLAP cube for promotion name, city, and month dimensions to calculate the total amount.

(2) Dicing the data cube in two dimensions: city (Madison and Indianapolis) and month (January and Novemeber).



## **Some Useful Links**
**1. Pentaho documentation:**
https://help.hitachivantara.com/Documentation/Pentaho/9.2

**2. Multidimensional Data Modeling in Pentaho**
https://help.hitachivantara.com/Documentation/Pentaho/9.2/Work_with_data/Multidimensional_Data_Modeling_in_Pentaho  

**3. How to Design a Mondrian Schema**
http://www-master.ufr-info-p6.jussieu.fr/2009/Ext/naacke/mondrian/doc/schema.html#Star_schemas

**4. Some Tutorial Videos for Pentaho**

- Understanding Pentaho Data Integration(PDI) | Pentaho Data Integration Tutorial | Edureka https://www.youtube.com/watch?v=J8NbYQaQiPo&t=4660s
- Using Pentaho Schema Workbench https://www.youtube.com/watch?v=Tqw3oOk5jsM&list=PLIS-R80eiu1snl5wW893-BLiE0yDVhQAe


**5. GROUP BY, GROUPING SETS, CUBE, ROLLUP, and Window Function Processing in PostgreSQL**

https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-GROUPING-SETS

**6.Crosstab for Pivot Table in PostgreSQL**
https://www.postgresql.org/docs/14/tablefunc.html
