---
id: 3554
title: 'NGINX Trailing Slashes for WordPress'
date: '2017-03-02T07:28:50-06:00'
author: 'Collin M. Barrett'
excerpt: 'How to configure a NGINX rewrite rule to always include trailing slashes in WordPress while not breaking the REST API.'
layout: post
guid: 'https://collinmbarrett.com/?p=3554'
permalink: /nginx-trailing-slashes-wordpress/
wp_featherlight_disable:
    - ''
image: /media/nginxTrailingSlashesForWordPress_collinmbarrett.jpg
categories:
    - Code
tags:
    - Chrome
    - Memphis
    - MemTech
    - NGINX
    - SEO
    - WordPress
---

Just a quick tip that was not easily discoverable on the inter-webs.

## Trailing Slash Primer

An URL without a trailing slash looks like: `https://collinmbarrett.com/nginx-trailing-slashes-wordpress`

An URL with a trailing slash looks like: `https://collinmbarrett.com/nginx-trailing-slashes-wordpress/`

The decision to include the slash or not for basic web pages seems to be somewhat subjective and up for debate. Certainly, sites should pick one and be consistent for SEO purposes; Google does not like [duplicate content](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html). Amongst WordPress circles (including sites like [wordpress.com](https://wordpress.com), [yoast.com](https://yoast.com/), and [tommcfarlin.com](https://tommcfarlin.com/trailing-slash-in-wordpress/)), the consensus seems to be to include the trailing slash for posts and pages. Files and API calls (more on the latter below) should not contain a trailing slash.

## NGINX Rewrite Rule

### Conventional Rewrite

There are lots of examples for a NGINX rewrite rule to force WordPress pages always to have the trailing slash. Variations of the following seem to be the most popular, and I have been using this rule for some time with success.

```
rewrite ^([^.]*[^/])$ $1/ permanent;

```

*via [Bob Monteverde](https://stackoverflow.com/questions/645853/add-slash-to-the-end-of-every-url-need-rewrite-rule-for-nginx/3912675)*

## Excluding WordPress REST API Calls

However, with the inclusion of the new WordPress [REST API](http://v2.wp-api.org/) in WordPress Core, the rewrite rule above breaks things. I discovered this when trying to enable the [Markdown](https://jetpack.com/support/markdown/) feature of [Jetpack](https://jetpack.com/). I watched API calls in the Chrome console get redirected to include a trailing slash and then fail. I was unable to discover a solution with Google-fu, but with some help from the local Slack community, I pieced together the ‘if()’ below. It seems to do the trick.

```
##
# Always Include Trailing Slash (less REST API calls and files)
##

if ($request_uri !~ "^/wp-json") {
        rewrite ^([^.]*[^/])$ $1/ permanent;
}

```

Shout out to [Ed](https://twitter.com/edyesed) in the [MemTech Slack](https://memphistechnology.org/blog/2015/05/19/join-memtech-on-slack-chat/) for pointing me in the right direction.