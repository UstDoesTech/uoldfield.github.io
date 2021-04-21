---
layout: post
title:  "Introduction to Data Lakes"
date:   2016-05-13 08:55:38 +0000
---
Data Lakes are the new hot topic in the big data and BI communities. Data Lakes have been around for a few years now, but have only gained popular notice within the last year. In this blog I will take you through the concept of a Data Lake, so that you can begin your own voyage on the lakes.

## What is a Data Lake?
Before we can answer this question, it’s worth reflecting on a concept which most of us know and love – Data Warehouses. A Data Warehouse is a form of data architecture. The core principal of a Data Warehouse isn’t the database, it’s the data architecture which the database and tools implement. Conceptually, the condensed and isolated features of a Data Warehouse are around:

1. Data Acquisition
2. Data Management
3. Data Delivery / Access

A Data Lake is similar to a Data Warehouse in these regards. It is an architecture. The technology which underpins a Data Lake enables the architecture of the lake to flow and develop. Conceptually, the architecture of a Data Lake wants to acquire data, it needs careful, yet agile management, and the results of any exploration of the data should be made accessible. The two architectures can be used together, but conceptually the similarities end here.

Conceptually, Data Lakes and Data Warehouses are broadly similar yet the approaches are vastly different. So let’s leave Data Warehousing here and dive deeper into Data Lakes.

Fundamentally, a Data Lake is just not a repository. It is a series of containers which capture, manage and explore any form of raw data at scale, enabled by low cost technologies, from which multiple downstream applications can access valuable insight which was previously inaccessible.

## How Do Data Lakes Work?
Conceptually, a Data Lake is similar to a real lake – water flows in, fills up the reservoir and flows out again. The incoming flow represents multiple raw data formats, ranging from emails, sensor data, spreadsheets, relational data, social media content, etc. The reservoir represents the store of the raw data, where analytics can be run on all or some of the data. The outflow is the analysed data, which is made accessible to users.

To break it down, most Data Lake architectures come as two parts. Firstly, there is a large distributed storage engine with very few rules/limitations. This provides a repository for data of any size and shape. It can hold a mixture of relational data structures, semi-structured flat files and completely unstructured data dumps. The fundamental point is that it can store any type of data you may need to analyse. The data is spread across a distributed array of cheap storage that can be accessed independently.

There is then a scalable compute layer, designed to take a traditional SQL-style query and break it into small parts that can then be run massively in parallel because of the distributed nature of the disks.

In essence – we are overcoming the limitations of
traditional querying by:

-  Separating compute so it can scale independently
- Parallelizing storage to reduce impact of I/O bottlenecks

There are various technologies and design patterns which form the basis of Data
Lakes. In terms of technologies these include:

- Azure Data Lake
- Cassandra
- Hadoop
- S3
- Teradata

With regards to design patterns, these will be explored in due course. However, before we get there, there are some challenges which you must be made aware of. These challenges are:

1. Data dumping – It’s very easy to treat a data lake as a dumping ground for anything and everything. This will essentially create a data swamp, which no one will want to go into.

2. Data drowning – the volume of the data could be massive and the velocity very fast. There is a real risk of drowning by not fully knowing what data you have in your lake.

These challenges require good design and governance, which will be covered off in the near future.

Hopefully this has given you a brief, yet comprehensive high-level overview of what data lakes are. We will be focusing on Azure Data Lake, which is a management implementation of the Hadoop architectures. Further reading on Azure Data Lake can be found below.

### Further Reading
[Getting Started With Azure Data Lake Store](https://adatis.co.uk/Getting-Started-with-Azure-Data-Lake-Store/)

[Getting Started With Azure Data Lake Analytics and U-SQL](http://adatis.co.uk/getting-started-with-azure-data-lake-analytics-u-sql/)

[Data Lake Overview](https://azure.microsoft.com/en-us/solutions/data-lake/)

This post was originally published at [Adatis](https://adatis.co.uk/introduction-to-data-lakes/) on the 13th of May 2016.