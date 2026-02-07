# U.S. Airline Operational Performance Analysis (2022–2024)

## Course
MSDS 420 – Database Systems  
Northwestern University  
Instructor: Dr. Abid Ali

## Team
Sara Alsiyat  
Prathamesh Nehete  
Yedong Deng  

## Project Overview
This project analyzes the operational performance of U.S. airlines using publicly available data from the U.S. Department of Transportation’s Bureau of Transportation Statistics (BTS) for the period 2022–2024. The analysis focuses on flight delays, cancellations, and operational efficiency across airlines, airports, and time periods.

The goal of the project is to demonstrate how structured database design, data modeling, and analytical querying can be used to support operational and strategic decision-making. The project follows a full database systems lifecycle, including staging, normalization, data warehousing, and dimensional modeling.

## Business Questions
The project addresses questions such as:
- What are the main causes of flight delays and cancellations across U.S. airlines?
- Which delays are operationally controllable versus uncontrollable?
- How do delay patterns vary by airport, season, and time of day?
- How does operational performance relate to customer complaints and financial trends?

## Data Sources
This project uses five public datasets from U.S. federal agencies:
1. BTS On-Time Performance Data (primary dataset)
2. BTS Airport Reference and Traffic Data
3. BTS Air Carrier Financial Reports (Form 41)
4. DOT Air Travel Consumer Complaint Data
5. NOAA Historical Weather Data (contextual)

Large raw data files are stored externally. Download links and instructions are documented in docs/data_download_guide.md and docs/data_links.md.

## Project Structure
data/        Raw and reference datasets (external links)
docs/        Documentation and data download guides
eda/         Exploratory data analysis notebooks
staging/     Staging databases (raw and cleaned)
dw/          3NF data warehouse schema
marts/       Dimensional models (star schema)
models/      ER diagrams and data models
etl/         Data loading and transformation scripts
sql/         DDL, queries, and validation scripts

## Database Design Approach
The project follows these design steps:
1. Staging Layer  
   - Staging 1: Raw data loaded as-is  
   - Staging 2: Cleaned and standardized data  

2. Data Warehouse (3NF)  
   - Fully normalized schema with primary and foreign keys  
   - Designed to reduce redundancy and ensure data integrity  

3. Data Marts (Dimensional Model)  
   - Star schema with fact tables and dimension tables  
   - Supports slice-and-dice analysis and visualization  
   - Includes slowly changing dimensions and defined hierarchies  

4. Source-to-Target Mapping (STTM)  
   - Documents how each source field maps to warehouse and mart tables  

## Tools and Technologies
Database: PostgreSQL or MySQL  
Query Language: SQL  
Data Processing: Python (pandas)  
EDA and Documentation: Jupyter Notebooks  
Version Control: GitHub  

## Notes
No automation pipelines are used, per project requirements.  
This repository focuses on database design, modeling, and analysis rather than application development.
