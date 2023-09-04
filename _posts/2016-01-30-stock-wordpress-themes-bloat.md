---
title: 'Beware the Bloat of Stock WordPress Themes'
date: '2016-01-30T09:32:01-06:00'
author: 'Collin M. Barrett'
excerpt: 'The why and how to removing bloat in stock WordPress themes by deregistering unused scripts and styles.'
layout: post-wp-import
permalink: /stock-wordpress-themes-bloat
redirect_from: /stock-wordpress-themes-bloat/
image: /assets/img/stockWordPressThemesBloat_collinmbarrett.jpg
categories:
- Code
tags:
- CSS
- JavaScript
- Performance
- PHP
- SEO
- WordPress
---

## Theme Bloat

When spinning up a cheap budget WordPress site, it is common and far more affordable to purchase a stock premium
WordPress theme from a marketplace like Themeforest than to build a custom theme. The quality of these themes vary
widely; but, with adequate research, I have been able to find pretty solid themes for not a lot of money over the years.
The theme this site used to run,
[Readme](https://themeforest.net/item/readme-a-readable-wordpress-theme/9167043?ref=collinbarrett&clickthrough_id=1029644360&redirect_back=true
"Themeforest Affiliate Link"), is a phenomenal, lightweight theme that still packs a good number of features.
Furthermore, their ongoing support has been excellent.

Any theme made for the broad market has features that not everyone will use. Readme, for example, includes a lot of
jQuery and CSS files for interesting features; however, I am choosing not to use many of them at this time. Even though
a theme does not use the features, is still instructs WordPress to load up all these extra files to my clients
increasing page load time and wasting bandwidth.

## Eliminating the Bloat

So, by stepping through each .js and .css file loaded, I determined which files I needed for the current feature set
that I am using and eliminated the rest. Deregistering unneeded scripts and styles is relatively simple, assuming you
have a child theme setup already. For example, in the snippet below I remove the share button feature by deregistering
its script and style in my child theme's functions.php.

```
function remove_scripts()
{
wp_deregister_script( 'selection-sharer' );
wp_deregister_style( 'selection-sharer' );
}
add_action( 'wp_enqueue_scripts', 'remove_scripts', 100 );
```

After eliminating this batch of CSS and jQuery plugins, I do see some moderate improvements. It does not work magic, but why load that extra ~100kb of data and process an additional seven server requests when they are not needed at all? If you are running a WordPress stock theme and looking to get an extra ounce of performance, I recommend trying this out.

<figure aria-describedby="caption-attachment-1343" class="wp-caption aligncenter" id="attachment_1343" style="width: 248px">[![Pingdom Test Before Removing Bloat](/assets/img/readmeBefore_cb-248x300.jpg)](/assets/img/readmeBefore_cb.jpg)<figcaption class="wp-caption-text" id="caption-attachment-1343">**Fig. 1.** Pingdom Test Before Removing Bloat</figcaption></figure>

<figure aria-describedby="caption-attachment-1342" class="wp-caption aligncenter" id="attachment_1342" style="width: 252px">[![Pingdom Test After Removing Bloat](/assets/img/readmeAfter_cb-252x300.jpg)](/assets/img/readmeAfter_cb.jpg)<figcaption class="wp-caption-text" id="caption-attachment-1342">**Fig. 2.** Pingdom Test After Removing Bloat</figcaption></figure>