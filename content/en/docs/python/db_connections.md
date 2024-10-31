---
title: Database Connections
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

Most modern applications contain some sort of database back-end.  In general, users never directly interact with the database.  Instead, the database is accessed indirectly through an external application that handles connections and logic.

![Files](images/connect_database3.jpg)

## Structured Query Language
Structured Query Language (SQL) is a data-specific language used in programming and designed for managing data held in a database management system (DBMS).  As a programming language, SQL is very powerful when used for searching or manipulating data.  While most DBMSs adhere to the ANSI SQL standards, there are varying dialects of SQL based on the organization creating the DBMS.  Some DBMS platforms you may encounter:

- Oracle
- SQL Server
- MySQL
- PostgreSQL
- MongoDB
- SQLite

SQL statements can range from a very simple statement such as the example in code listing~\ref{code:basic_sql} to a more advanced example such as code listing~\ref{code:advanced_sql}

\begin{codelisting}
\label{code:basic_sql}
\codecaption{Basic SQL statement}
```sql
SELECT * FROM employees;
```
\end{codelisting}


\begin{codelisting}
\label{code:advanced_sql}
\codecaption{Advanced SQL statement}
```sql
SELECT
  e.employee_id AS "Employee #"
  , e.first_name || ' ' || e.last_name AS "Name"
  , e.email AS "Email"
  , e.phone_number AS "Phone"
  , TO_CHAR(e.hire_date, 'MM/DD/YYYY') AS "Hire Date"
  , TO_CHAR(e.salary, 'L99G999D99', 'NLS_NUMERIC_CHARACTERS = ''.,
    '' NLS_CURRENCY = ''$''') AS "Salary"
  , e.commission_pct AS "Comission %"
  , 'works as ' || j.job_title || ' in ' || d.department_name
    || ' department (manager: ' || dm.first_name || ' ' || dm.last_name
    || ') and immediate supervisor: ' || m.first_name || ' '
    || m.last_name AS "Current Job"
  , TO_CHAR(j.min_salary, 'L99G999D99', 'NLS_NUMERIC_CHARACTERS = ''.,
    '' NLS_CURRENCY = ''$''') || ' - ' ||
      TO_CHAR(j.max_salary, 'L99G999D99', 'NLS_NUMERIC_CHARACTERS = ''.,
        '' NLS_CURRENCY = ''$''') AS "Current Salary"
  , l.street_address || ', ' || l.postal_code || ', ' || l.city || ', '
    || l.state_province || ', '
    || c.country_name || ' (' || r.region_name || ')' AS "Location"
  , jh.job_id AS "History Job ID"
  , 'worked from ' || TO_CHAR(jh.start_date, 'MM/DD/YYYY') || ' to '
    || TO_CHAR(jh.end_date, 'MM/DD/YYYY') ||
    ' as ' || jj.job_title || ' in ' || dd.department_name
    || ' department' AS "History Job Title"
    FROM employees e
    -- to get title of current job_id
      JOIN jobs j
        ON e.job_id = j.job_id
    -- to get name of current manager_id
      LEFT JOIN employees m
        ON e.manager_id = m.employee_id
    -- to get name of current department_id
      LEFT JOIN departments d
        ON d.department_id = e.department_id
    -- to get name of manager of current department
    -- (not equal to current manager and can be equal to the employee itself)
      LEFT JOIN employees dm
        ON d.manager_id = dm.employee_id
    -- to get name of location
      LEFT JOIN locations l
        ON d.location_id = l.location_id
      LEFT JOIN countries c
        ON l.country_id = c.country_id
      LEFT JOIN regions r
        ON c.region_id = r.region_id
    -- to get job history of employee
      LEFT JOIN job_history jh
      ON e.employee_id = jh.employee_id
    -- to get title of job history job_id
    LEFT JOIN jobs jj
      ON jj.job_id = jh.job_id
    -- to get namee of department from job history
    LEFT JOIN departments dd
      ON dd.department_id = jh.department_id

ORDER BY e.employee_id;
```
\end{codelisting}

Being familiar with multiple programming languages is a good thing and I would encourage interested programmers to learn as much SQL as they feel comfortable learning.  Learning some SQL will make application developers better at thinking about data and interacting with a DBMS.

The downside is that maintaining a proficiency in multiple languages can be difficult...especially, in a fast changing sector!

Luckily, we are part of an open source community that seeks to improve our abilities.

## SQLAlchemy
SQLAlchemy is a Python package that provides a nice “Pythonic” way of interacting with databases. So rather than dealing with the differences between specific dialects of traditional SQL, you can leverage the Pythonic framework of SQLAlchemy to streamline your workflow and more efficiently query your data.

The SQLAlchemy package may need to be installed before use.  Using the Python package manager **pip**, type ```pip install sqlalchemy``` in your terminal.
```
$ pip install sqlalchemy
Collecting sqlalchemy
  Downloading https://files.pythonhosted.org/packages/25/c9/b0552098cee325425a61efd
  f380c51b5c721e459081c85bbb860f501c091/SQLAlchemy-1.2.12.tar.gz (5.6MB)
    100% |████████████████████████████████| 5.6MB 226kB/s
Building wheels for collected packages: sqlalchemy
  Running setup.py bdist_wheel for sqlalchemy ... done
  Stored in directory: /Users/james/Library/Caches/pip/wheels/ed/bd/2e/d3874a6e97b8
  cc71e7e177c8d065ead30f67f380c4d9bbadaa
Successfully built sqlalchemy
Installing collected packages: sqlalchemy
Successfully installed sqlalchemy-1.2.12

```

SQLAlchemy includes the dialects for more popular DBMS platforms and has extensions for some of the more obscure databases.

## Connecting to a database
To start interacting with the database we first we need to establish a connection.

\begin{codelisting}
\label{code:sqlalchemy_engine_example}
\codecaption{Example Connection}
```python
import sqlalchemy as db
engine = db.create_engine('dialect+driver://user:pass@host:port/db')
```
\end{codelisting}

## Viewing Table Details
SQLAlchemy can be used to automatically load tables from a database using something called reflection. Reflection is the process of reading the database and building the metadata based on that information.

\begin{codelisting}
\label{code:view_table_details}
\codecaption{census\_metadata.py}
```python
import sqlalchemy as db

engine = db.create_engine('sqlite:///census.sqlite')
connection = engine.connect()
metadata = db.MetaData()
census = db.Table('census', metadata, autoload=True, autoload_with=engine)

print(census.columns.keys())

print(repr(metadata.tables['census']))
```
\end{codelisting}

## Querying
SLQAlchemy supports querying of data from a DBMS.  If needed or desired, you can open a connection and use SQL for the query.

\begin{codelisting}
\label{code:query_sql}
\codecaption{census\_sql.py}
```python
import sqlalchemy as db

engine = db.create_engine('sqlite:///census.sqlite')
connection = engine.connect()
results = connection.execute("select * from census")
for row in results:
    print(row)

connection.close()
```
\end{codelisting}

A better solution would be to allow SQLAlchemy to handle the SQL statement.
\begin{codelisting}
\label{code:query_results_set}
\codecaption{census\_query.py}
```python
import sqlalchemy as db

engine = db.create_engine('sqlite:///census.sqlite')
connection = engine.connect()
metadata = db.MetaData()
census = db.Table('census', metadata, autoload=True, autoload_with=engine)

# Equivalent to 'SELECT * FROM census'
query = db.select([census])

proxy = connection.execute(query)
print(proxy)

result = proxy.fetchall()

print(result[:10])
```
\end{codelisting}

**proxy**: The object returned by the .execute() method. It can be used in a variety of ways to get the data returned by the query.

**results**: The actual data asked for in the query when using a fetch method such as *.fetchall()* on a ResultProxy.

### Dealing with Large ResultSet

On occasion, you may encounter very large tables. SQLAlchemy has *.fetchmany()* to load optimal no of rows and overcome memory issues in case of large datasets

## Filter
Data can be filtered.

\begin{codelisting}
\label{code:query_filter}
\codecaption{census\_filter.py}
```python
import sqlalchemy as db

engine = db.create_engine('sqlite:///census.sqlite')
connection = engine.connect()
metadata = db.MetaData()
census = db.Table('census', metadata, autoload=True, autoload_with=engine)

# Equivalent to:
# SELECT * FROM census WHERE sex = F
#query = db.select([census]).where(census.columns.sex == 'F')

# Equivalent to:
# SELECT state, sex FROM census WHERE state IN (Texas, New York)
query = db.select([census.columns.state, census.columns.sex])
        .where(census.columns.state.in_(['Texas', 'New York']))

proxy = connection.execute(query)
print(proxy)

result = proxy.fetchall()

print(result[:10])
```
\end{codelisting}

## Join
If you have two tables that already have an established relationship, you can automatically use that relationship by just adding the columns we want from each table to the select statement.

\begin{codelisting}
\label{code:query_join}
\codecaption{census\_join.py}
```python
import sqlalchemy as db

engine = db.create_engine('sqlite:///census.sqlite')
connection = engine.connect()
metadata = db.MetaData()
census = db.Table('census', metadata, autoload=True, autoload_with=engine)
state_fact = db.Table('state_fact', metadata, autoload=True, autoload_with=engine)

query = db.select([census.columns.pop2008, state_fact.columns.abbreviation])
result = connection.execute(query).fetchall()

print(result[:10])
```
\end{codelisting}
