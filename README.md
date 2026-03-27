# SQL_DB_Utility
### A set of SQL stored procedures that detect performance issues, maintenance needs, and potential risk factors, and logs them to a central reporting database.

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

## Setup Instructions
- Download the stored procedures
- Create a new, empty database with a name of your choice
- Save/create the stored procedures to that database
- Open the Sample Report Generator query and modify the values as you see fit.
- Run the query against your new database (be advised that it will create tables in whatever db you are working in).


