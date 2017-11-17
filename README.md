
# Building an End-to-End Serverless Data Analytics Solution on AWS


## Overview

In this lab, you are going to build a serverless architecture to analyze the data directly from Amazon S3 using [Amazon Athena](https://aws.amazon.com/athena/) and visualize the data in [Amazon QuickSight](https://quicksight.aws/). The data set that you are going to use is a public data set that includes trip records from all trips completed in Yellow and Green taxis in NYC from 2009 to 2016, and all trips in for-hire vehicles (FHV) from 2015 to 2016\. Records include fields capturing pick-up and drop-off dates/times, pick-up and drop-off locations, trip distances, itemized fares, rate types, payment types, and driver-reported passenger counts. The data set is already partitioned and converted from CSV to Apache Parquet. In the first part of the lab you will be building SQL like queries using Amazon Athena to query both the data formats directly form Amazon S3 and comparing the query performance. In the second part, using Amazon Athena table, create in first part, as the data source for Amazon QuickSight you will generate visualization and meaningful insights from the data set in Amazon S3\.

![architecture-overview.png](quiver-image-url/1E1B2429B0C015C34A3A8A1B6AD305AE.png =619x302)

---

## AWS Console

### Verifying your region in the AWS Management Console

With Amazon Ec2, you can place instances in multiple locations. Amazon EC2 locations are composed of regions that contain more that one Availability Zones. Regions are dispersed and located in separate geographic areas (US, EU, etc.). Availability Zones are distinct locations within a region. They are are engineered to be isolated from failures in other Availability Zones and to provide inexpensive, low-latency network connectivity to other Availability Zones in the same region.

By launching instances in separate regions, you can design your application to be closer to specific customers or to meet legal or other requirements. By launching instances in separate Availability Zones, you can protect your application from localized regional failures.

### Verify your Region

The AWS region name is always listed in the upper-right corner of the AWS Management Console, in the navigation bar.

* Make a note of the AWS region name, for example, for this lab you will need to choose the **US West (Oregon)** region.
* Use the chart below to determine the region code. Choose **us-west-2 for this lab.**

|  Region Name |Region Code|
|---|---|
|US East (Northern Virginia) Region|us-east-1  |
|US West (Oregon) Region|us-west-2|
|Asia Pacific (Tokyo) Region|ap-northeast-1|
|Asia Pacific (Seoul) Region|ap-northeast-2|
|Asia Pacific (Singapore) Region|ap-southeast-1|
|Asia Pacific (Sydney) Region|ap-southeast-2|
|EU (Ireland) Region|eu-west-1|
|EU (Frankfurt) Region|eu-central-1|

---

|Lab|Name|
|---|----|
|Lab 1|[Serverless Analysis of data in Amazon S3 using Amazon Athena](./Lab1)|
|Lab 2|[Visualization using Amazon QuickSight](./Lab2)|
|Lab 3|[Serverless ETL and Data Discovery using Amazon Glue](./Lab3)|
|Lab 4|[Analysis of data in Amazon S3 using Amazon Redshift Spectrum](./Lab4)|


## AMAZON ATHENA

### What is Amazon Athena?

Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to setup or manage, and you can start analyzing data immediately. You don’t even need to load your data into Athena, it works directly with data stored in S3\. To get started, just log into the Athena Management Console, define your schema, and start querying. Amazon Athena uses Presto with full standard SQL support and works with a variety of standard data formats, including CSV, JSON, ORC, Apache Parquet and Avro. While Amazon Athena is ideal for quick, ad-hoc querying and integrates with Amazon QuickSight for easy visualization, it can also handle complex analysis, including large joins, window functions, and arrays. 

### What can I do with Amazon Athena?

Amazon Athena helps you analyze data stored in Amazon S3\. You can use Athena to run ad-hoc queries using ANSI SQL, without the need to aggregate or load the data into Athena. Amazon Athena can process unstructured, semi-structured, and structured data sets. Examples include CSV, JSON, Avro or columnar data formats such as Apache Parquet and Apache ORC. Amazon Athena integrates with Amazon QuickSight for easy visualization. You can also use Amazon Athena to generate reports or to explore data with business intelligence tools or SQL clients, connected via a [JDBC driver](http://docs.aws.amazon.com/athena/latest/ug/connect-with-jdbc.html).

### How do you access Amazon Athena?

Amazon Athena can be accessed via the AWS management console and a JDBC driver. You can programmatically run queries, add tables or partitions using the [JDBC driver](http://docs.aws.amazon.com/athena/latest/ug/connect-with-jdbc.html). 

### What is the underlying technology behind Amazon Athena?

Amazon Athena uses Presto with full standard SQL support and works with a variety of standard data formats, including CSV, JSON, ORC, Avro, and Parquet. Athena can handle complex analysis, including large joins, window functions, and arrays. Because Amazon Athena uses Amazon S3 as the underlying data store, it is highly available and durable with data redundantly stored across multiple facilities and multiple devices in each facility.

### How do I create tables and schemas for my data on Amazon S3?

Amazon Athena uses Apache Hive DDL to define tables. You can run DDL statements using the Athena console, via a JDBC driver, or using the Athena create table wizard. When you create a new table schema in Amazon Athena the schema is stored in the data catalog and used when executing queries, but it does not modify your data in S3\. Athena uses an approach known as schema-on-read, which allows you to project your schema onto your data at the time you execute a query. This eliminates the need for any data loading or transformation. Learn more about [creating tables](http://docs.aws.amazon.com/athena/latest/ug/creating-tables.html).

### What data formats does Amazon Athena support?

Amazon Athena supports a wide variety of data formats like CSV, TSV, JSON, or Textfiles and also supports open source columnar formats such as Apache ORC and Apache Parquet. Athena also supports compressed data in Snappy, Zlib, LZO, and GZIP formats. By compressing, partitioning, and using columnar formats you can improve performance and reduce your costs.

## AMAZON QUICKSIGHT

### What is Amazon QuickSight?

Amazon QuickSight is a fast, cloud-powered business analytics service that makes it easy to build visualizations, perform ad-hoc analysis, and quickly get business insights from your data. Using our cloud-based service you can easily connect to your data, perform advanced analysis, and create stunning visualizations and rich dashboards that can be accessed from any browser or mobile device.

### How is Amazon QuickSight different from traditional Business Intelligence (BI) solutions?

Traditional BI solutions often require teams of data engineers to spend months building complex data models before generating a report. They typically lack interactive ad-hoc data exploration and visualization, limiting users to canned reports and pre-selected queries. Traditional BI solutions also require significant up-front investment in complex and costly hardware and software, and then customers to invest in even more infrastructure to maintain fast query performance as database sizes grow. This cost and complexity makes it difficult for companies to enable analytics solutions across their organizations. Amazon QuickSight has been designed to solve these problems by bringing the scale and flexibility of the AWS Cloud to business analytics. Unlike traditional BI or data discovery solutions, getting started with Amazon QuickSight is simple and fast. When you log in, Amazon QuickSight seamlessly discovers your data sources in AWS services such as Amazon Redshift, Amazon RDS, Amazon Athena, and Amazon Simple Storage Service (Amazon S3). You can connect to any of the data sources discovered by Amazon QuickSight and get insights from this data in minutes. You can choose for Amazon QuickSight to keep the data in SPICE up-to-date as the data in the underlying sources change. SPICE supports rich data discovery and business analytics capabilities to help customers derive valuable insights from their data without worrying about provisioning or managing infrastructure. Organizations pay a low monthly fee for each Amazon QuickSight user, eliminating the cost of long-term licenses. With Amazon QuickSight, organizations can deliver rich business analytics functionality to all employees without incurring a huge cost upfront.

### Which data sources does Amazon QuickSight support?

You can connect to AWS data sources including Amazon RDS, Amazon Aurora, Amazon Redshift, Amazon Athena and Amazon S3\. You can also upload Excel spreadsheets or flat files (CSV, TSV, CLF, and ELF), connect to on-premises databases like SQL Server, MySQL and PostgreSQL and import data from SaaS applications like Salesforce.

## **ADDITIONAL RESOURCES**

Amazon Athena:

- <https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/>

- <http://docs.aws.amazon.com/athena/latest/ug/convert-to-columnar.html>

Redshift Spectrum

- Introduction: <https://aws.amazon.com/redshift/spectrum/>

- Lab: <https://w.amazon.com/bin/view/Plusteam/Workshop/Serverless-Labs/AmazonRedshiftSpectrum>

Serverless Analysis Architecture Blogs:

- <https://aws.amazon.com/blogs/big-data/derive-insights-from-iot-in-minutes-using-aws-iot-amazon-kinesis-firehose-amazon-athena-and-amazon-quicksight/>
- <https://aws.amazon.com/blogs/big-data/build-a-serverless-architecture-to-analyze-amazon-cloudfront-access-logs-using-aws-lambda-amazon-athena-and-amazon-kinesis-analytics/>

---
## License

This library is licensed under the Apache 2.0 License. 