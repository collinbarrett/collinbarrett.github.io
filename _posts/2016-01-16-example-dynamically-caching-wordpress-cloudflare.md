---
id: 677
title: 'Example of Dynamically Caching WordPress on Cloudflare'
date: '2016-01-16T10:45:41-06:00'
author: 'Collin M. Barrett'
excerpt: 'The performance improvements speak for themselves in this example of dynamically caching WordPress on
CloudFlare from a traditional shared hosting server.'
layout: post-wp-import
guid: '/?p=677'
permalink: /example-dynamically-caching-wordpress-cloudflare
redirect-from: /example-dynamically-caching-wordpress-cloudflare/
image: /assets/img/exampleDynamicallyCachingWordPressOnCloudFlare_collinmbarrett.png
categories:
- Code
tags:
- Cache
- CDN
- Chrome
- Cloudflare
- Compression
- CSS
- DigitalOcean
- DNS
- Family
- Friends
- Google
- HTML
- HTTPS
- JavaScript
- Performance
- SEO
- WordPress
---

A few days ago I discussed [Dynamically Caching WordPress on Cloudflare](/wordpress-cloudflare-dynamic-caching/). I saw
an improvement on my site after implementing this technique, but I wanted to test it out on a more real-world and
long-standing blog on traditional shared hosting (vs. the bleeding-edge [SSD VPS
stack](https://github.com/collinbarrett/wp-vps-build-guide "wp-vps-build-guide - GitHub") at
[DigitalOcean](https://www.digitalocean.com) on which my site is hosted). Suzanne, a friend from college, graciously let
me put [The Glorious Mundane](https://www.suzannehines.org/ "Suzanne Hines") to the test in this example of dynamically
caching WordPress on Cloudflare.

Suzanne is an excellent and consistent writer. She maintains one of the few personal blogs in my RSS reader that gets
updated on a truly regular basis, something I am striving for this year on my own blog. Her posts tend to be full of
high-resolution images of her children; and those, combined with other 3rd-party assets, tend to slow down her page
loads. I asked if I could help her grow her audience by optimizing her site a bit more for speed and, in turn, SEO
results.

## TL;DR

Implementing full-page caching via [Cloudflare](https://www.cloudflare.com/) reduced page load times by up to *40%*.

## Methodology

I will be honest up front in saying that my testing methodology was not exactly scientific (day job, right?). I used
[webpagetest.org](https://www.webpagetest.org/) on one of her newer blog posts that contains a pretty typical amount of
pictures and text. I was unable to test her homepage accurately as my before and after tests were split by a few days,
allowing a few of her scheduled posts to modify her homepage in between. This split between the tests could have also
allowed for drastically different conditions on the shared hosting server or on the network itself, but I believe the
data below does show a substantial improvement assuming no extreme variation in the environment. Each test (a
combination of a first view and a repeat view to check browser caching) was run three times consecutively on a virtual
Chrome instance in Dallas. I recorded the median of the three runs in the tables below, and I verified that none of the
six total runs were outliers. So, the median data should be sufficient to get an accurate view of the improvements.

## Before

| First View Load Time | Repeat View Load Time | Page Size |
|---|---|---|
| 9.197s | 3.237s | 3.245MB |

## Optimizations

I believe the most useful improvement I performed was the full page caching on Cloudflare; however several other small
optimizations were made between tests as well, that should be noted. All of her images were losslessly compressed via
[Kraken](https://kraken.io/?ref=0dc255f82a7e "Affiliate Link"), accounting for the minor adjustment in page size. While
the reduction in size was relatively small (average of 10%), the bandwidth saved should prove beneficial for limited
bandwidth visitors such as mobile users or her friends and family in Niger. Cloudflare also has better DNS servers
nominally and provides HTML, CSS, and JS minification, both of which do help as well.

## After

| First View Load Time | Repeat View Load Time | Page Size |
|---|---|---|
| 5.616s | 3.087s | 2.963MB |

We were able to shave off more than 3.5s from the first view load time for a reduction of 40%. By serving the entire
page content from Cloudflare’s closest CDN node to the client, shared hosting server response time and potentially
longer round trip time has been almost entirely eliminated. While 5.6s is still not great, it is substantially better
for visitor experience and SEO than the prior 9.2s. Furthermore, since Cloudflare also applied further browser caching
headers to all content served from the primary domain, hopping around the site after the first-page load does feel
significantly more snappy than before since the browser maintains a local cache of much of the content.

## Future Optimizations

I prepared for Suzanne a collection of optimizations that would require more manual adjustments on a post-by-post basis.
By inserting a WordPress-generated smaller version of each image into her posts (and then potentially linking to the
full-resolution copy), the amount of data required for image downloads could be significantly reduced. I also wanted to
convert her site over to, at a minimum, Cloudflare’s “Flexible SSL” so that we could realize the concurrency and other
improvements of HTTP/2. This, however, requires fixing a lot of hard-coded “HTTP” references which is beyond the scope
of what I was able to do.

I also attempted to migrate her site to my VPS to improve her server response times, but I am suspending that idea at
the moment due to some issues with [VaultPress](https://vaultpress.com/) restores. This improvement would only assist
her when working in the WordPress dashboard, though, since all of her public-facing pages are now fully cached with
Cloudflare.

## Conclusion

If you manage a WordPress instance that is not terribly dynamic (comments and posts do not flow in more frequently than
every few hours), I highly recommend [implementing full page caching on
Cloudflare](/wordpress-cloudflare-dynamic-caching/ "Dynamically Caching WordPress on CloudFlare").