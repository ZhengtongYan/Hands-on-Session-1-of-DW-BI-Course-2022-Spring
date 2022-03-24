# **Hands-on Session 1: DW&BI Demo System With PostgreSQL and Penhato**
This repository contains the instructions of the hands-on sessions of the DW&amp;BI course.

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

### **Part1: DW&BI project with Pentaho**
 
**1.ETL processing**

Use PDI to create a star schema and store the tables (fact and dimension tables) in PostgreSQL

(1) Create a star schema and then output these tables in PostgreSQL. **(2 points)**
 - 1 fact table (sales_fact) 
 - 5 dimension tables (customer_dim, date_dim, product_dim, salesrep_dim, promotion_dim)
 - 2 measures (dollars_sold, amount_sold)

**Tips:** Create individual transformation for each table, and create a job to execute all the transformations with condition checks and validations (e.g., check whether the file exists)

(2) Discover slowly changing dimension (SCD) type 1 and 2. 
- Open up the customers_export.csv file
- Find customer 146 Elia Fawcett and change her address from "8989 N Port Washington Rd" to "8990 S Port Washington Rd";
- Add a new customer Jim Green with the following information:
"788,Jim,Green,198 Fairlawn Ln.,55401,Minneapolis,MN,US,United States of America,2,us,AMERICA,1100,samuel.green@someexample.host.com,+1 443 512 1121,,145,2001,8307,-93.26806,44.984747"
- Re-run the transformation.

Show the different results of customer_dim table when using SCD1 and SCD2, and explain the use cases of those two types. **(2 points)**


**Tips**: You may refer to the following wikipedia page:
https://en.wikipedia.org/wiki/Slowly_changing_dimension



**2.OLAP Cube Generation and MDX Query**

(1) Use Pentaho Schema Workbench to generate an OLAP cube based on the star schema created in the previous step. This cube should be created on two measures in the five dimension tables. And you should create hierarchies (e.g., year-month-day) on the dimensions. **(1 points)**



(2) Execute MDX queries to query the OLAP cube **(1 points)**
- Analyze the two measures in 


**Tips:**
 Please refer to the wikipedia page about MDX query language
https://en.wikipedia.org/wiki/MultiDimensional_eXpressions

**3.OLAP Analysis and Results Reporting**

Use the Saiku analytics tool to create a analysis report using the OLAP cube created in the previous step. Create a report of the analysis results **(1 points)**



**Submission Requirements**: 
- Please upload all the files of ETL processing, OLAP cube generation.
- Please show the star schema figure and give 10 rows of each table. 
- Post the MDX queries and demonstrate the results
- Please upload all the results in a single PDF file and then compress all the files into a zip file.


### **Part2: OLAP analysis with SQL query**
Rank (1 points)
Cube (2 points)
Grouping set (1 points)
Rollup (1 points)
Drilldown (1 points)
Slice (1 points)
Dice (1 points)




## Some useful links
**1. Pentaho documentation:**
https://help.hitachivantara.com/Documentation/Pentaho/9.2

**2. Multidimensional Data Modeling in Pentaho**
https://help.hitachivantara.com/Documentation/Pentaho/9.2/Work_with_data/Multidimensional_Data_Modeling_in_Pentaho  

**3. How to Design a Mondrian Schema**
http://www-master.ufr-info-p6.jussieu.fr/2009/Ext/naacke/mondrian/doc/schema.html#Star_schemas

