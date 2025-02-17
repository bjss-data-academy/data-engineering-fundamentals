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

## Facts and Dimensions
The core of the Star Schema are the facts and dimensions. 

Together, they allow us to answer queries of the form:

> How much [fact] by [dimension] ?

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

Facts are physical processes. Each row in a fact table represents a measurement event, somehwere in the business.

Fact tables are often _high [cardinality](https://en.wikipedia.org/wiki/Cardinality)_ - there will be very many rows, each one representing a unique measurement.

A Data Warehouse may have many facts, each arranged in their own star schema.

### Grain - granularity of facts

Fact tables are designed to have fine granularity of the things they are measuring. This is known as the [_grain_](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/grain/) of the table.

Fine grained fact tables allow us to drill down into the data. The lowest granularity of query will be the individual row in our fact table.

Take product sales as an example. A sales fact table would probably record every individual sale. We can then query over the range "every sale ever" to one specific sale, and potentially everything in between.

Deciding on the grain of each fact is a core part of the design of a star schema.

### Surrogate keys: Stable identity for facts

Fact tables generally have a _Primary Key_ to identify each fact with.

Primary Keys can be business-related keys like `customer_name` for example. These tend to be _unstable_ over time: customers are free to change their legal name, and then we're stuck with all our relations keyed against the wrong name.

Fact tables prefer stable keys known as _surrogate keys_.

A surrogate key has no business meaning. It is simply some unique value to identify that row.

Typical surrogate keys are generated. They may be an auto-increment integer value, a random GUID, or (as in Oracle databases) an auto-incrementing string value.

Performance issues can arise with some choices of generator and your index strategy. A nice article about that is [here](https://blog.novanet.no/careful-with-guid-as-clustered-index/)

### Example schema: chart_song_fact

In our example above, we have one fact `chart_position`, in a fact table `chart_song_fact`. It's primary key is `song_id`, an auto-increment integer surrogate key.

This schema is part of a project to analyse the UK Top 100 music charts. The core fact was the position in the charts 1 to 100 each week, for every song released since the chart data was recorded.

## What is a Dimension?

Surrounding each fact is a _dimension_, providing the _context_ relevant to the fact. Dimensions provide the _who, what and when_ about each fact.

Typical dimensions include:

- Date and time of the fact
- Geography - region, country, city
- Grouping - Product category, Sales Channel
- Employee - which team member or department was involved
- Ranges to support window queries - date range, price range, quantity range

Once we have a dimension linked to a fact table, we can query for facts _along that dimension_. A time dimension allows us to answer _how many were sold on the 16th of January?_ and _how many were sold in November?_ as a _roll-up_ (aggregate).

Dimension tables are often of _low [cardinality](https://en.wikipedia.org/wiki/Cardinality)_ - not many rows are necessary. A date dimension might only have one row per month, if we only want to analyse to one month granularity.

### Display-only columns
A dimension table will often contain columns used purely for display purposes.

An example would be in a date dimension. A column _displayed_as_ might have a value _Firday, 7th February 2025_. This would allow us to populate a field to show to the user, without requiring programming logic to convert the raw date. 

> This is especially important when using Power BI and other low-code tools for the UI

These columns will not be used in queries. They are for display only.

## Date Dimensions

Speaking of time dimensions, _dates_ are a very common dimension to have in business.

There are a couple of nice tricks with date dimensions:

- The primary key is often better as the date itself eg 20250207 instead of a surrogate key. Easier for us humans to work with.
- The date dimension can include various aggregates like _month-of-year_, _week-number_, _tax-year_. These simplify queries like _Total revenue for tax year 23/24?_.

Both tricks simplify lookup and querying of dates and date ranges.

## Conformed dimensions

As dates are so common, they are often _shared_ across many separate facts. If we use _the same_ date dimension table to power all facts, this dimension is said to be [_conformed_](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/conformed-dimension/).

Conformed dimensions simplify queries so they work over the same ranges across different facts.

Any dimension that is shared across multiple facts can be conformed.

## Slowly changing dimensions and Kimball type

What happens when a dimension _changes_?

Take for example a customer credit score. That will start at some initial value, and then will most likely vary over time.

There are several ways of ahndling such slowly changing dimensions, catalogued as _Kimball types_ of Dimension.

Types range from 0 to 7, with types 0, 1 and 2 being common.

### Kimball Type 0

This type represents storing the dimension as an unchanging value. The initial value of customer creedit score would be stored this way. It is one fixed value that actually _never_ changes, because whatever happens in future, the initial value will always be just that.

### Kimball Type 1

With type 1, we want to _track_ the latest value, but _do not_ track any history.

To achieve this, we can lookup the dimension and simply _overwrite_ its value.

Our credit score of 10 can be overwritten with latest value 20. We lose history, and that's ok.

### Kimball Type 2

Here, we want to preserve the change history. We want to access all values of the dimension throughout its life cycle.

To achieve this, we _add rows_ to the dimension table, and link our fact to the newly added row.

It is convenient to have a _flag column_ marking the row as "current value". This optimises queries involving the present time. Historical queries are supported without the optimisation.

## Aggregating facts along dimensions provides insight

Facts are the basis of our business-level aggregates that our Gold stage creates.

As such, we need to be able to aggregate the quantity in a fact. We cannot always do this, leading us to define different kinds of facts:

- Additive facts
- Semi-additive facts
- Non-additive facts

An _additive fact_ can be aggregated across all of its Dimensions. AN example would be weight. You can always find a total weight.

A _non-additive fact_ cannot be aggregated at all. An example is a _ratio_ which cannot be added. It stands alone as a piece of data.

In between the two is the _semi-additive fact_, which can be aggregated along some, but not all, of its Dimensions. An example here is a _balance amount_. This can be aggregated across all Dimensions except for time.

# Further Reading

- [YouTube - Star Schema - Pragmatic Works](https://www.youtube.com/watch?v=7ciFtfi-kQs) 
- [Star Schema on Wikipedia](https://en.wikipedia.org/wiki/Star_schema)
- [Star Schema at Databricks](https://www.databricks.com/glossary/star-schema)
- Microsoft Learn [Star Schema](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema)
- [Agile Data Warehouse Design - Star Schema](https://learning.oreilly.com/videos/agile-data-warehouse/9781771374095/9781771374095-video229162/)

# Next

[Further Reading](/further-reading.md)

[Back to Contents](/contents.md)
