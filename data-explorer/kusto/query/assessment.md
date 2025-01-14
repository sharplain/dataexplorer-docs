---
title:  Writing assessment
description:  This article is for writing assessment purposes only.
ms.reviewer: orspod
ms.topic: reference
ms.date: 08/11/2024
---
# Kusto Query Language (KQL)

_Kusto Query Language_ (or KQL) is a powerful tool that you can use to explore your data and to discover patterns, identify anomalies and outliers, create statistical modeling, and more. 

This article provides an explanation of the query language and offers practical exercises to get you started on writing queries.

 - To access the query environment, use the [Azure Data Explorer Web UI](https://dataexplorer.azure.com/). 
 - To learn how to use KQL, see [Tutorial: Learn Common Operators](tutorials/learn-common-operators.md).

You can query different kinds of data. The language is expressive, so that queries are easily read and their intent understood, and it is optimized for authoring experiences. Kusto Query Language is optimal for querying telemetry, metrics, and logs. It offers deep support for text search and parsing, time-series operators and functions, analytics and aggregation, geospatial functions, vector similarity searches, and many other language constructs used in data analysis. 

A Kusto query is a read-only request to process data and return results. The request is stated in plain text, using a data-flow model that is easy to read, author, and automate. 

The query uses schema entities that are organized in a hierarchy similar to SQLs: databases, tables, and columns.

Kusto queries are made of one or more query statements.

There are two kinds of user [query statements](statements.md):

1. A [tabular expression statement](tabular-expression-statements.md)
1. A [let statement](let-statement.md)
1. A [set statement](set-statement.md)

All query statements are separated by a `;` (semicolon), and only affect the query at hand.

The most common kind of query statement is a tabular expression *statement*, which means both its input and output consist of tables or tabular datasets. Tabular statements contain zero or more *operators*, each of which starts with a tabular input and returns a tabular output. Operators are sequenced by a `|` (pipe). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.

For information about application query statements, see [Application Query Statements](statements.md#application-query-statements).

The query process can be thought of as a funnel, where you start out with an an input data table. Each time the data passes through another operator, it is filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query operator order is important, and it can affect both results and performance. At the end of the funnel, the data you are looking for is included in the output, in the format you need.

An example query is 

> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAwsuyS/KdS1LzSspVuCqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrGhrqGhhqKujpKaCJG4HENZENKklVsLVVUHLz8Q/ydHFUUgDZkpxfmlcCAIItD6l6AAAA" target="_blank">Run the query</a>

```kusto
StormEvents 
| where StartTime between (datetime(2007-11-01) .. datetime(2007-12-01))
| where State == "florida"  
| count 
```

|Count|
|-----|
|   28|

Keep in mind that KQL is case-sensitive for all elements, including table names, table column names, operators, and functions.

This query has a single tabular expression statement. The statement begins with a reference to a table called *StormEvents* and contains several operators, [`where`](where-operator.md) and [`count`](count-operator.md), each separated by a pipe. 
The data rows for the source table are filtered by the value of the *StartTime* column and then filtered by the value of the *State* column. 
In the last line, the query returns a table with a single column and a single row containing the count of the remaining rows.

In contrast to Kusto queries, [Management commands](../management/index.md) are requests to Kusto to process or modify data or metadata. For example, the following management command creates a new Kusto table with two columns: `Level` and `Number`:

```kusto
.create table Logs (Level:string, Text:string)
```

Management commands have their own syntax, which isn't part of the Kusto Query Language syntax, although the two share many concepts. In particular, management commands are distinguished from queries by having the first character in the text of the command be the dot (`.`) character (which can't start a query). Why do we do it like this? This distinction prevents many kinds of security attacks, simply because it prevents embedding management commands inside queries.

Not all management commands modify data or metadata. Each command in the large class of commands that start with `.show` is used to display data or metadata. For example, the `.show tables` command returns a list of all tables in the current database.

For more information on management commands, see [Management Commands Overview](../management/index.md).

## KQL in Other Microsoft Services

KQL is used by many Microsoft services. Learn more about the use of KQL in these environments on the following pages:

[Log Queries in Azure Monitor](/azure/azure-monitor/logs/log-query-overview)
[Kusto Query Language in Microsoft Sentinel](/azure/sentinel/kusto-overview)
[Understanding the Azure Resource Graph Query Language](/azure/governance/resource-graph/concepts/query-language)
[Proactive Threat Hunting in Microsoft 365 Defender](/microsoft-365/security/defender/advanced-hunting-overview)
[CMPivot Queries](/mem/configmgr/core/servers/manage/cmpivot-overview#queries)

## Related Topics

* [Tutorial: Learn Common Operators](tutorials/learn-common-operators.md)
* [Tutorial: Use Aggregation Functions](tutorials/use-aggregation-functions.md)
* [KQL Quick Reference](kql-quick-reference.md)
* [SQL to KQL "Cheat Sheet"](sql-cheat-sheet.md)
* [Best Practices for Query Design](best-practices.md)
