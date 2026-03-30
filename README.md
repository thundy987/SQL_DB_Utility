# SQL_DB_Utility
As a SOLIDWORKS PDM Data Management Specialist, I am regularly checking the health of customer's environments, which includes checking their database health and performance. SOLIDWORKS has a Status Report tool that has been very helpful in this task, but there's a couple things missing from it and I wanted to know more about how it might pull its' information from the SQL Server instance.

This tool is a result of me wanting to practice and improve my T-SQL skills by creating something that would be useful for an administrator of SQL databases. It's a diagnostic set of SQL stored procedures that detect performance issues, maintenance needs, and potential risk factors, and logs them to a central reporting database.
### Key Challenges
1. Learning how to query and combine DMVS was interesting because I had never used a table value function in a join before. Some of the information I needed lived at the db level and some at the instance level which led to the next challenge. This forced me to learn about Cross and Outer Apply statements instead of regular joins.
2. I was determined to make all of the modules report at an instance level for all dbs, but this meant that I had to iterate my query logic somehow. I ended up using a WHILE loop with Dynamic SQL (because I couldn't just say something like "USE @DbName) because this let me pass in variables to the executed sql string. I looped over db name instead of db_id to avoid issues with gaps from deleted dbs on the instance. Learning about SQL injection and how to write Dynamic SQL was the biggest challenge in this project and I look forward to practicing it in other projects to cement my understanding of it.
3. As my first project creating something in SQL, I felt like I LIVED in the MS docs. Constantly having many tabs open to different concepts and syntax lookups.
4. I quickly to build up incrementally. Start with base joins and pulling the raw columns I need. Then start doing column-level aggregrations as needed, converting/casting units and data types. Realizing that oh crap, I can't reference that column alias in my GROUP BY statement or something, and having to split out into a CTE (or multiple).
5. My biggest takeaway is that it always helps to write down exactly what I want or what I'm trying to do in plain English first before trying to write a single line of T-SQL.
## This utility offers the following modules:
### Backup Status
Checks backup health across all databases on the instance. Highlights last full/differential/log backup per database, time since each backup type, and an alert status flag.
### Disk Space Monitor
Reports disk volume usage and individual database file sizes. Alerts when free space on a volume falls below defined thresholds.
### Index Fragmentation Monitor
Reports fragmentation levels for all indexes across all databases on the instance (optionally including system databases) and recommends a maintenance action based on fragmentation thresholds.
### Missing Index Monitor
Identifies indexes SQL Server recommends based on query execution patterns captured since the last service restart. Dynamically builds a CREATE INDEX statement for each suggestion.
### Long Running Queries
Identifies currently executing queries that exceed defined elapsed time thresholds. Returns session details, resource consumption, and the associated SQL text to aid in performance troubleshooting.
### Statistics Health Check
Evaluates statistics staleness across all databases on the instance. Orders results by modification percentage to surface the most at-risk tables first. Also flags statistics that have never been computed.

### Screenshots
<img width="336" height="621" alt="{FEBF9966-1B1E-4F10-B250-1257A3AD7E03}" src="https://github.com/user-attachments/assets/daf736c5-65fc-41b5-a96d-3a2f1053ee16" />

## Setup Instructions
- Download the stored procedures
- Create a new, empty database with a name of your choice
- Save/create the stored procedures to that database
- Open the Sample Report Generator query and modify the values as you see fit.
- Run the query against your new database (be advised that it will create tables in whatever db you are working in).
- Once you have your preferred thresholds dialed in, there is also a SQL Agent Weekly Job Setup Script that you may use to run this tool weekly. Simply configure your preferences at the top.

## Additional Info
It was written using 2025 SQL Server Standard (Developer) on Windows Server 2025.

It was tested on the following databases:
- AdventureWorks2025
- AdventureWorksDW2025
- TSQLDEMO (Solidwork's test db for learning how to query the PDM database)
- WideWorldImporters
- Two real PDM customer databases I will not name.

## AI Usage Disclaimer
- I used Claude as a mentor for this project.
- It was forbidden from directly providing me with code. I had to write all code myself (it helped me write the opening comments on the first module I completed, I copied the formatting for the rest).
- I instructed Claude to act as a senior SQL Server and data engineer whose role is to teach me by nudging, not giving answers.
- Whenever I asked it a question, it would ask me leading questions, subtly pointing me where to look in the MS docs.
- It was my "rubber duck" to help reason through problems and it constantly challened my assertions to make sure I could explain my reasoning for decisions.
  
### Potential Project Improvements
This tool was not designed to replace established SQL monitoring platforms like SolarWinds, but some obvious gaps in its' capabilities include:
- Alerting mechanisms for issues
- Historical records. currently all the reporting tables are dropped and recreated every run instead of appending them for over-time analysis.
- SQL Agent Job monitoring to checked for failed or long-running jobs etc.
- Test this utility in other versions of SQL server to ensure compatibility.
