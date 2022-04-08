# **Hands-on Session 1: DW&BI Project and OLAP Analysis With PostgreSQL and Pentaho**
**This repository contains the instructions of the first hands-on session in our DW&BI course.**

## **Recorded Video Links**
- [Software Installation Guide for Hands-on Session 1 of DW&BI Course](https://www.youtube.com/watch?v=--afzrAZjyc)
- [Hands-on Session 1 of Data Warehousing and Business Intelligence Course (Part 1)](https://www.youtube.com/watch?v=6YB-arWROGQ)
- [Hands-on Session 1 of Data Warehousing and Business Intelligence Course (Part 2)](https://www.youtube.com/watch?v=ndRR0tN5LqI)


## **Learning Objectives**
Gain hands-on experience in building a DW&BI project and conducting OLAP analysis using PostgreSQL and Pentaho.
- Learn how to create star schema with fact tables and dimension tables using Pentaho Date Integration (PDI);
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
- [Pentaho Installation Guide.pdf](https://github.com/ZhengtongYan/Hands-on-Session-1-of-DW-BI-Course/blob/main/Installation%20Guide/Pentaho%20Installation%20Guide.pdf)
- [Pentaho Installation Problems.pdf](https://github.com/ZhengtongYan/Hands-on-Session-1-of-DW-BI-Course/blob/main/Installation%20Guide/Pentaho%20Installation%20Problems.pdf)

**Many students said that they have problems with installing and running the PDI software in macOS. You can refer to the following articles if you use macOS:**
- [How I setup my Pentaho Data Integration (PDI) and solving the problem on running it on Mac OSX Catalina 10.15.4](https://medium.com/@gembit.soultan/how-i-setup-my-pentaho-data-integration-pdi-and-solving-the-problem-on-running-it-on-mac-osx-6f0cc7f3b97c)
- [ETL Tool: Install pentaho 9.0 on mac (2020)](https://gingkolane.medium.com/install-pentaho8-3-on-mac-on-2-1-2020-69ca6e7b24c5)
- [Pentaho Data Integration not starting on new Mac M1](https://stackoverflow.com/questions/67972804/pentaho-data-integration-not-starting-on-new-mac-m1)



## **Exercises (20 points)**

Download the following data files from the [Data_Sources](https://github.com/ZhengtongYan/Hands-on-Session-1-of-DW-BI-Course/tree/main/Data_Sources) folder and use them to complete the exercises.

- **customers_export.csv** - Customers information 
- **products_export.csv** - Products information
- **promotions_export.csv** - Promotions information
- **salesrep_export.csv** -  Employees (sales representatives) information
- **orders_export.csv**	 - Order items information

### **Part1: DW&BI Project with Pentaho (9 points)**
 
**1.ETL processing**

Use PDI to create a star schema and store the tables (fact and dimension tables) in PostgreSQL

(1) Create a star schema and then output these tables in PostgreSQL. **(4 points)**

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



**Tip 1:** Create individual transformation for each table, and create a job to execute all the transformations with condition checks and validations (e.g., check whether the file exists)

**Tip 2:** For the creation of *sales_week_of _quarter* dimension, one possible way is to add a "Formula" step (in the "Scripting" folder) after the "Calculate Additional Fields" step. You can use the "sales_week_of_year" field to calculate the new "sales_week_of_quarter" field.

(2) Discover different slowly changing dimension (SCD) types. **(2 points)**

In the "dimension lookup/update" step, you can set different SCD types to handle the future changes with different ways in the dimension tables. The default type of dimension update is "Insert", i.e., SCD type 2.  
- Step 1: Open the customers_export.csv file;
- Step 2: Find customer 146 Elia Fawcett and change her address from "8989 N Port Washington Rd" to "8990 S Port Washington Rd";
- Step 3: Re-run the transformation, see what happens in the dimension table;
- Step 4: Clear the customer_dim table, and restore the changes in the customers_export.csv file;
- Step 5: Change the type of dimension update from "Insert" to "Punch through' (i.e., SCD type 1);
- Step 6: Repeat step 1 to step 3.
  
Compare the different results of customer_dim table when using SCD type 1 and SCD type 2, and explain the use cases of those two types. 


**Tips**: You may refer to the following wikipedia page about SCD types:
https://en.wikipedia.org/wiki/Slowly_changing_dimension



**2.OLAP Cube Generation**

Use Pentaho Schema Workbench to generate an OLAP cube based on the star schema created in the previous step. This cube should be created on two measures in the five dimension tables. You should create some hierarchies (e.g., year-month-day) on the dimensions. Finally, publish the cube into Pentaho server for further analysis. **(2 points)**


**3. OLAP Analysis and Results Reporting**

(1) Use the Saiku Analytics tool to create an analysis report using the OLAP cube created in the previous step. **(0.5 points)**

(2) Then, create a report to visualize the analysis results with charts (e.g., line chart, pie chart, and bar chart). **(0.5 points)**



### **Part2: OLAP analysis with SQL query (Total: 11 points)**
In Part 1, we focus on how to building a DW&BI application with PostgreSQL and Pentaho using the MOLAP model. In part 2, we will utilize a different approach to conduct the OLAP analysis only with PostgreSQL on the star schema.

**1. Full Star Join** 

Create a full star join by joining the fact table and dimension tables.

(1) Please use three different syntax for join operation **(0.5 points)**

- WHERE clause, e.g., WHERE T1.id=T2.id
- INNER JOIN operation, e.g., T1 INNER JOIN T2 ON (T1.id=T2.id)
- USING keyword, e.g., T1 INNER JOIN T2 USING (id)

(2) What are the surrogate key and business key in those tables? Why use the surrogate key in star schema? **(0.5 points)**


**2. Top-N Query**

Find the top-3 customers who come from USA and spend the highest amount of money in the year of 2006. Return the customer_id and total amount of money. 

Please use two different methods:
- Use GROUP BY, ORDER BY, and LIMIT Operators **(1 point)**
- Use windows function **(1 point)**

**3.Cube Creation**

(1) Build an OLAP cube for *sales_year* (in date_dim table) and *product_status* (in product_dim table) dimensions to calculate the total amount. Sort the resluts by sales_year and product_status with in **ascending order**. **(1 point)**

(2) How many combinations of the two dimension attributes are created in the cube? Based on these combinantions of dimension attributes, create the same OLAP cube onlu using **UNION** and **GROUP BY** keywords instead of using CUBE keyword. Comapre and ensure that you can get the same results of using CUBE keyword. Sort the resluts by sales_year and product_status with in **ascending order** **(1 point)**
 

(3) Replace the CUBE keyword with ROLLUP and GROUPING SETS keywords respectively, compare and exaplain these results? **(1 points)** 


**4.Pivot Table (1 point)**

First, calculate the total dollars sold per city of USA and per month in 2006. Then, use the crosstab function to create a pivot table view to show the results.

**Tips:** Please refer to the documentaion about crosstab function and pivot table:

- [**Crosstab for Pivot Table in PostgreSQL**](https://www.postgresql.org/docs/14/tablefunc.html)
- [**Pivot table in Wikipedia**](https://en.wikipedia.org/wiki/Pivot_table)


**5.Roll-up Operation (1 point)**

(1) Build an OLAP cube to calculate the total amount per month, per year, per city, per product category, and per parent category.

(2) Roll up the results on two dimensions: first roll-up from month to year and second roll-up from product category to parent category.

**Tips**: Roll-up means that you need to remove some attribute in the GROUP BY clause.

**Notice:**: Here the *roll-up operation* is a kind of OLAP operation which is not same the the *ROLLUP function* in SQL.

**6.Drill-down Operation(1 point)**

(1) Build an OLAP cube to calculate the total amount per country, per promotion name, and per product category.

(2) Drill down the results on two dimensions: first drill-down from country to city and second drill-down from product category to sub-category.

**Tips**: Drill-down means that you want to obtain more details on some dimensions, so you need to add some attribute in the GROUP BY clause.


**7.Slice Operation (1 point)**

(1) Build an OLAP cube for salesrep's department, quarter, and product category dimensions to calculate the total dollars sold.

(2) Slicing the data cube on the first quarter in 2006.


**8.Dice Operation (1 point)**

(1) Build an OLAP cube for promotion name, city, and month dimensions to calculate the total dollars sold.

(2) Dicing the data cube in two dimensions: city (Madison and Indianapolis) and month (January and Novemeber).


## **Submission Requirements**: 
- Please upload all the files of ETL processing, OLAP cube generation, etc.
- Please show the star schema figure and give 10 rows of each table. 
- Post the queries and demonstrate the results.
- Please upload all the results in a single PDF file and then compress all the files into a zip file.
- Please submit to Moodle page and the deadline is **April 14th, 2022**.


## **Some Useful Links**
[**1. Pentaho documentation**](https://help.hitachivantara.com/Documentation/Pentaho/9.2)

[**2. Multidimensional Data Modeling in Pentaho**](https://help.hitachivantara.com/Documentation/Pentaho/9.2/Work_with_data/Multidimensional_Data_Modeling_in_Pentaho )

[**3. How to Design a Mondrian Schema**](http://www-master.ufr-info-p6.jussieu.fr/2009/Ext/naacke/mondrian/doc/schema.html#Star_schemas)

[**4. Understanding Pentaho Data Integration(PDI)**](https://www.youtube.com/watch?v=J8NbYQaQiPo&t=4660s)

[**5. Using Pentaho Schema Workbench**](https://www.youtube.com/watch?v=Tqw3oOk5jsM&list=PLIS-R80eiu1snl5wW893-BLiE0yDVhQAe)

[**6. GROUPING SETS, CUBE, ROLLUP, and Window Function in PostgreSQL**](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-GROUPING-SETS)

