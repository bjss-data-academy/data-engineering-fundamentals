# Star Schema

The Star Schema is a way of organising a relational database for use with OLAP queries. It directly supports fast access to data with range-based and windowing queries. Optionally, it may store pre-calculated values of business aggregates.

Here is a simple example star schema, which we will discuss below:

![Image of Star Schema for UK charts data shoing a song facts table with two dimensions](/images/uk-charts-star-schema.png)

> See this diagram [online](https://dbdiagram.io/d/UK-Charts-Star-Schema-679258cf37f5d6cbebb53dcf)
>
> _See our guide to_ [Entity Relation Diagrams](https://github.com/bjss-data-academy/entity-relation-diagrams/blob/main/README.md)

## Features of the Star Schema

Star Schema use the following techniques to support fast, flexible queries:

- Consists of a central _Fact_ in a _Fact Table_
- Facts are surrounded by _Dimensions_ in _Dimension Tables_
- _Denormalised_: No complex multi-table joins
- Pre-calculated aggregate values
- Data formatted ready for reporting

The name Star Schema comes from it's physical arrangement: A central fact table surrounded by a constellation of dimension tables.

## What is a Fact?

A _fact_ is a _measurable quantity_ about the business.

Common examples of facts:

- Sales volume in units
- Revenue
- Profit
- Weight
- Delivery time
- Speed
- Distance

Because a fact is measurable, it will often have a unit of measure.

Ideas to identify potential facts:

- What business values have a _unit_ - like kilograms, or a quantity?
- What does the business currently track?
- What analysis queries need supporting?

A Data Warehouse may have many facts, each arranged in their own star schema.

## What is a Dimension?

Surrounding each fact

-- Describe / Define
--Examples of Dimensions
-- Date and date ranges (buckets) in particular

- Slowly changing dimensions
  -- Describe
  -- Kimball Types
  -- Example Type 0
  -- Example Type 1
  -- ??? Type 2

# Further Reading

- Microsoft Learn [Star Schema](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema)
- [Agile Data Warehouse Design - Star Schema](https://learning.oreilly.com/videos/agile-data-warehouse/9781771374095/9781771374095-video229162/)

# Next

[Further Reading](/further-reading.md)

[Back to Contents](/contents.md)
