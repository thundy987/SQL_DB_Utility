# SQL_DB_Utility

As a SOLIDWORKS PDM Data Management Specialist, I am regularly checking the health of customer's environments, which includes checking their database health and performance. SOLIDWORKS has a Status Report tool that is helpful in this task, but there's a couple things missing from it and I wanted to know more about how it might pull its' information from the SQL Server instance.

This tool is a result of me wanting to practice and improve my T-SQL skills by creating something that would be useful for an administrator of SQL databases. It's a diagnostic set of SQL stored procedures that detect performance issues, maintenance needs, and potential risk factors, and logs them to a central reporting database where results are timestamped and have a unique primary key.

---

## Technical Challenges and Lessons

<details>
<summary><strong>1. DMVs and Table-Valued Functions</strong></summary>

Querying and combining DMVs was new territory. Some information lived at the database level and some at the instance level, which required learning about `CROSS APPLY` and `OUTER APPLY` instead of standard joins - a pattern I hadn't used before.

</details>

<details>
<summary><strong>2. Dynamic SQL and WHILE Loops</strong></summary>

To report at the instance level across all databases, I needed to iterate query logic. Since `USE @DbName` isn't valid, I used a `WHILE` loop with Dynamic SQL to pass database names into an executed SQL string. I looped over database name rather than `db_id` to avoid gaps from deleted databases. Learning about SQL injection and writing safe Dynamic SQL was the biggest challenge of this project.

</details>

<details>
<summary><strong>3. Living in the MS Docs</strong></summary>

As my first SQL project, I had many documentation tabs open constantly for concepts and syntax lookups.

</details>

<details>
<summary><strong>4. Incremental Building</strong></summary>

I learned to start with base joins and raw columns, then add column-level aggregations, unit conversions, and casts. Realizing that "oh crap", I can't reference column aliases in `GROUP BY` led me to break logic out into CTEs.

</details>

<details>
<summary><strong>5. Plain English First</strong></summary>

My biggest takeaway: writing down exactly what I want in plain English before writing a single line of T-SQL makes everything easier.

</details>

---

## Modules

| Module | Description |
|---|---|
| **Backup Status** | Checks backup health across all databases on the instance. Highlights last full/differential/log backup per database, time since each backup type, and an alert status flag. |
| **Disk Space Monitor** | Reports disk volume usage and individual database file sizes. Alerts when free space on a volume falls below defined thresholds. |
| **Index Fragmentation Monitor** | Reports fragmentation levels for all indexes across all databases on the instance (optionally including system databases) and recommends a maintenance action based on fragmentation thresholds. |
| **Missing Index Monitor** | Identifies indexes SQL Server recommends based on query execution patterns captured since the last service restart. Dynamically builds a `CREATE INDEX` statement for each suggestion. |
| **Long Running Queries** | Identifies currently executing queries that exceed defined elapsed time thresholds. Returns session details, resource consumption, and the associated SQL text to aid in performance troubleshooting. |
| **Statistics Health Check** | Evaluates statistics staleness across all databases on the instance. Provides a modification percentage to highlight the most out-of-date tables. Also flags statistics that have never been computed. |

### Screenshots

<img width="336" height="621" alt="{FEBF9966-1B1E-4F10-B250-1257A3AD7E03}" src="https://github.com/user-attachments/assets/daf736c5-65fc-41b5-a96d-3a2f1053ee16" />

---

## Setup Instructions

1. Download the stored procedures.
2. Create a new, empty database with a name of your choice.
3. Save/create the stored procedures to that database.
4. Open the **Sample Report Generator** query and modify the values as you see fit.
5. Run the query against your new database *(note: it will create tables in whichever database you are working in)*.
6. Once your preferred thresholds are dialed in, use the **SQL Agent Weekly Job Setup Script** to schedule this tool to run weekly. Configure your preferences at the top of the script.

---

## Environment

- Written using **SQL Server 2025 Standard (Developer)** on **Windows Server 2025**

**Tested on:**
- AdventureWorks2025
- AdventureWorksDW2025
- TSQLDEMO *(SolidWorks' test database for learning PDM queries)*
- WideWorldImporters
- Two production PDM customer databases

---

## AI Usage Disclaimer

- Claude was used as a mentor, not a code generator.
- All code was written by me.
- It was instructed to teach by asking leading questions rather than providing answers and always pushed me to reason through problems and consult the MS docs directly.

---

## Potential Improvements

> This tool was not designed to replace established SQL monitoring platforms like SolarWinds, but some notable gaps include:

- Alerting mechanisms for detected issues
- SQL Agent Job monitoring (failed jobs, long-running jobs, etc.)
- Compatibility testing across additional SQL Server versions
