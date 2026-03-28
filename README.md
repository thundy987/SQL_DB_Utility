# SQL_DB_Utility
As a SOLIDWORKS PDM Data Management Specialist, I am regularly checking the health of customer's environments, which includes checking their database health and performance. SOLIDWORKS has a Status Report tool that has been very helpful in this task, but there's a couple things missing from it and I wanted to know more about how it might pull its' information from the SQL Server instance.

This tool is a result of me wanting to practice and improve my T-SQL skills by creating something that would be useful for an administrator of SQL database. It's a diagnostic set of SQL stored procedures that detect performance issues, maintenance needs, and potential risk factors, and logs them to a central reporting database.
### What I Learned
Oh boy.
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
It was written using 2025 SQL Server Standard (Developer) on Windows Server 2025 and still needs to be tested on other environments.

It was tested on the following databases:
- AdventureWorks2025
- AdventureWorksDW2025
- TSQLDEMO (Solidwork's test db for learning how to query the PDM database)
- WideWorldImporters
- Two real PDM customer databases I will not name. 

