# Introduction to Data Engineering

The first online business application was an inventory system.

It allowed telephone ordering of catering supplies to a national UK coffee shop chain. The system allowed analysis of sales traffic, supply chain logistics and predictions of business performance. To achieve this service, the Lyons Tea Company built their own [computer](<https://en.wikipedia.org/wiki/LEO_(computer)>), software and call centre - in 1951.

The importance of business data - and its volume - has only increased since then.

The web, the mobile web, high speed mobile data (well... _ish_) have meant that more data is created, by more business processes, more of the time.

_Data Engineering_ is a field within Software Engineering that allows us to work with such data, mining it for business insights. The kinds of application that provide such insights are termed _OLAP_ applications.

## OLAP and OLTP - Analysing transactions

Data Analysis applications are termed [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) applications, for _OnLine Analyical Processing_.

### OLAP Applications

OLAP applications work with large volumes of historic - and often real-time - transactions data, which they organise and enhance to create business level reports.

OLAP applications answer questions like:

- What was the best selling product in our Asia stores between Jan 2021-Dec 2024?
- Who are our top three customers by spend in each of our sales regions?
- Are sales of pencils seasonally affected?

OLAP applications often combine data from multiple sources in multiple formats, and enhance it with reference data. A lot of datasets can be bought these days. Sources of data include Sales accounts, customer loyalty cards, online clickstreams and more.

The organisation of data for an OLAP application needs to optimise these kinds of queries. Reports and queries must be answered quickly, despite the huge volumes of data involved.

### OLTP Applications

An OLTP application is some kind of automated business process. Standing for _Online Transaction Processing_, OLTP is the familar "Click to Buy Now" application we use every day.

OLTP can also refer to back-office systems, like stock control and employee management. Nor do they have to be real-time; an OLTP application can easily use _batch processing_, where processing happens infrequently - like at the end of the day.

### Different purposes, different data

[OLTP](https://en.wikipedia.org/wiki/Online_transaction_processing) applications require data organised for supporting individual transactions. That's rather different from OLAP needs.

> OLAP optimises data for aggregate reporting
>
> OLTP optimises data for individual transactions

## Business Insights

Data is worthless until we use it to improve our business in some way. That's the idea behind business insights, and we saw some examples above. How much? Who to? Where from? Best? Worst? These are all queries that help a business learn what's working and what needs to pivot.

Without data, those decisions are (at best) educated guesses. With data, we still need to analyse.

There are many kinds of analyses we can do. Two common ones are time-based and aggregate queries.

### Time-based reports

Time-based queries answer questions about when something happened. Did we sell more of X this time last year? Or at Christmas? Is product X tailing off? Should we introduce its upgrade now or later?

### Aggregates

Aggregate queries reveal the big picture. They are questions like _across all products_ how much profit did we make? Or who were our _top five_ sales staff _last quarter_?

OLTP transactional data applies to individual events, and is not well suited to these kinds of queries. Over this course, we will see how we can transform OLTP data into better structures for OLAP reporting.

# Managing big data is hard

Technical considerations come into play. It is one thing to summarise ten rows of a spreadsheet. It is quite another to summarise every Amazon order (for example) over the last quarter.

Big Data deserves its name when you look at the amount of disk space, RAM system memory and processor power it requires to handle it.

## Overview of tools

We will be taking advantage of the tools offered by Databricks Data Lakehouse and Apache Spark to help us make large volumes of data manageable.

The three services we will use in this course are:

- **Microsoft Azure** Cloud native data storage and computing resource
- **Apache Spark** Code to wrangle big data and transform it
- **Databricks Data Lakehouse** Givernance and perfromance technical features

The following topis are not limited to those three tools. Instead, they are fundamental approaches to transforming and organising data for OLAP reporting.

# Next

[Medallion Architecture](/medallion-architecture.md)

[Back to Contents](/contents.md)
