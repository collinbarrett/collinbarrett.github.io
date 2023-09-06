---
title: 'Decentraleyes: Block CDN Tracking'
date: '2016-02-03T04:00:19-06:00'
author: 'Collin M. Barrett'
excerpt: 'Decentraleyes is an add-on for Firefox that blocks CDN tracking by intercepting requests to common JavaScript
libraries and serves them from a local store.'
layout: post-wp-import
permalink: /decentraleyes-block-cdn-tracking
image: /assets/img/decentraleyesBlockCdnTracking_collinmbarrett.png
categories:
- InfoSec
tags:
- Advertisements
- Android
- Apple
- Cache
- CDN
- Cloudflare
- CSS
- Firefox
- Google
- JavaScript
- Performance
- Tracking
---

The modern web uses countless reusable JavaScript, jQuery, and CSS libraries and frameworks to render our favorite sites
and web apps. The progression of this modular design has increased production value while decreasing development cost;
therefore, re-inventing the wheel takes place less and less. Many of these libraries are [hosted on free
CDNs](https://w3techs.com/technologies/overview/content_delivery "Usage of JavaScript content delivery networks for
websites") such as Google Hosted Libraries, jQuery CDN, CDNJS, etc. to reduce the latency to client browsers. While it
seems harmless and beneficial for services to speed up the web for free by serving files to billions of users, these CDN
vendors then have the ability to track and profile end users. HTTP referer headers in conjunction with IP address
tracking and browser fingerprinting can tell these CDNs and their potential advertising customers a lot about us. Our
browsing habits could be analyzed, sold, and combined with other data for profit. Decentraleyes is a great tool that can
help protect against this type of privacy invasion.

## An Analogy

Let's say that, for some reason, every time you make a purchase at a retail store, you had to report to a single
universal organization to let them know who you are, where you are, and what you purchased. This team, in return,
provides you with a convenient and virtually free payment protocol to use in any retail store that you want. That
organization and their advertising partners then have the ability to profile your shopping habits to better market to
you. They provide a (typically) free service to users, often even giving users little kickbacks for using them, in
exchange for a lot of precious profile data. This organization is named “Visa,” and provides a decent analogy for the
intrusion Decentraleyes is aiming to block.

## How It Works

<figure aria-describedby="caption-attachment-1460" class="wp-caption alignright" id="attachment_1460"
    style="width: 300px">[![Decentraleyes for
    Firefox](/assets/img/decentraleyes02_cb-300x223.png)](/assets/img/decentraleyes02_cb.png)<figcaption
        class="wp-caption-text" id="caption-attachment-1460">**Fig. 1.** Decentraleyes for Firefox</figcaption>
</figure>

When a web browser makes a request for a common library from one of these CDNs to render any given site, Decentraleyes
intercepts the request and serves the file from a local store. The concept is simple, but it provides a two-fold
benefit. CDN vendors are unable to track users' habits across the web, and browsing speed and bandwidth use are slightly
optimized because this data is already on the local computer.

One of my most used browser bookmarks, Weather Underground, requires 16 such resources from CDNs. After installing
Decentraleyes, all of these files are served from the local plugin store rather than from the CDN providers. While 50kB
here and 100kB there may not seem like much, this tool will substantially reduce data usage over time on metered data
connections.

## Give It a Shot

Decentraleyes is currently only available for Firefox (Desktop and Android). Install it from the add-on repository (link
below), and you should be good to go. You can configure a few options in the add-on preferences pane. For the most
privacy conscious, there is a checkbox to “block requests for missing resources” which prevents the browser from ever
loading such libraries from a CDN, even if Decentraleyes does not have it stored locally. For most users, leaking this
bit of privacy is worth it to be still able to use non-broken websites. I have been testing the plugin on Mac, Windows,
and Android for over 24 hours, loading over 1,000 instances of libraries from the plugin's store and have been very
pleased. Shout out to [Thomas](https://github.com/Synzvato "Thomas Synzvato - GitHub") for the excellent plugin.

[Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes/)