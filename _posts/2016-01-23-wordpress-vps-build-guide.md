---
id: 1011
title: 'DIY VPS Build Guide for Fast and Cheap WordPress Hosting'
date: '2016-01-23T10:25:00-06:00'
author: 'Collin M. Barrett'
excerpt: 'A verbose build guide for a modern, high-performance production WordPress VPS. Stack: Ubuntu, NGINX, MariaDB,
HHVM, Redis, Let''s Encrypt.'
layout: post-wp-import
guid: '/?p=1011'
permalink: /wordpress-vps-build-guide
redirect_from: /wordpress-vps-build-guide/
image: /assets/img/wordPressVpsBuildGuide_collinmbarrett.jpg
categories:
- Code
tags:
- Cache
- Cloudflare
- Compression
- Database
- DigitalOcean
- Encryption
- Facebook
- Google
- HTTPS
- Linux
- NGINX
- Performance
- PHP
- WordPress
---

**Update 7.19.16:** The guide on GitHub has been dramatically updated since the below post was written. I am working on
a revised post, but for now, I would recommend just jumping over to the [GitHub
project](https://github.com/collinbarrett/wp-vps-build-guide#wp-vps-build-guide) rather than spending much time reading
this post. Thanks!

When it comes to hosting [WordPress](https://wordpress.org/), there are more options than ever before. I began, as many
others do, using budget shared hosting from [GoDaddy](https://www.godaddy.com/) and
[BlueHost](https://www.bluehost.com/). Soon, however, I realized that shared hosting just does not cut it for a site of
any decent functionality or size. There is a current batch of WordPress-optimized hosting from companies like [WP
Engine](https://wpengine.com/), [StudioPress Sites](https://wpengine.com/more/studiopress-sites/#pricing-tiles), or
[SiteGround](https://www.siteground.com/) that provide an excellent service, but the price usually reflects that. I
wanted to host my sites on a modern, cutting edge stack without paying the premium of these providers. I wanted to build
a DIY WordPress VPS, something I accomplished for $5/mo. with [DigitalOcean](https://www.digitalocean.com/).

So, for the last few years, I have been piecing together and updating a guide so that I could remind myself how to build
the server. The steps are quite verbose, without any build automation such as Ansible or Puppet, so as to reduce the
learning curve and to iterate rapidly without extra overhead. I decided to “open source” the guide on GitHub recently
(link at the end of the post), so I hope that it can help a few others as well.

## The Stack

| Component | Solution | Notes |
|---|---|---|
| Development Client | macOS | |
| Production Host | DigitalOcean | |
| Server | Ubuntu LTS x64 | |
| WordPress Management Tools | WP-CLI | |
| Database | MariaDB | |
| Object Cache Store | Redis | |
| PHP Compiler | HHVM | |
| Web Server | NGINX | w/microcaching |
| Connection | Modern TLS Ciphers HTTP/2 ipv4/ipv6 | |

## Design Decisions

The first inspiration I received to try this stack was [Mark Jaquith](https://markjaquith.com/)’s talk, “[Next
Generation WordPress Hosting
Stack](https://wordpress.tv/2014/10/16/mark-jaquith-next-generation-wordpress-hosting-stack/ "WordPress TV").” It is
worth a listen, and I based this build guide largely on Mark’s talk.

### Host OS

I selected [Ubuntu](https://ubuntu.com/) due to its low barrier to entry and a broad range of support. Other options may
have been leaner, but Ubuntu is fast, stable, and well-documented. The current LTS (long term support) version, Ubuntu
14, by default, uses an older kernel, however. I chose to upgrade the kernel to capture some more recent improvements,
but some may choose not to do so for stability purposes.

### Web Server

Since I am relatively new to the industry, I did not have a lot of history with [Apache](http://www.apache.org/).
[NGINX](http://nginx.org/) is being regarded by experts as very responsive, lean, and memory efficient. While none of my
sites are very demanding yet, I have found that NGINX does, in fact, perform very well. Implementing micro caching via
FastCGI helps reduce the I/O back to the PHP engine, further reducing the latency of NGINX.

### Database Server

I chose [MariaDB](https://mariadb.org/) purely based on others’ opinions that it slightly edges out
[MySQL](https://www.mysql.com/) in performance, etc. I do not have a strong preference, however, between MariaDB, MySQL,
or [Percona](https://www.percona.com/). Any of them should work fine. It is important to perform at least some basic
optimizations on the database server, though, such as enabling query caching. There are a variety of tuning scripts,
such as [MySQLTuner](https://github.com/major/MySQLTuner-perl), which help novice database administrators tweak this
further.

### PHP Execution Engine

The battle between [PHP-FPM](https://php-fpm.org/) and Facebook’s newer [HHVM](https://hhvm.com/) for PHP processing is
heating up with the recent release of PHP 7. The benchmarks that I see still show HHVM outperforming php7-fpm, though it
is much closer than with previous iterations of PHP-FPM.

### Object Caching

Object caching via [Redis](https://redis.io/) is the last server-side optimization that reduces the server’s latency.
Because WordPress relies on large queries to the database every time it serves a page, caching chunks of these query
results in memory saves a significant amount of time and load on the database server.

### Encryption and Transfer Protocol

Also, in this guide, I describe how to configure TLS encryption using the new and free [Let’s
Encrypt](https://letsencrypt.org/) service (I have recommended a 4096-bit RSA key because I am a bit of a security nut,
but a 2048-bit key should be perfectly fine). Not only is encryption so easy today, and highly recommended since
WordPress collects credentials and other personal information, but it paves the way for configuring HTTP/2. NGINX now
supports HTTP/2 natively which provides quite a few in-transit optimizations such as compressing headers and handling
concurrent requests much more efficiently.

### Deprecated

I spent quite awhile testing and using Google’s [PageSpeed
Module](https://developers.google.com/speed/pagespeed/module/) (also available for Apache) to perform optimizations on
the front-end DOM contents before leaving the server. Minification, image compression, etc. are all valuable ways of
speeding up page load times, but I was never able to get the benefits to justify themselves in my setups. The latency
introduced by PageSpeed’s processing outweighed the benefits.

### Not Tested

Additional technologies that other hosts use include varnish caching, Apache instead of NGINX, or
[memcached](https://memcached.org/) instead of Redis for object caching. I have not worked with any of these; however,
from my research, they are outperformed by the selections currently in the guide.

## Conclusion

These are the decisions that I have landed on after some research and testing over the past years, but I fully expect
the stack to morph over time as the technologies change. The guide, as I said, is somewhat verbose. If you want to
turbocharge your WordPress site on a small budget, give it a shot. If you are experienced in operations and see
something that should be improved or changed, please let me know or submit a pull request.
[wp-vps-build-guide](https://github.com/collinbarrett/wp-vps-build-guide#wp-vps-build-guide)