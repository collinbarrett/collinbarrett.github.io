---
id: 5058
title: 'HttpContext.Cache Multi-tenant Leak'
date: '2018-01-06T04:00:17-06:00'
author: 'Collin M. Barrett'
excerpt: 'Our team misunderstood the scope of ASP.NET''s HttpContext.Cache. This mishap led to some cross-instance data leakage in our hosting environment. Keys must always have enough unique information to identify their respective cache value.'
layout: post
guid: 'https://collinmbarrett.com/?p=5058'
permalink: /httpcontext-cache-multitenant-leak/
wp_featherlight_disable:
    - ''
image: /media/cpu_multitenantCacheLeak_collinmbarrett.jpg
categories:
    - Code
tags:
    - Cache
    - Database
    - Dotnet
    - Performance
    - 'Shelby Systems'
---

## Call for Caching

The [product](https://collinmbarrett.com/joining-shelby-systems/) I support has chart widgets on several pages. We need to refactor some of their inefficient queries, but we were recently under pressure to achieve a quick [performance](https://collinmbarrett.com/tag/performance/) boost. My team attempted to install [caching](https://collinmbarrett.com/tag/cache/) so that the chart datasets were only retrieved once per day.

## Flawed Approach

A colleague used [ASP.NET](https://collinmbarrett.com/tag/dotnet/)‘s HttpContext.Cache to persist the dataset in memory. The solution trimmed widget names to use as the key and reduced page load speeds on repeat visits.

After the release, we received reports from some customers hosted on shared servers. In a few cases, users saw chart data that they did not recognize. They were presumably seeing data cached from other customers.

> There is one instance of the Cache class per [application domain](https://docs.microsoft.com/en-us/dotnet/framework/app-domains/application-domains). As a result, the Cache object that is returned by the Cache property is the Cache object for all requests in the application domain.  
> — <cite>[MSDN](https://docs.microsoft.com/en-us/dotnet/api/system.web.httpcontext.cache)</cite>

Since the key was not unique to the customer, our environment was sharing cached data across the application domain. We had misunderstood HttpContext.Cache to be scoped instead to a single application instance.

## Resolution

Most of the leaked data were general/summary information and not highly sensitive. We deployed a hotfix in less than twenty-four hours to disable the feature.

A simple fix would be to make use of a piece of customer-specific data in the key to keep datasets unique. Long-term, we should host each instance in an isolated container, but we are not able to support that yet.

## QA Reminder

Until we have each instance isolated, QA should test all changes in a multi-tenant environment.

*<small>Disclaimer: Opinions expressed are solely my own and do not express the views or opinions of my employer.</small>*