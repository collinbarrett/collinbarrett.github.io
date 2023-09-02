---
id: 4160
title: 'Sending the Referrer Policy Header with NGINX'
date: '2017-06-13T12:41:00-05:00'
author: 'Collin M. Barrett'
excerpt: 'A brief overview of the Referrer Policy header and how to send it with NGINX.'
layout: post-wp-import
guid: '/?p=4160'
permalink: /referrer-policy-header-nginx
redirect-from: /referrer-policy-header-nginx/
image: /assets/img/privacyReferrerPolicy_collinmbarrett.jpg
categories:
- InfoSec
tags:
- Chrome
- HTTPS
- NGINX
- Tracking
- WordPress
---

I periodically run the collection of websites that I maintain through a variety of testing tools to monitor their
security and performance. One of these tools, [securityheaders.io](https://securityheaders.com/), recently added a check
for the [new Referrer Policy header](https://scotthelme.co.uk/a-new-security-header-referrer-policy/). I dug in a little
to learn about the header and to begin sending it via NGINX on my sites.

## Referrer Header

When we click a link on the internet, part of the HTTP specification is to send a referrer header to the next page you
are visiting. This information allows the next page to see where you came from and is quite valuable to sites for
optimizing lead funnels, SEO, etc. It is received with some concern, however, by the informed, privacy-minded user who
may not want to signal the last page they visited.

## Blocking with noreferrer

There has been an existing mechanism for sites to request that the referrer header not be sent by adding the
“noreferrer” keyword to a link. The example below demonstrates the HTML implementation. It is up to the browser to
respect that keyword, but most modern versions of mainstream browsers do to my knowledge.

```
<pre class="brush: xml; light: true; title: ; notranslate" title="">
&lt;a rel="noreferrer" href="https://collinmbarrett.com"&gt;
```

## Blocking with Referrer Policy Header

But, what if the site wants to specify this site-wide rather than adding the keyword on each link tag? The new Referrer Policy header allows for websites to define the policy that they desire web browsers to follow, and it also provides more granular options for when to send it and what content to include.

I will not re-post the definitions here, but there are a variety of policies to choose from including the following: no-referrer, no-referrer-when-downgrade, same-origin, origin, strict-origin, origin-when-cross-origin, strict-origin-when-cross-origin, and unsafe-url. The policy selected has varying impact and implications for the browser on what to include and when to send the referrer header. See [here](https://www.w3.org/TR/referrer-policy/#referrer-policies) for more info on the policies.

Note that browser support varies as of the time of writing since this is a relatively new feature. When I first tried to implement the “same-origin” policy on one of my sites, I saw the error below in the Chrome console. Chromium [only supports some of these policies](https://bugs.chromium.org/p/chromium/issues/detail?id=627968&q=Referrer-Policy%20header%20strict&colspec=ID%20Pri%20M%20Stars%20ReleaseBlock%20Component%20Status%20Owner%20Summary%20OS%20Modified) currently. Until the Chromium team resolves this issue, I think I will keep my sites at “no-referrer” to err on the side of privacy. If “same-origin” gets more widely adopted, I will try that so as to protect the integrity of my internal analytics.

> Failed to set referrer policy: The value ‘same-origin’ is not one of ‘no-referrer’, ‘no-referrer-when-downgrade’, ‘origin’, ‘origin-when-cross-origin’, or ‘unsafe-url’. The referrer policy has been left unchanged.

## Sending Referrer Policy Header with NGINX

Sending the referrer policy with NGINX is pretty simple. Just add something like the following inside a server block in the NGINX .conf files. Then, restart NGINX.

```
##
# Referrer Policy
##

add_header Referrer-Policy "no-referrer";

```

We can verify that NGINX is sending the header by inspecting the resource in Chrome. Open the Developer Tools, toggle to the Network tab, and refresh the page. Clicking on the root document of the site shows various request details including this header value.

<figure aria-describedby="caption-attachment-4199" class="wp-caption aligncenter" id="attachment_4199" style="width: 300px">[![Referrer Policy in Chrome Developer Tools](/assets/img/referrerPolicyChrome_collinmbarrett-300x159.jpg)](/assets/img/referrerPolicyChrome_collinmbarrett.jpg)<figcaption class="wp-caption-text" id="caption-attachment-4199">Referrer Policy in Chrome Developer Tools</figcaption></figure>

## Blocking as a Visitor

All this got me to thinking what I could do, if anything, as a user to block the referrer header from being sent. Web developers can utilize one or both of the two methods above, but what if I want the sites I visit to not know from where I came independent of their decision to implement a Referrer Policy? It seems like there may be some plugin-based solutions like [this one](https://chrome.google.com/webstore/detail/noref/dkpkjedlegmelkogpgamcaemgbanohip), but that is an area I certainly want to investigate further.