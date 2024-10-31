---
title: How To Approach an Assignment
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

![Map](images/elephant.svg)

> I don't know where to start.
>  - Anonymous ISDS student

Question: How do you eat an elephant?

Answer: One bite at a time.

Eating an elephant, or anything for that matter, can only be done one way: one bite at a time.  We use the analogy of eating an elephant to illustrate the fact that some projects look really big and overwhelming.  Intellectually, the best way to accomplish something big is to approach it in smaller pieces.  This chapter will attempt to suggest a roadmap for completing assignments.

Recommended steps:

1. View the data (optional).
2. Connect to the data source.
3. Pull data from the data source.
4. Wrangle the data.
5. Output information.
6. Properly close any open connections or files.

## View the Data
If possible, it is always nice to view your data at rest.  Expect to be given data in many different formats.  Some possible data files are xslx, csv, txt, or json.  A spreadsheet application such as MS Excel can be used to view xslx and csv files.  Your favorite code editor can also be used to view csv, txt, and json files.  Looking at the raw data is a habit and allows you to start forming opinions about the quality of the data.

![csv](images/csv_file.png)

In the recommended steps above, viewing the data is marked as 'optional' because there may be situations where you can't view the raw data.  If the data is storied in a database, you will likely need a special application or SQL query viewer.  Opening the files from a database management system with an inappropriate application could corrupt the files.

## Connect to the Data Source
One of the great aspects of a popular language such as Python is that there is likely a package for connecting to any data source.  For ASCII text files, you can use the built-in 'open' function.  While they are text files, the community has created csv and json packages for handling these special types of files.

Likewise, there are multiple packages for connecting to most DBMSs.  This book will discuss SQLAlchemy and Pandas but other packages are available.

## Pull Data
(coming soon!)

## Wrangle Data
(coming soon!)

## Output Information
(coming soon!)

## Properly Close Connections or Files
(coming soon!)
