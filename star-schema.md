# Star Schema

The Star Schema is a way of organising a relational database for use with OLAP queries. It directly supports fast access to data with range-based and windowing queries.

Here is a simple example star schema, which we will discuss below:

![Image of Star Schema for UK charts data shoing a song facts table with two dimensions](/images/uk-charts-star-schema.png)

> See it online [here](https://dbdiagram.io/d/UK-Charts-Star-Schema-679258cf37f5d6cbebb53dcf)

- Invented 1990s Ralph Kimball

# Example Star Schema

-- See ERD in Five minutes Primer link (prerequisite)

> _See our guide_ [Entity Relation Diagrams](https://github.com/bjss-data-academy/entity-relation-diagrams/blob/main/README.md) _for details on the diagram_

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

# Next

[Further Reading](/further-reading.md)

[Back to Contents](/contents.md)
