---
id: 2004
title: 'FilterLists, Gorhill, Jekyll, WordPress Plugin, and PHP'
date: '2016-02-29T08:11:48-06:00'
author: 'Collin M. Barrett'
excerpt: 'FilterLists got a bit of publicity and I began working on a new feature, learning Jekyll, PHP, and WordPress plugin development along the way.'
layout: post
guid: 'https://collinmbarrett.com/?p=2004'
permalink: /filterlists-gorhill-jekyll-wp-plugin-php/
wp_featherlight_disable:
    - ''
image: /media/filterListsGorhillJekyllWpPluginPhp_collinmbarrett.jpg
categories:
    - Code
tags:
    - Advertisements
    - Cache
    - FilterLists
    - HTML
    - JavaScript
    - Performance
    - PHP
    - Tracking
    - 'uBlock Origin'
    - WordPress
---

This was a good weekend of progress for side projects and learning. A couple weeks ago, I released [FilterLists](https://filterlists.com/) mainly to scratch my own itch because a similar resource was not available anywhere online. This weekend, the project got a tiny bit of publicity, and I began working on a first major new feature for the directory, learning a bit of [Jekyll](https://jekyllrb.com/), PHP, and WordPress plugin development along the way.

## Raymond Hill ([Gorhill](https://github.com/gorhill "Raymond Hill - GitHub"))

Gorhill is the maintainer and primary developer of [uBlock Origin](https://github.com/gorhill/uBlock "GitHub"), an alternative to [Adblock Plus](https://adblockplus.org/) with many more features, better performance, and a “better” business model (free, no donations sought, no paid whitelisting). I was tipped on Saturday afternoon that Gorhill had discovered FilterLists and was [considering](https://github.com/gorhill/uBlock/issues/1432#issuecomment-189686064 "uBlock Origin - GitHub Issues") linking to it in the official uBlock Origin wiki. Raymond suggested, however, that it would be preferred if FilterLists was open-source.

## Jekyll

I spent several hours on Saturday fiddling around with Jekyll, a static blog-aware site generator with official support by [GitHub Pages](https://pages.github.com/). If I could migrate FilterLists to a Jekyll site hosted on GitHub Pages, it would allow me to offload server maintenance, and better crowdsource the upkeep of FilterLists. I was able to get a quick mock-up on a local development server with Jekyll, but I quickly realized that losing the many features of a dynamic CMS like WordPress would sacrifice too much functionality for the FilterLists roadmap. Additionally, since the content of a WordPress install is stored in a database, I am not sure that I could effectively open source the WordPress site on GitHub as it stands. FilterLists is theoretically still open source, however, as the code for WordPress, the plugins in use, and the actual HTML content are all freely available.

## WP Plugin

Instead, I decided to use my remaining free weekend hours to begin adding a new feature to FilterLists that I think users will find beneficial. While FilterLists aims to be comprehensive, many of the lists are no longer actively maintained. I thought it would be useful to be able to see, at first glance, the last modified date of the lists.  
There were four different methods I brainstormed for acquiring the last modified dates of the lists.

Parsing the “Last-Modified” HTTP response header.

- Con: Most of the .txt files are not served with a Last-Modified header.

Scraping the “Last modified: ” date as reported on the list.

- Pro: Executed (ideally) asynchronously server-side.
- Con: Not all lists contain “Last modified: “.
- Con: Dependent on reported date in the list, not actual last modified.

Calling the Javascript function document.lastModified on each list.

- Pro: Most consistently available version of date from all lists.
- Con: Would seemingly require some weird, likely impossible cross-site scripting.

Maintaining a cache of all of the lists and comparing them to the most recent version from their origin server.

- Pro: The most accurate method for active lists.
- Con: Most time-intensive to develop.
- Con: Most resource-intensive to retrieve, cache, and parse all of the lists at regular intervals.
- Con: Non-functional for lists already stale/inactive.

For my first attempt, I opted for an easier version of the second option described above. In the first pass, I built a simple shortcode WordPress plugin, [Link Last Modified Checker](https://github.com/collinbarrett/link-last-modified-checker "GitHub"), to retrieve the provided URL and parse out the reported “Last-modified: ” date. This is currently working and live in alpha for the first five lists on [FilterLists](https://filterlists.com/).

The problem is that, as it stands, when a request arrives at the server for the FilterLists directory, the server goes out to each list, parses the date, and compiles the final HTML response all in real-time. For five lists, this works in a reasonable amount of time, but if I enable it on all lists, the request would either take forever or likely timeout. I have a live question over on the [WordPress Development StackExchange](https://wordpress.stackexchange.com/questions/219224/how-to-periodically-scrape-and-cache-strings-from-remote-txt-files-my-first-p "How to periodically scrape and cache strings from remote txt files.") as to how to do this asynchronously which already has a solid answer that I hope to follow this evening.

## Wrapping Up

In summary, FilterLists is now featured on the [uBlock Origin Wiki](https://github.com/gorhill/uBlock/wiki/Filter-lists-from-around-the-web "GitHub"), a new product feature is well underway, and I am more familiar with Jekyll, PHP, and WP plugin development.