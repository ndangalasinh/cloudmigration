# Data Engineering End to End Project: Data migration from on premises database to Cloud database.

## Tech Stach
- Azure Data Factory 
- Azure Data Lake Storage Gen 2
- Azure DataBricks
- Azure Synapse Analytics

## Overview
This is an end to end data engineering project in which the main objective is to transfer data from the on premise SQL database to a cloud database and use that data to generate a simple report.

It is fairly common for businesses to have on premise OLTP database that logs in business transactions as they happen, and then later move that information into a datawarehouse which the analytics team can use this information to generate reports and analysis. The data can be moved from the on premise database on regular cadance such as weekly or monthly depending on the needs of the company, after each movement the moved data can be wiped out of the onpremise database to reduce storage cost for the organization. This process can be automated and data engieers can be receiving alerts incase when the process has failed for their intervention.


This project simulates a scenario described above, where a business has on premise database and want to move this data into a cloud solution so that the datascientists and analysts will be able to make use of this data to generate reports and dashboards. In this project Microsoft Azure has been used to implement the cloud solution, data is being fetched from the on-prem database using Azure Data Factory, then the data is being transformed in different stages using Azure Databricks and last data is being visualized and analysed using Microsoft's Power Bi.

## Project's Main Components
To implement data migration from on-premises database to cloud using Microsoft Azure solution the following steps will be followed.

### Data Ingestion 
Azure Data Factory will ingest data into the Azure ecosystem by using the self-hosted integration runtime (SHIR) which will be installed on the server hosting the on premise database to scan data.

In this project we will be migrating one schema from the on premise database, hence all the tables in this schema will be exported and converted to parquet files. ADF will store those parquet files in a storage container named Bronze, which is in Azure Data Lake Storage Gen2.

### Data Transformation
Azure Databricks will be responsible to perform the data transformations needed after data has been ingested into the Azure Data Factory. These transformations are usually needed to make sure that data that is being stored into the datawarehouse is matching the required standards.

In this project the following transformations will be done on the data 
1. Silver transformations
    1. Reformat the columns containing date to remove time
    1. Change CamelCase column names into snake_case column names
1. Gold transformations
    1. Add the date when the data was ingested
    1. Add masking function for the columns that contain sensitive information

When data is ingested from the onpremise database, it will land on the Bronze storage container, then it will undergo the Silver Transformations after which it will be moved to the Silver storage container.

Data in the Silver storage container will then undergo Gold transformations and then the transformed data will be dumped into the Gold storage container.

 ### Data loading into Datawarehouse
 Azure Synapse Analytics is used to now load the transformed data that is in the Gold storage container into a datawarehouse. This Gold storage container now contains transformed data which contains a file for each table that existed on the on premise database, each of that file will be used to generate views in this datawarehouse.

 In since these veiws in the Datawarehouse are referencing the files, everytime we are loading new files in incremental load the views will be updated to contain the new data.

 Here is also the right place to have data modelled into the format or the structure that is needed in consumption, in this project data is moved in the same structure it came with but in some usage here is where you do all the join to model the data in the format that is needed.
 
 ### Data reporting in Power Bi
 The final part of this project is to use the Microsoft Power Bi, to generate reports based on the data in the Datawarehouse. 

 Here users such as analysts will be able to connect to the DataWarehouse In Azure Data Synapse Analytics and from there will have access to data needed to create reports.

## How to implement this project
### Requirements
### Step by Step implementation


