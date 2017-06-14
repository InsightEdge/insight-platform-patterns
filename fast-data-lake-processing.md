
### Fast Data Lake Processing

#### Intent

Define a way to repeatably compute insights based on (live) business data without adding performance 
cost on the business transaction process.


#### Also Known 
Live operational database
 
#### Motivation 

In today business there is a great need for insights that computed base on the history and present state of the system.
Most of the time this needs is in competition with the need for fast business transaction processing because both system use the same resources.  
In situation when performance penalty to the business transaction as a result of the insight computation is not acceptable a data lake is a good solution.

As a result there is a pattern that called [Data Lake](https://en.wikipedia.org/wiki/Data_lake). 
Basically Data Lake is a huge reservoir of data in various structure (some it may be even unstructured)  
[Stein, Brian; Morrison, Alan (2014)](http://www.pwc.com/en_US/us/technology-forecast/2014/cloud-computing/assets/pdf/pwc-technology-forecast-data-lakes.pdf) advise that "The main challenge is not creating a data lake, but taking advantage of the opportunities it presents."

Consider a situation when insights can save lots of money but a the a delay in the business transaction is very costly, for example when computing flights schedule or when doing online trading.
The essence of those business is built on good insights, but this insights can not compete or delay the business transactions.

Thus using a regular HTAP and computing the insights on the live data is too risky.
On the other hand it is not easy to perform repeatedly queries against such huge amount of unstructured data, a naive query will take very long time to execute.
The Fast Data Lake Processing aim to solve those problems. 
 
#### Applicability 
when do you need it?  Use fast data lake processing when:
* Insights can give you business advantage.
* Paying real-time operational performance penalty for those insights is not acceptable.
* Analysis/insights are executed on medium data size
* The amount and intensity of the insights harvested is unknown or has big variation, and insights time should be near real time.
* It is acceptable to derive the insights from data that is a couple of hours dated and not in the moment
* The insights requires some type of modification/pre-processing on the live data.

#### Structure 

1. Extract the selected data to an schema side reservoir from the data lake, fold and index the data on the fly if needed.
2. On the side reservoir perform the needed analysis, queries or machine learning as much as needed.
3. Repeat 1 and 2 as needed.

#### Participants

* business online transaction system that can be
    * Legacy database
    * Some kind of Enterprise server.
    * In memory technology
* Data Lake that can be Amazon S3, HDFS or similar technologies.
* A side reservoir serve as the fast data lake processing, this system should have the capability to run fast the analytic on its data and derive the insights, it should also be possible to fast ingest the data from the data lake to this reservoir.
* Some kind of transformer that fill the selected items from the data lake to the side reservoir, this transformer should be able to do both filtering the data and mutate it on the way as well as index it for fast queries if needed.
   
It is important that the transformer will be fast as it can be since it eat analysis time.

#### Collaborations

#### Consequences

If implemented correctly fast data lake processing can provide fast insights for the business with very small and controlled overhead (because of the schema, indices and the pre processing) on near live business system.

#### Implementation

In Memory Data Grid technology is a natural candidate to serve as the side reservoir, because:
1. It is very fast, in processing and ingest.
2. It enable complicated business related mutation on the data even after the data was ingests.
3. I can support Machine Learning.
4. It is support rich query language.
5. It is very easy to write a reporting or UI on top of it.

#### Sample Code

#### Known Uses

#### Related Patterns
HTAP