---
id: 4120
title: 'SELECT All HAVING Non-Distinct Composite Column with T-SQL'
date: '2017-06-11T12:50:47-05:00'
author: 'Collin M. Barrett'
excerpt: 'A T-SQL solution using HAVING and COUNT for selecting all records with a non-distinct composite column.'
layout: post
guid: '/?p=4120'
permalink: /t-sql-select-all-having-non-distinct-composite-column/
image: /assets/img/tSqlRank_collinmbarrett.png
categories:
- Code
tags:
- Database
- 'fred''s'
---

## Requirement

This week, the cross-functional team that is testing the application I have been building discovered a data issue where
a table that should only contain one record for each (`[Sku]`, `[GroupCode]`) pair had more than one in some cases. This
table was not designed to have (`[Sku]`, `[GroupCode]`) as a composite key (there is an `[Id]` identity column as a
key), allowing this issue to occur.

Below is a sample of what the table `[Prices]` could have contained.

| Id | Sku | GroupCode | Price |
|---|---|---|---|
| 1 | 1 | 1 | 1.00 |
| 2 | 1 | 1 | 2.00 |
| 3 | 1 | 2 | 1.00 |
| 4 | 2 | 1 | 1.00 |
| 5 | 2 | 1 | 2.00 |
| 6 | 2 | 2 | 1.00 |

To diagnose why this issue (such as records where `[Id] IN (1,2)`) was happening, I needed a query to select only the
records that contained more than one (`[Sku]`, `[GroupCode]`) composite.

## A Solution

I did not have a lot of time to test my Google-Fu for this one, so I received assistance from a BI developer who sits
across the hall from me. She suggested a query like below.

```
<pre class="brush: sql; title: ; notranslate" title="">
SELECT COUNT([Sku] + [GroupCode]) AS 'Count',
[Sku],
[GroupCode]
FROM [Prices]
GROUP BY [Sku], [GroupCode]
HAVING COUNT([Sku] + [GroupCode]) &gt; 1
```

I had used the `HAVING` keyword before, but I had not used it extensively enough to think to apply it to this problem off the top of my head. The difference between `HAVING` and `WHERE` is quite subtle; but, for this situation, I think `HAVING` is required since I wanted to select duplicate composite columns after the `GROUP BY` has already been applied.

Furthermore, I was interested in treating `[Sku]` and `[GroupCode]` like a single composite column. So, concatenating the two columns fulfills this requirement. The query above, however, adds the two columns rather than concatenates because the columns are `INT`s. So, I cast the `INT`s to `VARCHAR`s below.

```
<pre class="brush: sql; title: ; notranslate" title="">
SELECT COUNT(CAST([Sku] AS VARCHAR(20)) + CAST([GroupCode] AS VARCHAR(20))) AS 'Count',
[Sku],
[GroupCode]
FROM [Prices]
GROUP BY [Sku], [GroupCode]
HAVING COUNT(CAST([Sku] AS VARCHAR(20)) + CAST([GroupCode] AS VARCHAR(20))) &gt; 1
```

The result set from the sample `[Prices]` table is below and is what I needed to diagnose further why the issue was occurring.

| Count | Sku | GroupCode |
|---|---|---|
| 2 | 1 | 1 |
| 2 | 2 | 1 |