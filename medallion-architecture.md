# Medallion Architecture

We need to take data as it comes from our transactional sources, filter it, sort it, combine it, and finally prepare it for presentation.

We need a data pipeline to do this.

_Medallion Architecture_ is a popular way of organising a data pipeline into three stages. Each stage has a name - bronze, silver and gold. Each stage is repsonsible for one job in the data pipeline.

![Image medallion architecture stages](/images/medallion-architecture.png)

Each stage increases data quality, while transforming raw data into business-level insights.

## Bronze Stage

The bronze stage deals with raw data. It will:

- Ingest
- Validate

Data is ingested from each of its sources into our data lake tools.

Raw Data comes as-is from any source it needs to.

This includes raw transactional data, such as from Point of Sale systems, card swipes, logins and existing databases. It also includes third-party data from web services such as payment processors or inventory systems. It may include reference data, such as maps and demographics.

We use a variety of technical approaches to get this data. Common ones include:

- SQL Queries from accessible databases
- HTTP calls to web services
- Reading files: CSV, JSON, XLSX, PDF, Word, Text
- Manual data entry (possibly)

Once the raw data is ingested into the system, it can have basic validation applied. Empty records can be discarded. Records with basic errors can either be corrected or discarded. Only basic validation is done at this stage, at the level of "obvious errors" on individual records.

By the end of Bronze stage, we have all the relevant raw data we need for our analysis available to our tool chain.

## Silver Stage

The silver stage further processes the output from bronze stage. This is where the heavy-lifting gets done in transforming the data from 'what you get' towards what the business needs to understand.

The core tasks for Silver Stage are to:

- Filter
- Clean
- Augment

_Filtering_ data means removing parts we know we will not need. We know that raw data comes from anywhere we can get it, in any format we can obtain. Clearly, this data is not formatted for our specific purpose. It will both include irrlevant (to us) data, and be missing pieces of critical importance.

![Shows irrelevant data items being filtered out](/images/filtering.png)

_Cleaning_ the data we do need involves actions like removing duplicate records (_de-duping_) and enforcing consistency.

![Showing duplicates crossed out from a data set](/images/de-duping.png)

Getting consistent data can be a large task. One example might be taking three data sources which specify customer location in different ways; perhaps by street name and number in one, postcode in another and MP Constituency in the third. We want to be able to query by location, so we would find a way to convert each format into a common one - perhaps postcode, or 'nearest major town'.

_Augmenting_ data means adding missing data from other sources we can link to.

An example of augmenting data would be to take a list of BJSS employee records with their home address from an HR system, and augment that data with their nearest BJSS Office location. Then we could easily query for employees-by-office-location.

![Showing employee data augmented by office location](/images/augmenting-data.png)

The silver stage completes with us having cleaned, validated data that has been augmented to suit our business queries.

## Gold Stage

The final stage - gold - is about organising the data ready for use.

Analytics users will want to be able to creaft ad-hoc queries over _business aggregates_. These queries will be expected to run quickly enough to be usable. The data needs to be flexible enough to support _drill down_ and _window_ queries.

_Drill down_ is where we can query at different grnaularity:

- Total sales worldwide
- Total sales by region
- Total sales by country

Each of these queries references fewer data items than the one before. importantly, they answer different business-level questions.

_Window_ queries define ranges of interest, usually a range of dates or perhaps a range of spending:

- Total profit between January and March
- Total profit between February and April
- Number of customers spending £500-1000 annually
- Number of customers spending £750-1500 annually

### Preparing for read access

The final task of gold stage is to store data in the structure that best suits the queries we want to run (and reasonably expect might run in future).

Part of this decision ivolves the access technology:

- Raw SQL queries?
- A tool like Crystal Reports?
- Low-code app like Microsoft Power Platform?
- Custom mobile app powered by REST API?

Whatever final structure we choose, the gold stage is repsonsible for organising the transformeed data.

In the next section, we look at one powerful output structure called the _star schema_.

# Further Reading

- [Medallion Architecture at Databricks](https://www.databricks.com/glossary/medallion-architecture)

# Next

[Star Schema](/star-schema.md)

[Back to Contents](/contents.md)
