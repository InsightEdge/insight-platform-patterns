
### Data Lake

#### Intent

Define a way to compute insights from business data without adding performance 
cost on the business transaction process.


#### Also Known As
???
 
#### Motivation 

In today business there is a great need for insights that computed base on the history and present state of the system.
Most of the tme this needs is in competition with the need for fast business transaction processing because both system use the same resources.  
In situation when performance penalty to the business transaction as a result of the insight computation is not acceptable a data lake is a good solution.

Consider a situation when insights can save lots of mony but a the a delay in the business transaction is very costly, for example when computing flights schedule or when doing online trading.
The essence of those business is built on good insights, but this insights can not compete or delay the business transactions.

Thus using a regular HTAP and computing the insights on the live data is too risky.

 
#### Applicability 

Use data lake when

* Insights can give you business advantage.
* Paying performance penalty for those insights are not acceptable.
* The amount and insensitivity of the insights harvesting is unknown or has big variation.
* It is possible to derive the insights from data that is couple of hours dated.
* The insights derivation is best done not on the live data but on some derivation of it. 

#### Structure 

1. Extract Data from the business system to a side system that and transform it while if needed.
2. On the side system perform the needed analysis queries or machine learning as much as needed.
3. Repeat.

#### Participants

* business online transaction system that can be
    * Legacy database
    * Some kind of Enterprise server.
    * In memory technology  
* offline system that serve as the data lake, this system should have the capability to run the analytic on its data and derive the insights.
* Some kind of transformer that serve as a data transfromer from the business online system to the data lake, this transformer should be able to both filtering the data and mutate it on the way.
   
It is important that the transformer will be fast as it can be since it share time with the analysis phase.
It is also important that the transformer will take very small resources from the online system as it transform the data.   

#### Collaborations

#### Consequences

If implemented correctly data lake can provide the insights for the business with very small and controlled overhead on the live business system.
data lake can categories according to the following requirements.
* Fast data lake - a data lake that needed to support near realtime queries.
* Deep data lake - a data lake that needed to support rich processing and data manipulation.
* Structure data lake - a data lake that needed to support schema for its data.
* Indexed data lake - a data lake that needed to support random access for some of its data.  

#### Implementation

#### Sample Code

#### Known Uses

#### Related Patterns
HTAP