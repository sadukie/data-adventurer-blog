---
title: "Azure Cosmos DB ❤️ PostgreSQL"
url: '/2022/11/02/azure-cosmos-db-❤%ef%b8%8f-postgresql/'
tags: azure-cosmos-db postgres citus azure
---

When I talk about Azure Cosmos DB, I am usually talking about one of the APIs in the NoSQL realm. I've got a graph database talk that's making its rounds – next up at [.NET Conf 2022](https://www.dotnetconf.net/agenda). For the [Festive Tech Calendar](https://festivetechcalendar.com/) this year, I will be showing their document database by using Azure Cosmos DB for MongoDB. Those are topics for another day. However, today, I want to tell you about Azure Cosmos DB for PostgreSQL.

Coming from a relational data background and having my mind put Azure Cosmos DB on the NoSQL side, my mind melted down when I first heard about Azure Cosmos DB for PostgreSQL. It kept going “But… Azure Cosmos DB… but [Postgres](https://www.postgresql.org/docs/current/history.html)…. but NoSQL… but SQL…” and then finally … 💥.

I had worked a bit with Postgres long ago, so I knew I was back in the relational realm. However, I suspected that there was a twist if Azure Cosmos DB was involved. When I think about Azure Cosmos DB and data, I think about scale – [large amounts of data](https://learn.microsoft.com/azure/cosmos-db/introduction?WT.mc_id=DT-MVP-4025435), [horizontal scaling](https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview?WT.mc_id=DT-MVP-4025435), and [partitioning](https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview?WT.mc_id=DT-MVP-4025435). Sure enough, Azure Cosmos DB for PostgreSQL allows for horizontal scaling of Postgres data with the help of an open source extension known as [Citus](https://www.citusdata.com/).

This is SQL at scale. If you are coming into this with a general understanding of SQL but aren't sure how this changes things in data modeling, I recommend checking out [this Microsoft Learn module on modeling data for Azure Cosmos DB for PostgreSQL](https://learn.microsoft.com/en-us/training/modules/model-data-azure-cosmos-db-postgresql/). The scenario in the module starts off with a relational intent and talks through the considerations when distributing data. This is another great resource for data modeling with Azure Cosmos DB for PostgreSQL: [How to do data modeling for distributed Postgres in Azure Cosmos DB – Events](https://learn.microsoft.com/en-us/events/azure-cosmos-db-liftoff/how-to-do-data-modeling-for-distributed-postgres-in-azure-cosmos-db)

You may be wondering… **When would I choose Azure Cosmos DB for PostgreSQL over PostgreSQL?** This is a managed platform, meaning you do not have to worry about the hardware or operating system level updates. Azure takes care of that for you. When you have **large amounts of data (such as 100+ GBs of data)**, then you need to think about growth and on a platform that is meant to support it. When you're getting **beyond the limits of a single node**, the scaling offered by Azure Cosmos DB for PostgreSQL makes sense. The cases made in this article by Claire Giordano on the Microsoft Community Hub really apply: [When to use Hyperscale (Citus) to scale out Postgres – Microsoft Community Hub](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/when-to-use-hyperscale-citus-to-scale-out-postgres/ba-p/1958269).

If you want to learn more about Azure Cosmos DB for PostgreSQL, check out the following resources:

- [Azure Cosmos DB for PostgreSQL learning path on Microsoft Learn](https://learn.microsoft.com/training/paths/azure-cosmos-db-for-postgresql/?WT.mc_id=DT-MVP-4025435)
- [Azure Cosmos DB: Liftoff](https://learn.microsoft.com/en-us/events/azure-cosmos-db-liftoff/) – a post Ignite event that showcases everything Azure Cosmos DB for PostgreSQL
- [Citus Con: An Event for Postgres (2022)](https://learn.microsoft.com/en-us/events/citus-con-postgres-2022/) – great way to learn more about the Postgres + Citus reakn