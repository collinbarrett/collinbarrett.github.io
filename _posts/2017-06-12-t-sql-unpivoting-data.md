---
id: 4150
title: 'UNPIVOTing Data with T-SQL'
date: '2017-06-12T12:26:36-05:00'
author: 'Collin M. Barrett'
excerpt: 'An example of how to use the T-SQL UNPIVOT operator to transform columns to rows.'
layout: post
guid: '/?p=4150'
permalink: /t-sql-unpivoting-data/
wp_featherlight_disable:
    - ''
image: /assets/img/tSqlRank_collinmbarrett.png
categories:
    - Code
tags:
    - Database
    - 'fred''s'
---

## Requirement

I had a data set (pricing by state) delivered last week from a business application user that I needed to import into the application’s database. The user provided an Excel worksheet in the format below:

| SKU | TN | MS | AR |
|---|---|---|---|
| 1 | .99 | .89 | .89 |
| 2 | 1.09 | .99 | .99 |
| 3 | .99 | 1.09 | .99 |

My database table, however, was designed as follows:

| Sku | State | Price |
|---|---|---|
| 4 | TN | .89 |
| 4 | MS | .99 |
| 4 | AR | 1.09 |
| 5 | TN | .89 |
| 5 | AR | 1.09 |
| 5 | MS | .99 |

I needed a plan to transform the worksheet data to match my table’s structure.

## A Solution

First, I imported the data from the Excel workbook directly into a new table (`[tmpPrices]`) using the [SSMS Import and Export Wizard](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard).

Then, I could [`INSERT INTO SELECT`](https://www.w3schools.com/sql/sql_insert_into_select.asp) from the imported table using the query below. The `UNPIVOT` operator rotates the `[State]` column identifiers into row values that align with a particular `[SKU]`. The column specified before the `UNPIVOT` operator (`[Price]`) is the one that holds the values that are currently under the columns being rotated. The column that will contain the column values that I rotated follows the operator (`[State]`).

```
<pre class="brush: sql; title: ; notranslate" title="">
INSERT INTO [Prices]
([Sku],
[State],
[Price]
)
SELECT [SKU],
[State],
[Price]
FROM
(
SELECT [SKU],
[AR],
[MS],
[TN]
FROM [tmpPrices]
) p UNPIVOT([Price] FOR [State] IN([AR],
[MS],
[TN])) unpvt;
```

*via [Microsoft](https://docs.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms177410(v=sql.105))*