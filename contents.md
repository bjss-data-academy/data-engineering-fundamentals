# Contents

## Introduction to Big Data

- [Introduction to Data Engineering](/introduction.md)
- Business Insights
- Examples
- Time periods: What happened when?
- Aggregation: How much overall?

- Managing big data is hard
- Example numbers
- Usable queries need to be fast

## Overview of tools

    - Apache Spark
    - Databricks
    - Azure cloud

## Medallion Architecture

- [Transforming raw data to insight](/medallion-architecture.md)
  - Transforming raw data to insight
  - Bronze – Silver – Gold
  - Ingest – Add value – Present
  - Introduce Spark DataFrame object – abstraction of column data
  - ??? Lab read a CSV and display
  - ??? Lab read a JSON and display

## Star Schema

- [Star Schema](/star-schema.md)

  - Invented 1990s Ralph Kimball
  - Example schema
    -- See ERD in Five minutes Primer link (prerequisite)

  - Facts
    -- Describe / Definition (measurement)
    -- Examples of facts in typical business

  - Dimensions
    -- Describe / Define
    --Examples of Dimensions
    -- Date and date ranges (buckets) in particular

  - Slowly changing dimensions
    -- Describe
    -- Kimball Types
    -- Example Type 0
    -- Example Type 1
    -- ??? Type 2

  - Walk through example of a complete Star Schema ERD

## Enforced Fun Quiz

- tbd maybe do not include

## Further Reading

- [Recommendations](/further-reading.md)
