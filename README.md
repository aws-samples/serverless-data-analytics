
# Building an End-to-End Serverless Data Analytics Solution on AWS


## Overview

In this lab, you are going to build a serverless architecture to analyze the data directly from Amazon S3 using [Amazon Athena](https://aws.amazon.com/athena/) and visualize the data in [Amazon QuickSight](https://quicksight.aws/). The data set that you are going to use is a public data set that includes trip records from all trips completed in Yellow and Green taxis in NYC from 2009 to 2016, and all trips in for-hire vehicles (FHV) from 2015 to 2016\. Records include fields capturing pick-up and drop-off dates/times, pick-up and drop-off locations, trip distances, itemized fares, rate types, payment types, and driver-reported passenger counts. The data set is already partitioned and converted from CSV to Apache Parquet. In the first part of the lab you will be building SQL like queries using Amazon Athena to query both the data formats directly form Amazon S3 and comparing the query performance. In the second part, using Amazon Athena table, create in first part, as the data source for Amazon QuickSight you will generate visualization and meaningful insights from the data set in Amazon S3. An optional lab is included to incorporate serverless ETL using AWS Glue to optimize query performance. We also give you access to a take-home lab for you to reapply the same design and directly query the same dataset in Amazon S3 from an Amazon Redshift data warehouse using Redshift Spectrum\.

![architecture-overview.png](https://s3.amazonaws.com/us-east-1.data-analytics/labcontent/reinvent2017content-abd313/architectureoveriew.PNG)

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
## Labs

### Pre-requisites

[Create a new AWS Account](https://aws.amazon.com/free/) if you don't have one. 
 

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

For more details refer [Amazon Athena FAQ] (https://aws.amazon.com/athena/faqs/)

## AMAZON QUICKSIGHT

### What is Amazon QuickSight?

Amazon QuickSight is a fast, cloud-powered business analytics service that makes it easy to build visualizations, perform ad-hoc analysis, and quickly get business insights from your data. Using our cloud-based service you can easily connect to your data, perform advanced analysis, and create stunning visualizations and rich dashboards that can be accessed from any browser or mobile device.

### How is Amazon QuickSight different from traditional Business Intelligence (BI) solutions?

Traditional BI solutions often require teams of data engineers to spend months building complex data models before generating a report. They typically lack interactive ad-hoc data exploration and visualization, limiting users to canned reports and pre-selected queries. Traditional BI solutions also require significant up-front investment in complex and costly hardware and software, and then customers to invest in even more infrastructure to maintain fast query performance as database sizes grow. This cost and complexity makes it difficult for companies to enable analytics solutions across their organizations. Amazon QuickSight has been designed to solve these problems by bringing the scale and flexibility of the AWS Cloud to business analytics. Unlike traditional BI or data discovery solutions, getting started with Amazon QuickSight is simple and fast. When you log in, Amazon QuickSight seamlessly discovers your data sources in AWS services such as Amazon Redshift, Amazon RDS, Amazon Athena, and Amazon Simple Storage Service (Amazon S3). You can connect to any of the data sources discovered by Amazon QuickSight and get insights from this data in minutes. You can choose for Amazon QuickSight to keep the data in SPICE up-to-date as the data in the underlying sources change. SPICE supports rich data discovery and business analytics capabilities to help customers derive valuable insights from their data without worrying about provisioning or managing infrastructure. Organizations pay a low monthly fee for each Amazon QuickSight user, eliminating the cost of long-term licenses. With Amazon QuickSight, organizations can deliver rich business analytics functionality to all employees without incurring a huge cost upfront.

### Which data sources does Amazon QuickSight support?

You can connect to AWS data sources including Amazon RDS, Amazon Aurora, Amazon Redshift, Amazon Athena and Amazon S3\. You can also upload Excel spreadsheets or flat files (CSV, TSV, CLF, and ELF), connect to on-premises databases like SQL Server, MySQL and PostgreSQL and import data from SaaS applications like Salesforce.

For more details refer [Amazon QuickSight FAQ](https://quicksight.aws/resources/faq/)

## Amazon Redshift Spectrum

### What is Amazon Redshift Spectrum?

Amazon Redshift Spectrum is a feature of Amazon Redshift that enables you to run queries against exabytes of unstructured data in Amazon S3, with no loading or ETL required. When you issue a query, it goes to the Amazon Redshift SQL endpoint, which generates and optimizes a query plan. Amazon Redshift determines what data is local and what is in Amazon S3, generates a plan to minimize the amount of Amazon S3 data that needs to be read, requests Redshift Spectrum workers out of a shared resource pool to read and process data from Amazon S3.

Redshift Spectrum scales out to thousands of instances if needed, so queries run quickly regardless of data size. And, you can use the exact same SQL for Amazon S3 data as you do for your Amazon Redshift queries today and connect to the same Amazon Redshift endpoint using your same BI tools. Redshift Spectrum lets you separate storage and compute, allowing you to scale each independently. You can setup as many Amazon Redshift clusters as you need to query your Amazon S3 data lake, providing high availability and limitless concurrency. Redshift Spectrum gives you the freedom to store your data where you want, in the format you want, and have it available for processing when you need it.

### Can Redshift Spectrum replace Amazon EMR?

No. While Redshift Spectrum is great for running queries against data in Amazon Redshift and S3, it really isn’t a fit for the types of use cases that enterprises typically ask from processing frameworks like Amazon EMR.
Amazon EMR goes far beyond just running SQL queries. Amazon EMR is a managed service that lets you process and analyze extremely large data sets using the latest versions of popular big data processing frameworks, such as Spark, Hadoop, and Presto, on fully customizable clusters. With Amazon EMR you can run a wide variety of scale-out data processing tasks for applications such as machine learning, graph analytics, data transformation, streaming data, and virtually anything you can code. You can also use Redshift Spectrum together with EMR. Amazon Redshift Spectrum uses the same approach to store table definitions as Amazon EMR. So, if you’re already using EMR to process a large data store, you can use Redshift Spectrum to query that data right at the same time without interfering with your Amazon EMR jobs.

Query services, data warehouses, and complex data processing frameworks all have their place, and they are used for different things. You just need to choose the right tool for the job.

### When should I use Amazon Athena vs. Redshift Spectrum?

Amazon Athena is the simplest way to give any employee the ability to run ad-hoc queries on data in Amazon S3. Athena is serverless, so there is no infrastructure to setup or manage, and you can start analyzing your data immediately.

If you have frequently accessed data, that needs to be stored in a consistent, highly structured format, then you should use a data warehouse like Amazon Redshift. This gives you the flexibility to store your structured, frequently accessed data in Amazon Redshift, and use Redshift Spectrum to extend your Amazon Redshift queries out to the entire universe of data in your Amazon S3 data lake. This gives you the freedom to store your data where you want, in the format you want, and have it available for processing when you need.

### Can I use Redshift Spectrum to query data that I process using Amazon EMR?

Yes, Redshift Spectrum can support the same Apache Hive Metastore used by Amazon EMR to locate data and table definitions. If you’re using Amazon EMR and have a Hive Metastore already, you just have to configure your Amazon Redshift cluster to use it. You can then start querying that data right away along with your Amazon EMR jobs.

For more details refer [Amazon Redshift Spectrum FAQ](https://aws.amazon.com/redshift/faqs/) 

## AWS Glue

### What is AWS Glue?

AWS Glue is a fully-managed, pay-as-you-go, extract, transform, and load (ETL) service that automates the time-consuming steps of data preparation for analytics. AWS Glue automatically discovers and profiles your data via the Glue Data Catalog, recommends and generates ETL code to transform your source data into target schemas, and runs the ETL jobs on a fully managed, scale-out Apache Spark environment to load your data into its destination. It also allows you to setup, orchestrate, and monitor complex data flows.

### What are the main components of AWS Glue?

AWS Glue consists of a Data Catalog which is a central metadata repository, an ETL engine that can automatically generate Python code, and a flexible scheduler that handles dependency resolution, job monitoring, and retries. Together, these automate much of the undifferentiated heavy lifting involved with discovering, categorizing, cleaning, enriching, and moving data, so you can spend more time analyzing your data.

### When should I use AWS Glue?

You should use AWS Glue to discover properties of the data you own, transform it, and prepare it for analytics. Glue can automatically discover both structured and semi-structured data stored in your data lake on Amazon S3, data warehouse in Amazon Redshift, and various databases running on AWS. It provides a unified view of your data via the Glue Data Catalog that is available for ETL, querying and reporting using services like Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum. Glue automatically generates Python code for your ETL jobs that you can further customize using tools you are already familiar with. AWS Glue is serverless, so there are no compute resources to configure and manage.

### What data sources does AWS Glue support?

AWS Glue natively supports data stored in Amazon Aurora, Amazon RDS for MySQL, Amazon RDS for Oracle, Amazon RDS for PostgreSQL, Amazon RDS for SQL Server, Amazon Redshift, and Amazon S3, as well as MySQL, Oracle, Microsoft SQL Server, and PostgreSQL databases in your Virtual Private Cloud (Amazon VPC) running on Amazon EC2. The metadata stored in the AWS Glue Data Catalog can be readily accessed from Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum. You can also write custom PySpark code and import custom libraries in your Glue ETL jobs to access data sources not natively supported by AWS Glue. For more details on importing custom libraries, refer to our documentation.

### What is the AWS Glue Data Catalog?

The AWS Glue Data Catalog is a central repository to store structural and operational metadata for all your data assets. For a given data set, you can store its table definition, physical location, add business relevant attributes, as well as track how this data has changed over time.

The AWS Glue Data Catalog is Apache Hive Metastore compatible and is a drop-in replacement for the Apache Hive Metastore for Big Data applications running on Amazon EMR. For more information on setting up your EMR cluster to use AWS Glue Data Catalog as an Apache Hive Metastore, click here.

The AWS Glue Data Catalog also provides out-of-box integration with Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum. Once you add your table definitions to the Glue Data Catalog, they are available for ETL and also readily available for querying in Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum so that you can have a common view of your data between these services.

### How can I customize the ETL code generated by AWS Glue?

AWS Glue’s ETL script recommendation system generates PySpark code. It leverages Glue’s custom ETL library to simplify access to data sources as well as manage job execution. You can find more details about the library in our documentation. You can write ETL code using AWS Glue’s custom library or write arbitrary Spark code in Python (PySpark code) by using inline editing via the AWS Glue Console script editor, downloading the auto-generated code, and editing it in your own IDE. You can also start with one of the many samples hosted in our Github repository and customize that code.

### When should I use AWS Glue vs. AWS Data Pipeline?

AWS Glue provides a managed ETL service that runs on a serverless Apache Spark environment. This allows you to focus on your ETL job and not worry about configuring and managing the underlying compute resources. AWS Glue takes a data first approach and allows you to focus on the data properties and data manipulation to transform the data to a form where you can derive business insights. It provides an integrated data catalog that makes metadata available for ETL as well as querying via Amazon Athena and Amazon Redshift Spectrum.

AWS Data Pipeline provides a managed orchestration service that gives you greater flexibility in terms of the execution environment, access and control over the compute resources that run your code, as well as the code itself that does data processing. AWS Data Pipeline launches compute resources in your account allowing you direct access to the Amazon EC2 instances or Amazon EMR clusters.

Furthermore, AWS Glue ETL jobs are PySpark based. If your use case requires you to use an engine other than Apache Spark or if you want to run a heterogeneous set of jobs that run on a variety of engines like Hive, Pig, etc., then AWS Data Pipeline would be a better choice.

For more details refer [AWS Glue FAQ](https://aws.amazon.com/glue/faqs/) 

## **ADDITIONAL RESOURCES**

### Amazon Athena:

- <https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/>
- <http://docs.aws.amazon.com/athena/latest/ug/convert-to-columnar.html>

### Redshift Spectrum
- https://aws.amazon.com/blogs/big-data/10-best-practices-for-amazon-redshift-spectrum/

### Serverless Analysis Architecture Blogs:
- <https://aws.amazon.com/blogs/big-data/derive-insights-from-iot-in-minutes-using-aws-iot-amazon-kinesis-firehose-amazon-athena-and-amazon-quicksight/>
- <https://aws.amazon.com/blogs/big-data/build-a-serverless-architecture-to-analyze-amazon-cloudfront-access-logs-using-aws-lambda-amazon-athena-and-amazon-kinesis-analytics/>

---
## License

This library is licensed under the Apache 2.0 License. 