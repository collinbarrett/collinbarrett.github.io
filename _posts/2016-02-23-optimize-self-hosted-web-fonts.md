---
id: 1725
title: 'Optimizing Self-Hosted Web Fonts'
date: '2016-02-23T04:00:16-06:00'
author: 'Collin M. Barrett'
excerpt: 'By implementing .woff2 and slimming down my icon font, I reduced the weight of my site''s fonts by about 50KB.'
layout: post
guid: '/?p=1725'
permalink: /optimize-self-hosted-web-fonts/
wp_featherlight_disable:
    - ''
image: /assets/img/optimizeSelfHostedWebFonts_collinmbarrett.jpg
categories:
    - Code
tags:
    - Advertisements
    - Apple
    - Cache
    - CDN
    - Cloudflare
    - Compression
    - CSS
    - DNS
    - Family
    - Firefox
    - Google
    - Linux
    - NGINX
    - Performance
    - Tracking
    - 'uBlock Origin'
    - WordPress
---

For anyone who has been here before, you may have noticed my recent font swap-a-roo to Open Sans. I know, designers, it is trendy and probably overused, but I think it brings a much crisper feel to the text than the old serif font I had before. In this transition, I did some font optimization on my WordPress site via recommendations by the [DareBoost](https://www.dareboost.com/en) analyzer. With just an hour or two of tweaking, I was able meaningfully to trim the payload of my site’s first page load. If you are reading this on a mobile data plan, you are welcome.

## TL;DR

By implementing .woff2 and slimming down my icon font, I reduced the weight of my fonts by about 50KB.

## A Primer on Fonts

Fonts are stored in various file formats.

- .eot
- .svg
- .ttf
- .woff
- .woff2

Certain browsers are only capable of rendering certain font formats. The .woff2 type is newest, smallest, and therefore optimal; however only the most modern browsers can support it. The latest release of my web server, NGINX, did not have the .woff2 mime type configured by default.

## Why Optimize Fonts?

<figure aria-describedby="caption-attachment-1733" class="wp-caption alignright" id="attachment_1733" style="width: 300px">[![Poor Example of Serving Web Fonts](/assets/img/2016/02/webFontBadExample_cb-300x84.jpg)](/assets/img/2016/02/webFontBadExample_cb.jpg)<figcaption class="wp-caption-text" id="caption-attachment-1733">**Fig. 1.** Poor Example of Serving Web Fonts</figcaption></figure>

I ran across a cyber-security blog today which I will not name because I like the blog. However, since I have been working with fonts this weekend, I decided to check and see how they were serving fonts. Figure 1 (click/tap to enlarge) is what I saw in Firefox 44.0.2 on Mac. This blog is using almost 1MB (of the only 1.5MB total, including images) of bandwidth just for serving its fonts! Furthermore, the site is forcing a download of four variants of the same font when they are likely only using one of the variations (not confirmed, though). Lastly, of lesser importance but still easily repaired, the site is serving fonts as content type “octet-stream” rather than “font-ttf” which could have performance/compatibility issues with some browsers. To their benefit, though, they were successfully requesting that the browser cache the fonts, so clicking around on their site did not prompt the 1MB download every time.

Optimizing web fonts is necessary for compatibility; but, once that is straightened out, I primarily think it is an important process for performance. Sites that have been online for awhile could use an audit of their fonts now and then make sure they are using the most performant implementation. While trimming 100s or even 10s of KBs from the payload may seem small, in the mobile-first age of the Internet, you are doing your visitors a thankless favor by reducing the data (cents) they are charged when they view your site.

## My Situation

The parent theme (WordPress term for the base theme, which I have heavily modified via a “child” theme) I use on this site was configured to use [Google Fonts](https://fonts.google.com/ "Google Fonts") by default for regular text. This service, and others like it, are extremely popular for their simplicity, selection, and compatibility characteristics. By using a service like this, however, I could be violating the privacy of my visitors by allowing the font provider to collect analytics about what sites my visitors frequent, etc transparently. Additionally, depending on a site’s CDN configuration, there might be a slight performance hit due to the extra DNS lookup and (ideally) TLS handshake that is not required if I serve the font from my domain. For these privacy and performance reasons, I and many other tinfoil hat types have [uBlock Origin](https://github.com/gorhill/uBlock "uBlock Origin - GitHub") reject all connections to web font providers for using a basic, local alternative for sites that do not serve a font from their first-party domain. I did not want viewers of my blog to be subject to reading something as bland as Arial, so I am serving you a font from my first-party domain.

## Serving a Font from My Domain

I acquired the Open Sans font kit from [Font Squirrel](https://www.fontsquirrel.com/fonts/open-sans "Open Sans - Font Squirrel"). By default, Font Squirrel did not include the .woff2 format for Open Sans, which is a little annoying because they do offer the capability. I used their [Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator "Webfont Generator - Font Squirrel") with the original .ttf to acquire the full kit including .woff2 for Open Sans. Font Squirrel is an excellent service providing you with a CSS sheet that looks something like below for cross-browser compatibility of the various font formats.

```
<pre class="brush: css; title: ; notranslate" title="">
@font-face {
font-family: 'open_sanslight';
src: url('opensans-light-webfont.eot');
src: url('opensans-light-webfont.eot?#iefix') format('embedded-opentype'),
url('opensans-light-webfont.woff2') format('woff2'),
url('opensans-light-webfont.woff') format('woff'),
url('opensans-light-webfont.ttf') format('truetype'),
url('opensans-light-webfont.svg#open_sanslight') format('svg');
font-weight: normal;
font-style: normal;
}
```

I uploaded and unzipped the kit on my host, ensuring that I referenced the included stylesheet, and the browser beautifully applied Open Sans to my copy. If you are trying to follow along, the only hiccup here could be modifying the stylesheet to match your directory structure, but that should be pretty simple to debug.

I ran into a gotcha with NGINX, however, and I am not sure if you would see something similar currently in Apache as well. The latest release of NGINX did not support the woff2 mime type, so warnings were thrown in the browser. Modern and powerful browsers should still render the font just fine, but the font should be served as the correct mime type. I fixed it by adding the directive `application/font-woff2 woff2;` to NGINX’s mime types file stored by default on Ubuntu/Debian at `/etc/nginx/mime.types`.

Additionally, I modified my cache-control directives in NGINX to cache .woff2 for a long time. Gzip compression should not be enabled on .woff2, however, as it is already about as compressed as it is going to get and not worth the overhead.

The .woff2 for Open Sans Light downloaded on the first page load of my site is now only a whopping 24.45KB vs. a traditional .ttf which, for Open Sans Light, rings in at 45.37KB. So, this technique will not work miracles for performance, but it certainly does help.

## Bonus: Slimming Icon Fonts

If you are like me and have a theme designed for mass use, it likely uses some icon font as well (for features such as the social media links in the top right corner of my site). My parent theme, for example, uses [Fontello](https://fontello.com/) to create a custom font of approximately 120 icons. Many of these icons I will never actually use on this site. They have a small footprint, but it seems wasteful for me to serve so many unused icons to my visitors. So, I slimmed down my Fontello icon font to only the icons I use. This optimization reduced the icon font’s weight on this site from ~28KB to ~9KB for more than 100% savings (pre-gzip, so their bandwidth usage is even less). Unfortunately, Fontello does not have any way of supporting .woff2 files at this time, which would squeak out even a little bit more efficiency. In addition to slimming down the icon font itself, I slimmed down the correlating stylesheet containing the rules for all of the unused icons.

## Conclusion

If you are a subscriber to the 80/20 rule, optimizing your fonts almost certainly does not fall in the recommended 20%. If you are a performance junkie, however, making sure your site’s fonts are up to snuff might be just what you are looking for to get that extra ounce of pizzazz out of your page loads. If you have any questions about the process, feel free to let me know in the comments below.