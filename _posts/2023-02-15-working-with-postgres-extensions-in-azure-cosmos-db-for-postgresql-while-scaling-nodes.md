---
title: Working with Postgres Extensions in Azure Cosmos DB for PostgreSQL while Scaling Nodes
tags: azure-cosmos-db postgres scalability
---

Yesterday, I did a presentation for the [Azure Cosmos DB Global User Group](https://bit.ly/3lhOiE9) on [Azure Cosmos DB for PostgreSQL](https://bit.ly/3InHNJ1). I mentioned in the PostGIS part that I ran into an issue, so I wanted to describe it here.

**Problem**: I installed PostGIS on my single-node cluster without issues. However, I scaled my cluster to 2 nodes afterwards. When I ran the query that uses ST_X and ST_Y from PostGIS, I got the following error:

```
ERROR:  type "public.geometry" does not exist
CONTEXT:  while executing command on private-w0.azure-cosmos-db-global-ug-demo.postgres.database.azure.com:5432
```

When I read the CONTEXT message, I realized by the w0 reference that the worker nodes didn’t have PostGIS installed. When you scale the nodes – at least in this case, it doesn’t enable the extensions over there.

## My Solution

I looked at [the PostgreSQL extensions in Azure Cosmos DB for PostgreSQL page](https://learn.microsoft.com/en-us/azure/cosmos-db/postgresql/reference-extensions?WT.mc_id=DT-MVP-4025435) to see if there was guidance there, but I saw nothing there. I did make note, though, that `create_extension()` works for the PostgreSQL command `CREATE EXTENSION`.

So then I looked at the PostgreSQL documentation to see if there was a DROP equivalent – because SQL shows patterns of DROP statements that match CREATE statements. I came across [DROP EXTENSION](https://www.postgresql.org/docs/current/sql-dropextension.html). So this works on a single node… will this also work with the distributed cluster?

So I guessed that `drop_extension()` might also exist in Azure Cosmos DB for PostgreSQL. It does! And it worked! I dropped the extension and then recreated it, which created it on all of my nodes – 1 coordinator node and 2 worker nodes. This is what I had to run:

```sql
SELECT drop_extension('postgis');
SELECT create_extension('postgis');
```