#Lab 4 [Take Home]: Analysis of data in Amazon S3 using Amazon Redshift Spectrum
---

##Architectural Diagram
![architecture-overview-lab4.png](quiver-image-url/03C7313F9465CBA6924B923928100C80.png =548x290)

---

## Deploying Amazon Redshift Cluster 

In this section you will use the CloudFromation template to create Amazon RedShift cluster resources. The template will also install [pgweb](https://github.com/sosedoff/pgweb), SQL Client for PostgreSQL, in an  Amazon EC2 instance to connect and run your queries on the launched Amazon Redshift cluster. Alternatively, you can connect to the Amazon Redshift cluster using standard SQL Clients such as SQL Workbench/J. For more information refer http://docs.aws.amazon.com/redshift/latest/mgmt/connecting-using-workbench.html.

1. Login in to your AWS console and open the [Amazon CloudFormation Dashboard](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2]) 
2. Make a note of the AWS region name, for example, for this lab you will need to choose the **US West (Oregon)** region.
3. Click **Create Stack**
4. Select **Specify an Amazon S3 template URL**
5. Type Amazon S3 URL of the Amazon CloudFromation Teamplate. 

```
  https://s3-us-west-2.amazonaws.com/pluslabs/redshift-demo-cdov1.0.template
```
6. Click **Next**
7. Type a name *(e.g. RedshiftSpectrumLab)* for the **Stack Name**

![IMAGE](quiver-image-url/5DBE9603B431254ABE298D2C5ABCD9B7.jpg =453x222)

8. Enter the following **Parameters** for **Redshift Cluster Configuration**
i. Choose 8multi-node* for **ClusterType**
ii. Type *2* for the **NumberOfNodes**
iii. For **NodeType** select *ds2.xlarge*

![IMAGE](quiver-image-url/FD5FA63A642C11B9464E06A91BD5EBCD.jpg =349x135)

9.  Enter the following **Parameters** for **Redshift Database Configuration**
 i. Type a name (e.g. dbadmin) for **MasterUserName**
 ii. Type a password for **MasterUserPassword**
 iii. Type the a name (e.g. taxidb) for **DatabaseName**
 iv. Type the IP address of your local machine for **ClientIP**

![IMAGE](quiver-image-url/B4696A12788DACAB44CB30A8A9610635.jpg =353x110)

10. Enter the following **Parameters** for **Glue Crawler Configuration**
i. Type the name(e.g. taxi-spectrum-db) for **GlueCatalogDBName**
ii. Type the name(e.g. csvCrawler) for **CSVCrawler**
iii. Type the name(e.g. parquetCrawler) for **ParquetCrawler**

10. Click **Next**
11. [Optional] In the **Tags** sub-sections in **Options** type a **Key** name *(e.g. Name)* and **Value** for key.
12. Click **Next**
13. Check **I acknowledge that AWS CloudFormation might create IAM resources.**
14. Click **Create**

> **Note:** This is may take approximately 15 minutes 

15. Ensure that status of the Amazon CloudFromation stack that you just create is **CREATE_COMPLETE**
16. Select your Amazon CloudFormation stack *(RedshiftSpectrumLab)*
17. Click on the **Outputs** tab
18. Review the list of **Key** and thier **Value** which will look like the following. 

![IMAGE](quiver-image-url/D79E778950D5017888EF7B39D2F4A767.jpg =468x262)

---
## Running AWS Glue Crawlers - CSV & Parquet Crawler 
1. Open [AWS Management Console for Glue](https://us-west-2.console.aws.amazon.com/glue/home?region=us-west-2#)
2. Go to AWS Glues Crawlers page by clicking on **Crawlers** in the navigation pane
3. Select the AWS Glue Crawler for CSV(e.g. csvCrawler)
4. Click **Run crawler**
5. Select the AWS Glue Crawler for CSV(e.g. csvCrawler)
6. Click **Run crawler**

> Note: This may take approximately 5 min for both the crawlers to parse the data in CSV and Parquet format. 

7. Wait for the **Status** of both the crawlers to *Ready* state

 Now that you have run the crawlers lest ensure that new tables *TaxiDataYellow* and *NYTaxiRides* been created. 

8. To to the list of databases in the AWS Glue Data Catalog click on **Databases** in the navigation pane.
9. Click on **taxi-spectrum-db**
10. Click on **Tables in taxi-spectrum-db**
11. Click on **TaxiDataYellow** to review the table definition and schema 
12. Navigate back and click on **NYTaxiRides** to review the table definition and schema
13. Click on **View partitions** to review the partition metadata
---

## Create Redshift Spectrum Scehma and reference external table form AWS Glue Data Catalog Database

1. Open the [Amazon CloudFormation Dashboard](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2]) 
2. Make a note of the AWS region name, for example, for this lab you will need to choose the **US West (Oregon)** region.
3. Select your Amazon CloudFormation stack *(RedshiftSpectrumLab)*
4. Click on the **Outputs** tab
5. Naviage to the **pgWeb** URL
6. In the pgWeb console ensure that the **SQL Query** tab is selected
7. Copy the following statement to create a database *(e.g. taxispectrum)* in Redshift Spectrum

```sql
  create external schema taxispectrum from data catalog
  database 'taxi-spectrum-db' 
  iam_role '<specify the redshift IAM Role arn from the CloudFormation outputs section>'
```
8. Replace the *<specify the redshift IAM Role arn from the CloudFormation output section'>* in the statment with the value of **redshiftIAMRole** from the **Outputs** tab of the Amazon CloudFromation stack *(RedshiftSpectrumLab)* you created as part of the lab.
9. 
> Note: The IAM role must be in single quotes

9. Click **Run Query**

> Note: You can create an external table in Amazon Redshift, AWS Glue, Amazon Athena, or an Apache Hive metastore. For more information, see [Getting Started Using AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/getting-started.html) in the AWS Glue Developer Guide, [Getting Started](http://docs.aws.amazon.com/athena/latest/ug/getting-started.html) in the Amazon Athena User Guide, or [Apache Hive](http://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hive.html) in the Amazon EMR Developer Guide.If your external table is defined in AWS Glue, Athena, or a Hive metastore, you first create an external schema that references the external database. Then you can reference the external table in your SELECT statement by prefixing the table name with the schema name, without needing to create the table in Amazon Redshift. For more information, see [Creating External Schemas for Amazon Redshift Spectrum](http://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-external-schemas.html.)

## Querying data from Amazon S3 using Amazon Redshift Spectrum

Now that you have created the schema, you can run queries on the data set and see the results in PGWeb Console.

1. Copy the following statement into the query pane, and then choose **Run Query**.

```sql
    SELECT * FROM taxispectrum.taxi limit 10
```

Results for the above query look like the following:
![Screen Shot 2017-11-14 at 9.16.45 PM.png](quiver-image-url/BC09E3AC46F16F2055A1EEF0734292A5.png =1036x388)

2.	Copy the following statement into the query pane, and then choose **Run Query** to get the total number of taxi rides for yellow cabs. 

````sql
    SELECT COUNT(1) as TotalCount FROM taxispectrum.taxi
````
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 9.25.23 PM.png](quiver-image-url/6E5D380BE04F4B59C90A2B0E403F7A94.png =400x113)

3. Copy the following statement into the query pane, and then choose **Run Query** to query for the number of rides per vendor, along with the average fair amount for yellow taxi rides

````sql
    SELECT 
    CASE vendorid 
         WHEN '1' THEN 'Creative Mobile Technologies'
         WHEN '2' THEN 'VeriFone Inc'
         ELSE CAST(vendorid as VARCHAR) END AS Vendor,
    COUNT(1) as RideCount, 
    avg(total_amount) as AverageAmount
    FROM taxispectrum.taxi
    WHERE total_amount > 0
    GROUP BY (1)
````
Results for the above query look like the following:

![Screen Shot 2017-11-14 at 9.46.55 PM.png](quiver-image-url/37C32AF182AE2812587C6AD76935DF02.png =1327x136)
---

## Querying partitioned data using Amazon Redshift Spectrum

By partitioning your data, you can restrict the amount of data scanned by each query, thus improving performance and reducing cost. Amazon Redshift Spectrum leverages Hive for [partitioning](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterPartition) data. You can partition your data by any key. A common practice is to partition the data based on time, often leading to a multi-level partitioning scheme. For example, a customer who has data coming in every hour might decide to partition by year, month, date, and hour. Another customer, who has data coming from many different sources but loaded one time per day, may partition by a data source identifier and date.


Now that you have added the partition metadata to the Athena data catalog you can now run your query.

1. Copy the following statement into the query pane, and then choose **Run Query** to get the total number of taxi rides

```sql
    SELECT count(1) as TotalCount from taxispectrum.ny_pub
```
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 10.08.50 PM.png](quiver-image-url/58242EF60ABE2C995A589AF7F3EBEEAC.png =368x93)

>**Note:**
> This query executes much faster because the data set is partitioned and it in optimal format - Apache Parquet (an open source columnar).

2. Copy the following statement into the query pane, and then choose **Run Query** to get the total number of taxi rides by year

```sql
    SELECT YEAR, count(1) as TotalCount from taxispectrum.ny_pub GROUP BY YEAR
```
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 10.11.47 PM.png](quiver-image-url/4D1FF37E88598C346E95E2EF2E68493B.png =789x359)

3. Copy the following statement into the query pane, and then choose **Run Query** to get the top 12 months by total number of rides across all the years

```sql
    SELECT YEAR, MONTH, COUNT(1) as TotalCount 
    FROM taxispectrum.ny_pub
    GROUP BY (1), (2) 
    ORDER BY (3) DESC LIMIT 12
```
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 10.13.54 PM.png](quiver-image-url/E7C8440B09253FD8944CFACC61407F34.png =1042x457)

4. Copy the following statement into the query pane, and then choose **Run Query** to get the monthly ride counts per taxi time for the year 2016.

```sql
    SELECT MONTH, TYPE, COUNT(1) as TotalCount 
    FROM taxispectrum.ny_pub
    WHERE YEAR = 2016 
    GROUP BY (1), (2)
    ORDER BY (1), (2)
```
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 10.18.08 PM.png](quiver-image-url/360A77F070EECAD261E77D52EC2623A9.png =1094x610)

5. Copy the following statement anywhere into the query pane, and then choose **Run Query**.

```sql
    SELECT MONTH, TYPE,
      avg(trip_distance) avgDistance,
      avg(total_amount/trip_distance) avgCostPerMile,
      avg(total_amount) avgCost,
      percentile_cont(0.99)
      within group (order by total_amount)
    FROM taxispectrum.ny_pub
    WHERE YEAR = 2016 AND (TYPE = 'yellow' OR TYPE = 'green')
    AND trip_distance > 0 AND total_amount > 0
    GROUP BY MONTH, TYPE
    ORDER BY MONTH
```
Results for the above query look like the following:
![Screen Shot 2017-11-14 at 10.23.51 PM.png](quiver-image-url/75D9E296EB1C4523A917BA14575C1A1E.png =1491x646)
---
## License

This library is licensed under the Apache 2.0 License. 