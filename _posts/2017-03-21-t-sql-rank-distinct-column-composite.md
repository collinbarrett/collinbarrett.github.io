---
title: 'SELECT Most Recent for Each Distinct Column Composite with T-SQL RANK()'
date: '2017-03-21T20:35:22-05:00'
author: 'Collin M. Barrett'
excerpt: 'A T-SQL solution using the RANK() function for selecting the most recent record for each distinct composite of
multiple columns.'
layout: post-wp-import
permalink: /t-sql-rank-distinct-column-composite
redirect_from: /t-sql-rank-distinct-column-composite/
image: /assets/img/tSqlRank_collinmbarrett.png
categories:
- Code
tags:
- Database
- 'fred''s'
---

I know enough SQL to perform basic data wrangling, but when it comes to more advanced queries, I still have a lot to
learn. At work today, I ran into the following problem. It took a bit of time to piece together a solution using T-SQL
RANK(), so I am documenting it for posterity (mostly so I can refer to it when I undoubtedly forget how to do this in
the future). I am happy to entertain suggestions for improvement.

## Requirement

My team is developing a retail pricing application. When price changes are processed at corporate and sent to the
stores, the application archives the records in a processed table (SQL Server). We need a reporting solution that shows
the application users all prices that are currently active at the stores.

To do this, I needed to create a view selecting the most recent (yet not exceeding the current DateTime) records for
each distinct Group/Store/Vendor/SKU combination in the processed table.

## A Solution

Transact-SQL’s RANK() function…

> Returns the rank of each row within the partition of a result set. The rank of a row is one plus the number of ranks
that come before the row in question.
> — <cite>[MSDN](https://docs.microsoft.com/en-us/sql/t-sql/functions/rank-transact-sql)</cite>

By passing in the four columns that I am using as a kind of composite key for the desired result set as the values for
PARTITION BY, I can select only the records with the most recent EffectiveDate for each given unique 4-column
combination.

The only other gotcha in my scenario is that EffectiveDate could potentially be in the future, in which case I should
return the most recent EffectiveDate that is not in the future. I handled this with a simple WHERE clause.

## The View Query

```
SELECT Id,
Group,
Store,
Vendor,
Sku,
Price,
EffectiveDate
FROM
(
SELECT P.Id,
P.Group,
P.Store,
P.Vendor,
P.Sku,
P.Price,
P.EffectiveDate,
RANK() OVER(PARTITION BY P.Group,
P.Store,
P.Vendor,
P.Sku ORDER BY P.EffectiveDate DESC) AS RecencyRank
FROM dbo.Prices AS P WITH (NOLOCK)
WHERE P.EffectiveDate &amp;lt;= GETDATE()
) AS NonFutureAndRecencyRanked
WHERE RecencyRank = 1;
```

*Schema modified to protect intellectual property.*

*via [StackOverflow](https://stackoverflow.com/questions/612231/how-can-i-select-rows-with-maxcolumn-value-partition-by-another-column-in-mys/612408#612408)*