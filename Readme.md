![LifeLogo](https://github.com/cam48eco/LifeDWH/blob/main/img/LIFE.png)

# ETL solution for supporting sensors data transformation 

## Motivation 
The motivation to prepare the solution was a need to facilitate the data flow and ensuring consolidated access to data neccessary for physical analyses of water from  Pilica catchment area.


## Assumptions
The point of departure for solution preparation was a statement, that the solution should be deployed in a way, assuming usage of on-premise infrastructure, including an server working on local machine (e.g. laptop) connected to web, due to lack of financial resources for estabilishment and maitenance of external server or cloud solutions. In result, free versions of software (SSMS Express) have to be used, limiting the access to useful features, in particular, automated task schedulling in database itself. 
The solution' architecture follows these limitations, and in consequence tasks, automatizing data flows, were implemented with in-house T-SQL scripts / Airflow/python DAGs.  
The added value of the solution is its scalability and adaptability, enabling after minor changes, preparation a similar solutions in on-premise servers or (with more effort) in the cloud. Disregard on above mentioned limits, some of cloud solutions have been also discussed and implemented (in particular for visualization), to take the adventages from such kind of technologies as Tableau and Looker. 

## Stack

- Windows 10
- Google Drive
- SQL Server 2017 
- SSMS 17.9.1
- Airflow under Debian (in Win10 environment; setup see [info](https://www.freecodecamp.org/news/install-apache-airflow-on-windows-without-docker/))

## Scope and architecure design
The repository documents the steps and include resources used for solution creation and set up of data warehouse (under SQL Server 2017) and its maitenance with orchestration tool (Airflow).
The following issues are covered:
- I. Data sources identification, mapping, preprocessing and storage 
- II. ETL and Data warehouse design and implementation
- III. Automation of the ETL process with Airflow.

Fig. 1. Brief presentation of the pipeline / ETL architecture assumptions
![LifeBPM](https://github.com/cam48eco/LifeDWH/blob/main/img/diagram_Airflow.svg)

Source: Author own elaboration 
 


## I. Data sources identification, mapping, initial preprocessing and storage 

According to project assumptions, the data sources includes:  
- data from sensors mounted on buoys, 
- data from Hydrowskaz. 

After sources identyfication, all data included in the sources, have been investigated to map their structure. The mapping results were included in the publicly avaiable file on Google Drive:
[(see here)](https://drive.google.com/file/d/1Y7GSUoNxyXYVJ8wsZpzKDmYjiW3U6H-p/view?usp=drive_link), as 'sensors_observations'

In all of above mentioned cases, it was neccessary to transform the data from the original formats into csv's with following common data structure:  
(TBC)   

In result .csv files have been created and filled with data (depending on the data source, this process was more or less automated). The initial source' data catalogue includes: 
(TBC)


All above mentioned .csv files are stored on Google Drive, publicly available [(see here)](XX), as 'sensors_observations'. 

Data are reviewed and updated periodically. 
In case the new observations are detected by research team in existing data sources, the relevant .csv file is updated in the repository. 
In case if the new flat data source with new data would appear, the new .csv file would be created and pushed into the repository.
Primarily, the repository has been stored on github, but eventually, the Google Drive has been choosen as a storage. 


## II. ETL and Data warehouse design and implementation


### 2.1. Preparatory tasks  

#### 2.1.1. First data transformation 

TBC

 
#### 2.1.2. Creation and feeding of 'sources database' for storing 'transactional' data 

TBC

**'Sources database' creation**

The process has been developed in SSMS (see below).

![OltpLogo](https://github.com/cam48eco/LifeDWH/blob/main/img/CreateOLTP.png)

In addition, apart from the default SQL Server .dbo schema (for 'observations' tables), an additional schema .dim (for 'community' table) has been created. 

![OltpLogo](https://github.com/cam48eco/LifeDWH/blob/main/img/CreateSchema.png)



**'Sources database' tables creation and data fetching throghout migration from .csvs** 

TBC


#### 2.1.3. 'Staging database' and its tables creation with fetching with data transformed from 'Sources database' 

TBC 

## III. Dimensions tables and Fact table 

The last stage was focused on preparation and feeding of dimensions tables and first fact table (for population on catchment area)  on the basis of data stored in staging database, and to ensure that these processes will be repeated cyclicaly, each time when previous task (C5 - feeding staging database) is completed. The core of the solution was the preparation of relevant DAGs (C6 for dimensions tables and C7 for fact table, respectivelly)  along with accompanying T-SQL scrpits: 
- script for dimension tables creation and feed [C6](https://github.com/cam48eco/LifeDWH/blob/main/dags/C6_Create_Feed_DimTables.sql)
- script for fact table creation and feed [C7](https://github.com/cam48eco/LifeDWH/blob/main/dags/C7_Create_Feed_FactTables.sql).

