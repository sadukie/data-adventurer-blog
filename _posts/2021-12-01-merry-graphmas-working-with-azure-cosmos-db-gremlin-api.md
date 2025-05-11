---
title: "Merry Graphmas: Working with Azure Cosmos DB Gremlin API"
---

This post is part of the 2021 C# Advent Calendar. Check out the complete line-up at: [C# Advent (csadvent.christmas)](https://csadvent.christmas)

In this post, we will use Gremlin.Net to pass Gremlin queries to an Azure Cosmos DB Gremlin API endpoint. We will show how to get data back. Populating graphs and removing things from the graph will not be covered.

## What is Graphmas?

Graphmas is our celebration of the December holidays as they appear in the Azure Open Datasets' Public Holidays dataset. We will be exploring relationships in graph database form using Azure Cosmos DB Gremlin API.

![Graphmas Wordle - showing festive words for Christmas in other languages]({{site.baseurl}}/assets/images/posts/2021-12-01/graphmas-wordle.png)

Graphmas wordle created at [WordArt.com](https://wordart.com)

Christmas is the holiday I grew up celebrating in December, so between my love of graph databases (Graph) + Christmas (mas), we get Graphmas.

## Why C#?

I've been a Microsoft MVP in C#, which is nowadays part of the Developer Technologies expertise. I have a soft spot for C# as it's the language I have grown to love despite being the language with curly braces that I swore I wouldn't learn since I strongly disliked curly brace languages. That and when my friend Matt Groves mentioned the C# Advent calendar, I couldn't resist sharing my love of graph data with the C# side of things.

## Getting Started

I have created a sample .NET Core Console Application that has a variety of queries useful for getting to understand a graph database. The queries in this application are also used in my lightning for the Cincinnati Software Craftsmanship meeting later this evening as well as in an upcoming video in the Festive Tech Calendar (going live December 5, 2021!).

The resources needed for this include:

- Azure Cosmos DB Gremlin API resource
- Code from[my graphmas GitHub repo](https://github.com/sadukie/graphmas)
- Python 3
- Visual Studio Code with C# extensions
- .NET 6

## Downloading the Code

All sample code for this blog post lives in [my graphmas GitHub repo](https://github.com/sadukie/graphmas). The C# code is in the folder named `MerryGraphmas`. Note that this is demo code and is not intended for production as written. This is to show you how Gremlin queries operate and how to work with the Gremlin.Net driver. As of the end of November 2021, Azure Cosmos DB Gremlin API requires GraphSON v2. For our example, we are using [version 3.4.9 of the Gremlin.Net driver](https://tinkerpop.apache.org/dotnetdocs/3.4.9/api/Gremlin.Net.Driver.html).

## Creating an Azure Cosmos DB Gremlin API Resource

If you want to follow along in your own instance, you can use the Python 3 Jupyter Notebook Graphmas.ipynb to load your own dataset. Due to timing, a C# loading script is not available yet, though [an issue has been logged](https://github.com/sadukie/graphmas/issues/1).

## Setting Environment Variables

For our demo, we have a couple resources hard-coded. However, the host and primary key fields are being read from the environment. For your environment, you will need to set these fields. Here is how to find these values for your Azure Cosmos DB Gremlin API resource.

In the Azure portal, on your Azure Cosmos DB Gremlin API resource:

1. From the left-hand navigation, select **Keys**.
2. Within the Gremlin Endpoint, select the **Host** information that appears between wss:// and :443/.
3. For the **PrimaryKey** environment variable, copy the **Primary Key**.

For my environment, I work in Visual Studio Code. So I set my environment variables with the following details:

```powershell
$env:Host = "sd-graphmas.gremlin.cosmos.azure.com"
$env:PrimaryKey = "LongPasswordHere=="
```

## Exploring the .NET Core Console Application

In this code, we are:

- Creating a Gremlin Server with our connection information set up above
- Creating a Gremlin Client to talk with the Azure Cosmos DB Gremlin API instance using GraphSON2
- Displaying a menu of Gremlin statements to explore

I started out by using this as guide: [Getting started with Azure Cosmos DB: Graph API – Code Samples](https://docs.microsoft.com/en-us/samples/azure-samples/azure-cosmos-db-graph-gremlindotnet-getting-started/azure-cosmos-db-graph-gremlindotnet-getting-started/)

I wanted to apply this to exploring our holidays dataset from the [Azure Open Datasets](https://azure.microsoft.com/en-us/services/open-datasets/#overview). Unfortunately, their documentation shows it in the Python space and not in other languages, so there does not seem to be an easy way to get the data from Azure Open Datasets to C#. But I am still looking into that.

### Gremlin Server Settings

In order to create our connection to the Azure Cosmos DB Gremlin API, we first need to create a Gremlin Server. We need the following information:

- Host – the host name we stored in $env:\Host
- Port
- enableSsl – whether SSL is enabled
- username – which involves the path to the collection including the Database and Container values. In our example, we are looking at the graphmas Database and the holidays collection.
- password – the primary key that we stored in `$env:\PrimaryKey`

```csharp
string containerLink = "/dbs/" + Database + "/colls/" + Container;

var gremlinServer = new GremlinServer(Host, Port, enableSsl: EnableSSL, 
                                        username: containerLink, 
                                        password: PrimaryKey);
```

### Gremlin Client Settings

The Gremlin Client is responsible for sending commands to the Gremlin server. We initialize it in our Main and pass it around as needed.

```csharp
using (var gremlinClient = new GremlinClient(
    gremlinServer, 
    new GraphSON2Reader(), 
    new GraphSON2Writer(), 
    GremlinClient.GraphSON2MimeType, 
    connectionPoolSettings, 
    webSocketConfiguration))
{

  ...
}
```

Some things to note about creating the Gremlin client:

- The `GremlinServer` object needs to be created for this.
- For Azure Cosmos DB Gremlin API as of November 2021, it needs to use GraphSON v2. We are using Gremlin.Net 3.4.9 for this demo. If you want to use Gremlin.Net 3.5.x, know that GremlinClient.GraphSON2MimeType is no longer there.
- The connection pool and web socket configuration are copied from the Getting started code sample.

### Menu Details

To run this app from a Terminal in the MerryGraphmas folder, use the following command dotnet run

The menu shows queries with examples of exploring the vertices and edges as well as graph traversal.

```
Gremlin Queries
===============
1. Get Vertex Count
2. Get Unique Vertex Labels
3. Get Edge Count
4. Get Unique Edge Labels
5. Get All Vertices
6. Get All Holiday Vertices
7. Get All Country Vertices
8. Get All Edges
9. Get First Vertex
10. Get All Properties and Values for a Holiday vertex
11. Get All Vertices with a holidayDate property
12. Get All Vertices without a holidayDate property
13. Get the Number of Holidays by Date
14. Get the Holidays Ordered by Date, then Name
15. Get Holidays Between Colombia and Mexico
16. Get Holidays Between Colombia and Mexico - Path
17. Get Holidays Between Colombia and Mexico - Execution Profile
What query would you like to run? Select 0 to quit.
```

My main goal in this app is to show some queries with Gremlin and how to throw Gremlin queries to a Gremlin API using Gremlin.Net. I made sure to include how to explore a graph when getting dropped in, answering questions such as:

- How many vertices are there? How many edges are there?
- What are the labels for the vertices and edges?
- How do you select vertices of a specific label?
- How do you limit the number of results that come back?
- How do you look at properties and their values?
- How do you filter vertices?
- How do you see the path of a graph traversal?
- How do you profile Gremlin queries?


Each option returns the following type of output:

```
What query would you like to run? Select 0 to quit.
2
        Query: [Get Unique Vertex Labels, g.V().label().dedup()]
        Result:
        "Country"
        "Holiday"

        StatusAttributes:
        ["x-ms-status-code"] : 200
        ["x-ms-total-server-time-ms"] : 65.2307
        ["x-ms-total-request-charge"] : 25.43
```

The output includes:

- Query – menu option text as well as the Gremlin query
- Result – the output, as seen in the JSON portion of the Azure Data Explorer interface
- Status Attributes – which contains the status code, the total server time to execute the query, and the total request charge

## Introducing the Data

Here is a sample graph from the data, show the relationships of Boxing Day.

![Relationships between Boxing Day and countries]({{site.baseurl}}/assets/images/posts/2021-12-01/boxing-day-relationships.png)

In our example, we have:

- Many countries (green vertices) celebrate (arrows point to) Boxing Day (orange vertex). Those edges are labeled `celebrates`.
- Boxing Day has an additional edge (arrow coming out of it) that is pointed to United Kingdom. The label for this edge is `originated in`.

There are 105 vertices and 110 edges. There are 2 types of vertices – labeled Holiday and Country. There are 2 types of edges – labeled **celebrates** and **originated in**.

## Running Queries in Azure Data Explorer

Using Azure Data Explorer in my Azure Cosmos DB Gremlin API resource, I can see that there are 38 countries and 67 holidays. The query to get that is: `g.V().group().by('label').by(values('name').count())`

![Azure Data Explorer is showing the **sd-graphmas** Azure Cosmos DB Gremlin API resource, looking at the graphmas/holidays graph.  The query mentioned above appears in a tab labeled Graph.  The results appear below the query in a section labeled JSON.]({{site.baseurl}}/assets/images/posts/2021-12-01/azure-data-explorer.png)


While the .NET Core Console Application executes Gremlin queries, you can also copy and paste them into Data Explorer to see the graph when appropriate. In this example, Azure Data Explorer is showing the sd-graphmas Azure Cosmos DB Gremlin API resource, looking at the graphmas/holidays graph. The query mentioned above appears in a tab labeled Graph. The results appear below the query in a section labeled JSON.

When there's an appropriate graph, the graph appears in its own Graph section, between the JSON and Query Stats sections. Consider the example `g.V().has('name','Colombia')`:

![Azure Data Explorer with the query mentioned above and the resulting graph, showing Colombia between Navidad and La Inmaculada Concepción.]({{site.baseurl}}/assets/images/posts/2021-12-01/graph-view.png)


## Making the Graph Readable

The graph initially starts out by showing IDs. In the command bar, select **Style**. Set the vertex to show as **name**. Set the property to use for the node color as **label**. Change the Show from Targets to **All neighbors**. Then click **Ok**.

![Change the style of the graph in Azure Data Explorer]({{site.baseurl}}/assets/images/posts/2021-12-01/change-style.png)

This makes it a lot easier to understand what nodes you are working with and how those relationships connect. The edge labels are not seen on the graph, though they are seen on the Properties pane on the right. You must have a node selected in order for the details to show up. When collapsed, this pane is labeled Properties.

![Showing the Edge labels in the Properties pane on the right hand side of Azure Data Explorer]({{site.baseurl}}/assets/images/posts/2021-12-01/vertex-properties.png)


So if you run the console application and find you want to see the graph and trace out the traversals while understanding the query, I highly recommend running these queries in Azure Data Explorer.

## Background Assumptions

When I wrote this demo app, I specifically kept in mind a C# developer who may not have seen Gremlin before and who may not be familiar with graph databases

## Vertices and Edges

Vertices and edges make up the **graph**. A **vertex** is a thing, and an **edge** is a relationship.

In our example above, the holidays and the countries are our vertices. Most relationships are representing countries celebrating a holiday. There is one exception for Boxing Day originating in United Kingdom.

Notice that the direction matters on these relationships. The arrowheads are key.

## Gremlin Guidance

Queries start with `g` which represents the graph – the collection of vertices and edges. The queries are read from left to right. The pieces of a Gremlin query are split by a period (`.`). They start with a graph and then go into either the collection of vertices or the collection of edges.

The collection of vertices is represented by `V()`. The collection of edges is represented by `E()`. These are seen coming from the `g`. The query to get all vertices is `g.V()`. The query to get all edges is `g.E()`.

There are many things you can do, including:

- Filtering
- Aggregating
- Ordering

Many of the queries in the .NET Core Console Application show how to do these things.

## Graph Traversal Guidance

When navigating a graph statement, it is important to understand the collections of vertices and edges in reference to where you are in the traversal. Consider the following query, which gets from Colombia to Mexico via holidays.

```gremlin
g.V().has('name','Colombia')
	.outE()
	.inV()
	.inE()
	.outV()
	.has('name','Mexico')
```

I have broken the query out into multiple lines to better see the traversal. Let's talk about what out and in represent.

### Vertex Perspective

We start our journey with the vertices, specifically looking at the vertex with its name property set to Colombia.

The `outE()` gets all edges coming out of Colombia. If we were concerned about any edges coming into Colombia, we could refer to those as `inE()`. As there are no relationships going into Colombia, it doesn't have any results for its `inE()`. As we are concerned about getting to Mexico from Colombia, we will follow the edges out.


For vertices, the `outE` and `outELabel` represent the edges that are coming out of the vertex. The `inE` and the `inELabel` represent the edge is going into the vertex.

![Diagram of holidays celebrated by Colombia]({{site.baseurl}}/assets/images/posts/2021-12-01/vertex-perspective.png)

### Edge Perspective

Once we're at the edges coming out of Colombia, we need to navigate the other vertices on those edges. Colombia is where the edge is coming **out** from. In order to get the vertices where the edges are going into, we need to call `inV()` after the `outE()`.

For edges, the `outV` and `outVLabel` represent the vertex that the edge is coming out of. The `inV` and the `inVLabel` represent the vertex the edge is going into.

### Traversing the Graph

Let's get back to our query:

```gremlin
g.V().has('name','Colombia')
	.outE()
	.inV()
	.inE()
	.outV()
	.has('name','Mexico')
```

So we have illustrated through the `inV()`. We are currently at the holidays celebrated by Colombia. One of these holidays is celebrated by Mexico. To get there, we need to continue traversing the graph, like so:

- Get all the other countries that celebrate these holidays. First – the edges for those relationships: `inE()`
- Get all the countries (vertexes) where these **celebrates** edges are coming out of: `outV()`
- Filter down to the vertex that has the **name** property set to **Mexico**: `.has('name','Mexico')`

When looking at that query, it might be hard to follow at first in trying to keep the ins and outs straight. I have found that running this query in Azure Data Explorer and seeing the Graph representation has made it easier to keep those traversals straight.

## Conclusion

Play around with the .NET Core Console Application to learn more about the queries and how we apply the graph traversal knowledge while exploring the graph. If you want to see the graph and follow along the traversals, copy the queries over into Azure Data Explorer in the Azure portal for your Azure Cosmos DB Gremlin API resource.

## Additional Resources

If you want to learn more about Azure Cosmos DB Gremlin API or Gremlin, check out the following resources:

- [Azure Cosmos DB Cheat Sheets](https://docs.microsoft.com/en-us/azure/cosmos-db/sql/query-cheat-sheet)
- [Azure Cosmos DB Gremlin graph support and compatibility with TinkerPop features](https://docs.microsoft.com/en-us/azure/cosmos-db/graph/gremlin-support)
- [Practical Gremlin: An Apache TinkerPop Tutorial](https://www.kelvinlawrence.net/book/Gremlin-Graph-Guide.html) by Kelvin R. Lawrence
- [Gremlin Cheat Sheet](https://dkuppitz.github.io/gremlin-cheat-sheet/101.html) by Daniel Kuppitz