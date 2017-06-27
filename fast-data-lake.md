### Fast Data Lake 


#### Intent
Define a way to consistently compute insights based on (live) business data without creating performance cost on the business transaction process.
 
 
#### Also Known As 
Live operational database


#### Motivation 
In today’s business environment, there is an increasing need for insights that are derived from historical and present data.  These requirements usually compete with the need for fast business transaction processing because both systems use the same resources.
Some examples of segments where business transaction speed is vital for efficient operations and business success include: flight scheduling, on-line trading, banking, e-commerce and more.

One way to store data for analytical analysis is implement a data lake.
[Data Lake](https://en.wikipedia.org/wiki/Data_lake) is a huge reservoir of data in various structures (some may be even unstructured) that are saved quickly in their natural  format that facilitates the collocation of data in various schemata and structural forms
The idea of data lake is to have a single store of all data in the enterprise ranging from raw data (which implies exact copy of source system data) to transformed data which is used for various tasks including reporting, visualization, analytics and machine learning. 

However, it is challenging to leverage data lakes in an efficient manner due to the large amounts of data along with the lack of uniform schema. Performing repeated queries against huge amounts of unstructured data can take a long time to execute.

[Stein, Brian; Morrison, Alan (2014)](http://www.pwc.com/en_US/us/technoindicescast/2014/cloud-computing/assets/pdf/pwc-technology-forecast-data-lakes.pdf)  advise that "The analytics challenge is not creating a data lake, but taking advantage of the opportunities it presents."

A Fast Data Lake will solve the query latency challenge.


#### Applicability 
Fast data lake is beneficial when:
* Analytical insights can give you business advantage
* Paying real-time operational performance penalty for those insights is not acceptable
* Analysis/insights are executed on medium data size (up to ~2 PTB)
* You require flexibility concerning the types of analysis that are supported
* The query response time must be “fast”
* It is acceptable to derive insights from data that is dated (according to business specification) and not necessarily in real-time

#### Structure 
1. Extract the selected data from the data lake, transform the data to the appropriate structure according to the expected queries and save it in a medium that supports queries. 
2. The required analysis, queries or machine learning are run on the transformed data
2. Repeat 1 and 2 as required. The new transformed data will replace the existing data.
 

#### Participants
* Business online transaction systems such as:
  * Legacy database
  * Enterprise server
  * In-memory technology
* Data lake that such as:
  * Amazon S3
  * HDFS or similar technologies
* A medium serving as a fast data lake that supports:
  * Fast analytics and insights 
  * Fast data ingestion from the data lake
  * Data streamer to quickly move data from the data lake to the medium
	Note that analysis time is influenced by the streaming speed.
  

#### Collaborations
 

#### Consequences
If implemented correctly fast data lake processing will provide fast insights (resulting from the schema, indices and the pre-processing) accompanied with a very small and controlled overhead (streaming time and frequency) on the business transactional environment.
 
#### Implementation
In Memory Data Grid technology is a natural candidate to serve as the medium for the fast data lake because it:
1. Ingests and processes data very fast 
2. Enables complicated business-related data transformation during and after ingestion
3. Supports machine learning
4. Supports rich query language
5. Makes reporting and visualization easy

Implementation with GigaSpaces[InsightEdge](https://insightedge.io/)
 
The following options are available for data streaming from the data lake to the GigaSpaces IMG
1. Use Apache Spark to read data from the data lake as an RDD, which can be filtered and transformed and then saved to the grid.
2. Use Java Client to read from the data lake, filter and transform the data and then save to the grid using XAP API
3. Write a specific [Processing Unit](https://docs.gigaspaces.com/xap/12.1/dev-java/the-processing-unit-overview.html) that reads the specific data that is needed for each partition from the data lake, filters and transforms the data and then saves it to the grid
 
The following options are available for performing analytics and machine learning on the fast data lake:
1. Grid aggregation commands
2. Python via Spark Notebook
3. Spark Notebook as a UI
4. Spark Machine Learning 
 
Sample Code
 
##### Example of loading data from the data lake to the grid using Spark SQL
Example of InsightEdge code which reads data from "postgresql" into XAP's grid using Spark SQL.
download zepplin's [Notebook](https://github.com/InsightEdge/aa-helios.git)
Note that in order to run the example, you must configure postgresql.
 
```Scala
   import org.apache.spark._
   import org.apache.spark.sql._
   import sqlContext.implicits._
   import java.sql._
   
   val startTimeDB3 = System.currentTimeMillis()
   // Read data from postgress using spark jdbc
   val resultDataFrame = sqlContext.load("jdbc", Map(
                 "url" -> "jdbc:postgresql://localhost/postgres?user=giga123&password=giga123","dbtable" -> "forecast_result"))
   // Define the schema for this data.              
   var resultRDD = resultDataFrame.map(row =>  new 
            Result(
               row.getAs[Long]("routing_id"), 
               row.getAs[Int]("status_code"), 
               row.getAs[Int]("sponsor_code"),
               row.getAs[Int]("sponsor_id"), 
               row.getAs[Int]("num_observations")
       ))
       
   // Place here, the code that filters and transforms the RDD
       
   // save the RDD to the date grid, note that if the Result object defined with indices those indices will be used to fetch the data from the grid
   // and on all the following analytics queries. 
   resultRDD.saveToGrid() 
   val endTimeDB3 = System.currentTimeMillis()
   println("--- Time took to get result data from DB in seconds=" + 
                 (endTimeDB3-startTimeDB3)/1000.0f)
```
 
#### Known Uses
 
#### Related Patterns
HTAP

