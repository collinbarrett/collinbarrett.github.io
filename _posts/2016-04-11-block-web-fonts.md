---
title: 'Blocking Web Fonts for Speed and Privacy'
date: '2016-04-11T04:17:47-05:00'
author: 'Collin M. Barrett'
excerpt: 'How to configure your browser to block web fonts to speed up your web browsing and protect your privacy.'
layout: post-wp-import
permalink: /block-web-fonts
image: /assets/img/blockWebFonts_collinmbarrett.jpg
categories:
- InfoSec
tags:
- Advertisements
- Cache
- Chrome
- FilterLists
- Firefox
- Google
- HTTPS
- Performance
- Tracking
- 'uBlock Origin'
---

## A Primer on Web Fonts

The developer of a website has two options for selecting a font in which he or she would prefer you to read the site's
text. They can choose from a set of “[web safe fonts](https://www.cssfontstack.com/ "CSS Font Stack")” that are assumed
to be locally installed on most of their visitors' devices, or they can direct the visitor's browser to download a
custom web font before rendering the page. Since the selection of fonts that are available by default on substantially
all devices is so narrow, most developers with any desire for consistent and quality design will opt to use a web font.

While a designer can go to great lengths to tweak how a site looks for the visitor, like anything else on the internet,
the final power is up to the visitor and his or her browser configuration. You have the choice of allowing your browser
to download web fonts; and, since downloading web fonts can have some negative impact, I choose to block many of them.
If you care to do the same, the rest of this piece will explain why and how.

## Why Block Web Fonts?

### Bandwidth

As a user of the internet, I want the web, of course, to be visually appealing. However, I also want the web to be fast.
If I am trying to load a one-off news article for a quick peruse, I do not necessarily care to wait for my browser to
download unnecessary resources such as advertisements, social media widgets, and web fonts.
Furthermore, when using my rated data plan on my mobile device, I do not always care to pay for bandwidth used to
download mere aesthetics such as fonts.

### Security

While the risk and frequency of security vulnerabilities are relatively small with web fonts, there is still a precedent
for concern. On occasion, there have been remote code execution vulnerabilities such as [this
one](https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2015/ms15-044 "Vulnerabilities in Microsoft
Font Drivers Could Allow Remote Code Execution") in 2015. Such problems can be quickly patched and pose a relatively
little threat to the average internet citizen, but there is always a possibility of being in the wrong place at the
wrong time when a yet unknown vulnerability could occur.

### Privacy

Additionally, most of the time web fonts are downloaded from a third-party delivery network such as [Google Web
Fonts](https://fonts.google.com/) or [Adobe Fonts](https://fonts.adobe.com/). One of the advantages of using these
systems is that the likelihood that a visitor's browser already has a font cached locally from a previous visit to any
other site using the same font is relatively high. If browser caches the font, a re-download does not need to be
initiated. Whether the font is cached locally or not, however, the client browser still creates an extra HTTP(S)
connection to the third-party domain.

There are some privacy concerns when your browser makes the link to the font network's domain. While they deny the
practice, these services have the ability of profiling your browser (using browser fingerprinting with
your IP address and HTTP referer header) to track what sites you visit, building a valuable profile about you that they
could sell to marketers. I like to think the best of companies, but since I cannot know for sure what business decisions
they make behind closed doors, I choose to take whatever steps I can to protect my privacy.

### Censorship

Lastly, as a quick note to developers, it must be mentioned that oppressive governments like China forcefully block many
Google services. Different network-related censoring could automatically block Google Web Fonts or its competitors, so
their utility does not serve as a perfect global option for styling your web property.

<figure aria-describedby="caption-attachment-2630" class="wp-caption alignright" id="attachment_2630"
    style="width: 263px">[![uBlock Origin
    Logo](/assets/img/uBlockOrigin_collinmbarrett-263x300.png)](/assets/img/uBlockOrigin_collinmbarrett.png)<figcaption
        class="wp-caption-text" id="caption-attachment-2630">*Fig. 1.* uBlock Origin Logo</figcaption>
</figure>

## Prerequisites

While there are, I am sure, many more ways to block web fonts; my current favorite is using the uBlock Origin extension
to filter out requests for web fonts. For this guide, it is assumed that you have both of the following installed on
your device.

- Supported Browser: [Firefox](https://www.mozilla.org/en-US/firefox/) or [Chrome](https://www.google.com/chrome/)
- uBlock Origin - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/ "uBlock Origin for Firefox") |
[Chrome](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en "uBlock Origin
for Chrome") | [GitHub](https://github.com/gorhill/uBlock "uBlock Origin on GitHub")

## Blocking Schemes

There are a few different ways to block web fonts that I am going to describe. Pick your poison, but I marked my
recommendation. These methods could be joined and remixed in a variety of configurations, but the three generic options
to follow are suitable as a starting point.

### Nuclear Scheme: Block All

#### Description

If you have bought in 100% and wanted to block all web fonts, here is the nuclear option. Before proceeding, know that
you are blocking web fonts downloaded from third-party networks as well as fonts being served directly from the
first-party domains of the sites you visit. All text including the wording of this very article will only render in
basic web safe fonts that are pre-installed on your device.

Furthermore, the modern web makes extensive use of icon fonts. These particular fonts are collections of symbols that
can dramatically affect the usability of websites. (The magnifying glass icon in the top right corner of this site is an
example of an icon font glyph.) By blocking all web fonts, you might find that many websites are hard to navigate
because icons such as arrows, hamburger menus, etc. are not displayed. Continue at your own risk.

#### Instructions

<figure aria-describedby="caption-attachment-2644" class="wp-caption alignright" id="attachment_2644"
    style="width: 300px">[![uBlock Origin Web Fonts
    Toggle](/assets/img/uBlockOriginWebFontsToggle_collinmbarrett-300x174.jpg)](/assets/img/uBlockOriginWebFontsToggle_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-2644">*Fig. 2.* uBlock Origin Web Fonts Toggle
    </figcaption>
</figure>

To implement this scheme, just click “Block remote fonts” in the “Settings” tab of uBlock Origin. If you want to loosen
the restrictions to allow web fonts for your favorite trusted sites or for sites that are unusable without their icon
fonts, you can do so on a site-by-site basis. Just toggle the font icon circled in blue in Fig. 2 (resembling a capital
“A”) back on in the uBlock Origin GUI while viewing the site to be whitelisted.

### Strict Scheme: Allow Only First-Party (Recommended)

#### Description

This option blocks all web fonts from third-party providers but allows the browser to download fonts from first-party
domains. I prefer this method as it does not require the extra connection to a third-party domain (speed and privacy),
and since I trust the first-party site enough to connect to it, so I do not take on much additional privacy or security
risks when I allow my browser to download a font directly from them.

#### Instructions

Add the following filter to the “My filters” tab in uBlock Origin:

```
*$font,third-party

```

If you want to loosen the restrictions for your favorite sites, you can do so on a site-by-site basis. Just modify the
filter rule you added above to include the first-party domains for which you would like to allow third-party web fonts
to be downloaded. Example as follows:

```
*$font,third-party,domain=~example.com|~example2.net

```

*via [Gorhill](https://github.com/gorhill/uBlock/issues/363#issuecomment-191796040 "uBlock Origin GitHub Issue") and
[IDKwhattoputhere](https://github.com/gorhill/uBlock/issues/363#issuecomment-199870634 "uBlock Origin GitHub Issue")*

### Easier Scheme: Block Curated Third-Party

#### Description

There is a great curated list of filters that you can subscribe to in uBlock Origin that will block the most common
third-party font domains. This option tends to be a bit kinder on allowing icon fonts to pass through, so I recommend
starting here if you consider yourself a beginner.

#### Instructions

If you already have uBlock Origin installed on the browser that you are currently reading this article in, just click
the “Add” link below to subscribe automatically to this list.

Fanboy's Anti Third-Party Fonts - [View](https://fanboy.co.nz/fanboy-antifonts.txt "View Fanboy's Anti Third-Party
Fonts") | [Add](abp:subscribe?location=https://fanboy.co.nz/fanboy-antifonts.txt&title=Fanboy%20Third-Party%20Fonts "Add
Fanboy's Anti Third-Party Fonts to uBlock Origin or Adblock Plus")

## Hope for the Future

Storage is cheap. While I do not have high hopes of this occurring soon, it would be great to see a standards body
develop an extensive set of free fonts that should be pre-installed on all operating systems across all devices. This
local cache would allow designers to exhibit creativity while still reducing the bandwidth and privacy concerns of using
web fonts.

## Want to Learn How to Block Something Else?

Check out [FilterLists](https://filterlists.com/), a site I created to curate all of the known content blocking lists
that you can subscribe to in tools like uBlock Origin.