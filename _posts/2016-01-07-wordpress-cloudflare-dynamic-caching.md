---
title: 'Dynamically Caching WordPress on Cloudflare'
date: '2016-01-07T17:59:26-06:00'
author: 'Collin M. Barrett'
excerpt: 'A how-to guide to speeding up websites by dynamically caching WordPress on Cloudflare including traditionally
non-static content like HTML.'
layout: post-wp-import
permalink: /wordpress-cloudflare-dynamic-caching
redirect_from: /wordpress-cloudflare-dynamic-caching/
image: /assets/img/dynamicallyCachingWordPressOnCloudFlare_collinmbarrett.png
categories:
- Code
tags:
- Cache
- CDN
- Cloudflare
- CSS
- DigitalOcean
- DNS
- HTML
- NGINX
- Performance
- PHP
- SEO
- WordPress
---

I have been using [Cloudflare](https://www.cloudflare.com) and [WordPress](https://wordpress.org/) together for several
years. It is a great choice as a free CDN, and it provides quality DNS and security features as well. As I have perused
the blogosphere for methods to optimize a WordPress install and its underlying hosting stack, Cloudflare has come up
frequently; but I had never discovered this “cache everything” tip before. This week, though, I found that I have been
drastically under-using Cloudflare’s edge caching for the WordPress sites I host. I could turbocharge my sites by
dynamically caching WordPress on Cloudflare.

Before I move on, I want to start by saying that this tactic is probably not beneficial on highly dynamic sites. If a
site has posts going live all the time or maintains a high volume of comments, dynamically caching everything at the CDN
level is probably not ideal. Every time a post or comment is updated, the edge cache has to be purged which requires a
fair bit of overhead. However, for sites that do not change frequently, this tactic is working great.

## Results

The default caching settings in Cloudflare only cache static portions of the site. These are things like images, CSS,
etc. The HTML, though, is typically dynamically generated in a CMS like WordPress, so caching the HTML can prevent
visitors from seeing recent updates. I have been serving everything including HTML from the CDN cache for a couple of
days, though, and the numbers speak for themselves.

<div class="wp-block-image">
  <figure class="aligncenter">[![Before and after enabling full CDN dynamic caching at three U.S.
    nodes.](/assets/img/dynamicCachingCDN_cb-300x241.png)](/assets/img/dynamicCachingCDN_cb.png)
    <figcaption>**Fig. 1.** Before and after enabling full CDN dynamic caching at three U.S. nodes.</figcaption>
  </figure>
</div>Fig. 1 shows the response times for my site from New Relic’s three U.S. nodes. It is very obvious where I flipped
the switch to full page caching, **reducing the response time at both of the West Coast nodes by about 70%**.

<div class="wp-block-image">
  <figure class="aligncenter">[![Before and after enabling full CDN dynamic caching at all global New Relic
    nodes.](/assets/img/dynamicCachingCDN_01_cb-300x182.png)](/assets/img/dynamicCachingCDN_01_cb.png)
    <figcaption>**Fig. 2.** Before and after enabling full CDN dynamic caching at all global New Relic nodes.
    </figcaption>
  </figure>
</div>Fig. 2 shows the average wait time drop across all New Relic Synthetics nodes. Again, the numbers do not lie;
**the wait time is cut globally by about 80%**.

You can feel that speed by just clicking around my website. Lastly, if your site is insanely popular or if you hit it
big on [Product Hunt](https://www.producthunt.com "Product Hunt"), Cloudflare will handle the massive influx of traffic
with ease. Fig. 3 below shows 500 clients per second connecting to my homepage over 1 minute without any real problems.
This is all served using only a $5 [DigitalOcean](https://www.digitalocean.com/) Droplet.

<div class="wp-block-image">
  <figure class="alignright">[![Fig. 3. Load Test of 500 Clients per Sec. for 1 Min. Connecting to collinmbarrett.com
    after Dynamically Caching WordPress on
    CloudFlare](/assets/img/dynamicCachingCDN_02_cb-1-300x202.jpg)](/assets/img/dynamicCachingCDN_02_cb-1.jpg)
    <figcaption>**Fig. 3.** Load test of 500 clients per sec. for 1 min. connecting to collinmbarrett.com.</figcaption>
  </figure>
</div>## Part 1. Cloudflare Page Rules

Implementing full page caching with WordPress requires two key components: Cloudflare Page Rules and the “Sunny”
WordPress plugin to purge the CDN cache when the site is updated. Exactly 3 Page Rules are required to get this working
properly, so as long as you do not need additional Page Rules, **Cloudflare’s free plan works just fine**. The page
rules required are below; make sure to order them in the same way as they are listed.

The first rule ensures that Cloudflare does not cache dynamic content in the WordPress backend, which would, at best,
prevent updates in the administrator panel from being visible; or, at worst, allow non-logged-in users to access your
cached administrator dashboard.

***mydomain.tld/wp-admin***

- Custom caching: Default
- Browser cache expire TTL: 30 mins.

The second rule prevents page and post previews from being cached until the editor publishes them.

***mydomain.tld/?p=***

- Custom caching: Default
- Browser cache expire TTL: 30 mins.

The last rule is what then allows all other pages, posts, etc. to be served to visitors from Cloudflare’s CDN rather
than from your origin server. I would recommend setting the browser cache (replacing {custom} in the third rule) for the
public-facing pages to be about the max time between your site getting updated. So, for a blog with a new post daily and
maybe a few comments daily, maybe 12 hours is good. If posts and comments are updated more frequently, drop that number
down to as low as 30 mins. You want visitors to cache your content in their browsers as long as possible without having
them miss new content.

***mydomain.tld/***

- Custom caching: Cache everything
- Edge cache expire TTL: Respect all existing headers
- Browser cache expire TTL: {custom} minutes

## Part 2. Sunny by Typist Tech

At the time of this post, [Sunny](https://wordpress.org/plugins/sunny/) only has about 1,000 active installs, which is
why I want to spread around this tip. This plugin is required if you do not want to log manually into Cloudflare and
purge individual files every time you publish a new post. New posts, comments, etc. will not be visible to your visitors
unless this plugin submits a cache purge API call to Cloudflare on your behalf. This request instructs Cloudflare to
grab the latest of the related new files from your origin server. I have only been using the plugin for a couple of
days, but it seems to work as expected. Thanks, Typist Tech, for the great little plugin. Follow their plugin
installation instructions, and turbocharge your website by dynamically caching WordPress on Cloudflare!

**Update 7.17.16:** This site no longer uses this dynamic caching method. I have implemented NGINX micro caching in its
stead. This change was primarily to take control of the site’s analytics; they are hosted locally with IP addresses
hashed to protect the privacy of my visitors. Locally hosted analytics do not get updated when HTML is cached at the
edge of the CDN.

**Update 2.24.16:** Sunny still does not purge RSS feeds. If this is necessary for your use case, here is a temporary
workaround. Add the following code to your child theme’s functions.php, obviously modifying the URLs to match the feeds
you would like to purge.

*via [GitHub](https://github.com/TypistTech/sunny/issues/2 "Purge RSS Feeds - Sunny")*

```
// purge RSS feed from Cloudflare via Sunny
function purge_rss_cloudflare() {
  Sunny_Purger::purge_cloudflare_cache_by_url( 'https://example.com/feed/' );
  Sunny_Purger::purge_cloudflare_cache_by_url( 'https://example.com/category1/feed/' );
  Sunny_Purger::purge_cloudflare_cache_by_url( 'https://example.com/category2/feed/' );
  Sunny_Purger::purge_cloudflare_cache_by_url( 'https://example.com/category3/feed/' );
}
add_filter( 'publish_post', 'purge_rss_cloudflare' );
add_filter( 'private_to_published', 'purge_rss_cloudflare' );
add_filter( 'edit_post', 'purge_rss_cloudflare' );
add_filter( 'delete_post', 'purge_rss_cloudflare' );
```

**Update 1.20.16:** I have discovered that the current version of Sunny [does not purge RSS feeds from Cloudflare](https://github.com/TypistTech/sunny/issues/2#issuecomment-173085571 "GitHub Issues"). I hope to develop a pull request for this, but just be aware of your caching headers for RSS feeds if you try this method out.

*via [Typist Tech](https://typist.tech/)*