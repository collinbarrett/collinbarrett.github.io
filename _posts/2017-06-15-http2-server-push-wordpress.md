---
title: 'Activating HTTP/2 Server Push with WordPress'
date: '2017-06-15T07:00:19-05:00'
excerpt: 'A quick and dirty test of HTTP/2 Server Push on WordPress.'
layout: post-wp-import
permalink: /http2-server-push-wordpress
image: /assets/img/http2ServerPush_collinmbarrett.jpg
categories:
- Code
tags:
- Cache
- CDN
- Chrome
- Cloudflare
- CSS
- HTTPS
- JavaScript
- NGINX
- Performance
- WordPress
---

## HTTP/2 Server Push Primer

One of the supposed benefits of the new HTTP/2 specification is [Server
Push](https://httpwg.org/specs/rfc7540.html#PushResources). I have been dabbling with the feature off and on for awhile,
but I wanted to actually run some quick performance analysis as well as document a way to implement it on a WordPress
site hosted on NGINX. Traditionally, the core process of loading a page involves the following:

1. Browser requests the page root document
2. Server sends the page root document
3. Browser parses root document and requests any additional resources
4. Server sends additional resources

With HTTP/2 Server Push, step 3 is essentially eliminated. When a server receives a request for a document, it can now
anticipate which additional resources the browser would request to complete the rendering of the page and send those
before the browser even requests them. This sounds like a performance boost in theory; but, in my quick testing, I have
found that mileage varies.

## Initial Test Results

Using the fantastic [WebPageTest](https://www.webpagetest.org/) tool, I performed three test batches, each with nine
runs.

### Aggregated Data

The table below shows the aggregated results for the median run in each batch (except for the row marked “Mean” which is
the mean of all nine). For the first batch, I ran the test using all of the other performance features I have built into
my site, including HTTP/2, save for HTTP/2 Server Push. The second batch used HTTP/2 Server Push for all scripts and
stylesheets. The last batch added HTTP/2 Server Push additionally to the .woff and .woff2 font files as well as the site
logo .png.

| Median Run | [None](https://www.webpagetest.org/result/170615_1Q_2JV/3/details/#waterfall_view_step1) | [JS &amp;
CSS](https://www.webpagetest.org/result/170615_2A_2H5/7/details/#waterfall_view_step1) | [JS, CSS, Fonts,
Logo](https://www.webpagetest.org/result/170615_8F_2CP/9/details/#waterfall_view_step1) |
|---|---|---|---|
| Load Time | 1.588s | 1.709s | 1.706 |
| Load Time (Mean of 9 Runs) | 1.401s | 1.834s | 1.897s |
| First Byte | 0.250s | 0.236s | 0.286s |
| Start Render | 1.366s | 1.282s | 1.579s |
| Visually Complete | 2.168s | 2.171s | 2.571s |
| WebPageTest Speed Index | 1789 | 1740 | 2059 |
| Document Complete Time | 1.588s | 1.709s | 1.706s |
| Fully Loaded Time | 1.631s | 1.750s | 1.712s |

I was expecting speeds to improve from left to right in that table. Unfortunately, that is not the case. Some argument
can be made that batch two (pushing scripts and stylesheets) did outperform in some metrics such as the WebPageTest
Speed Index, however not using HTTP/2 Server Push at all appears to be the most performant for this particular page.

### Waterfalls

The waterfall charts (click to enlarge) are where you can really see what this feature is doing. When no assets are
pushed, resources are received by the full round-trip request/response lifecycle and not all in parallel.

<figure aria-describedby="caption-attachment-4222" class="wp-caption aligncenter" id="attachment_4222"
    style="width: 249px">[![No HTTP/2 Server Push
    Waterfall](/assets/img/noServerPush_collinmbarrett-249x300.jpg)](/assets/img/noServerPush_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-4222">No HTTP/2 Server Push Waterfall -
        [WebPageTest](https://www.webpagetest.org/result/170615_1Q_2JV/3/details/#waterfall_view_step1)</figcaption>
</figure>

Applying HTTP/2 Server Push to the scripts and stylesheets downloads all of those resources in parallel and they require
far less time to acquire. The files are now downloaded from the browser's perspective on the order of around 10ms on
average vs. more like 25ms when not pushed. This disparity would likely be even greater if I were not using a global CDN
(Cloudflare) to deliver assets from nodes near the client.

<figure aria-describedby="caption-attachment-4223" class="wp-caption aligncenter" id="attachment_4223"
    style="width: 249px">[![HTTP/2 Server Push Scripts & Stylesheets
    Waterfall](/assets/img/scriptsStylesheetsServerPush_collinmbarrett-249x300.jpg)](/assets/img/scriptsStylesheetsServerPush_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-4223">HTTP/2 Server Push Scripts &amp; Stylesheets
        Waterfall - [WebPageTest](https://www.webpagetest.org/result/170615_2A_2H5/7/details/#waterfall_view_step1)
    </figcaption>
</figure>

The true median run when I applied HTTP/2 Server Push to fonts and the logo .png as well produced a bit of an anomaly
waterfall. As you can see below, all of these assets were downloaded immediately and in parallel as we'd expect.
However, the download times for the assets were much higher than expected. If we look at [all of the other runs in the
batch](https://www.webpagetest.org/result/170615_8F_2CP/), though, this seems to be due to a fluke of some kind (such as
network congestion).

<figure aria-describedby="caption-attachment-4224" class="wp-caption aligncenter" id="attachment_4224"
    style="width: 269px">[![HTTP/2 Server Push Scripts, Stylesheets, Fonts, & Logo
    Waterfall](/assets/img/allServerPush_collinmbarrett-269x300.jpg)](/assets/img/allServerPush_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-4224">HTTP/2 Server Push Scripts, Stylesheets, Fonts,
        &amp; Logo Waterfall -
        [WebPageTest](https://www.webpagetest.org/result/170615_8F_2CP/9/details/#waterfall_view_step1)</figcaption>
</figure>

This was a quick unscientific test, to be sure, but it seems to indicate that, at least for this particular page, HTTP/2
Server Push produces a mixed back regarding performance change. I would need to test this on multiple devices and
multiple network connections to get a bit better feel as to whether it is a keeper or not.

## WordPress Implementation

Unfortunately, applying HTTP/2 Server Push to resources is not automatic. For now, you have to hint what resources to
push manually. The key prerequisite is that your web server supports HTTP/2.

With WordPress, two plugins that I have tested automatically handle this for scripts and stylesheets. [HTTP/2 Server
Push](https://wordpress.org/plugins/http2-server-push/) is a fantastic standalone plugin that does just that. Install
and activate it, and it should go into effect immediately.

If you are a CloudFlare user, the [CloudfLare plugin](https://wordpress.org/plugins/cloudflare/) can do essentially the
same thing without needing to install a second plugin. To enable HTTP/2 Server Push with this plugin, just add the line
below to wp-config.php. More info
[here](https://support.cloudflare.com/hc/en-us/articles/115002816808-How-do-I-enable-HTTP-2-Server-Push-in-WordPress).

```
define('CLOUDFLARE_HTTP2_SERVER_PUSH_ACTIVE', true);

```

These plugins can only apply the necessary hints to scripts and stylesheets, however. My understanding is that this is
due to the current queueing architecture of WordPress resources. If you are using NGINX as a web server, though, you can
add the hints via headers in the server block of your NGINX .conf file. Something like below should do the trick.

```
add_header link "</assets /img/logo_collinmbarrett.png>; rel=preload; as=image";
add_header link "</wp-content /themes/collinmbarrett/fonts/lato-regular-webfont.woff2>; rel=preload; as=font;
crossorigin";

```

## Wrap-Up

I have not yet decided my strategy is going forward for this feature. I want to love it, but I love results more. I
think it requires case-by-case analysis and tuning. Give it a shot for your sites, and let me know how it works for you.