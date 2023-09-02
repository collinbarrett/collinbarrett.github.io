---
id: 4654
title: 'Hardening Firefox. Replacing Chrome.'
date: '2017-08-07T07:07:11-05:00'
author: 'Collin M. Barrett'
excerpt: 'Using Firefox instead of Chrome on the desktop, and how I am hardening Firefox to increase security and privacy.'
layout: post
guid: '/?p=4654'
permalink: /hardening-firefox-replacing-chrome/
wp_featherlight_disable:
    - ''
image: /assets/img/firefox_collinmbarrett.jpg
categories:
    - InfoSec
tags:
    - Advertisements
    - CDN
    - Chrome
    - Encryption
    - Firefox
    - Google
    - HTTPS
    - Performance
    - Tracking
    - 'uBlock Origin'
---

## The Case for Firefox

I have been on a quest for some time to reduce my exposure to Google products. I switched from Gmail to [Protonmail](https://protonmail.com/) and from Google Search to [StartPage](https://www.startpage.com/). Google is generally on the cutting edge of performance, features, standards, and security in many of their products. The trade-off, however, is handing over incredible amounts of personal data to a company whose business model is using my data for marketing and product development.

A product as fundamental as a desktop web browser is difficult to replace. I have tried reverting from Chrome to Firefox many times over the last few years, but the performance difference was always too noticeable. As of late, however, Firefox is making great strides in performance, and I have recently switched back to it as my daily driver.

## Hardening Firefox

### User.js

Firefox, by default, is designed with security and privacy as a top priority. Even as such, there are still many ways that the browser can leak personally identifiable data. [User.js](https://github.com/pyllyukko/user.js) is a fantastic open-source project which provides a customized profile for tightening up all of the potential weak points left open in a default Firefox installation. This file disables many new HTML APIs that trackers can use to profile the user, locks down Add-ons and plugins, disables various forms of telemetry, hardens cryptography and cipher suites, and [much more](https://github.com/pyllyukko/user.js#what-does-it-do).

By default, this user.js file breaks some functionality of many popular sites. If that is the case, there is a [relaxed branch](https://github.com/pyllyukko/user.js/tree/relaxed) which provides a bit more of a balance between usability and ultimate privacy. Alternatively, you can determine which settings need to be toned down for your specific needs and adjust them as needed. That is the path I am following by updating my own [fork](https://github.com/pyllyukko/user.js/compare/master...collinbarrett:master) of the project.

### Add-ons

To further harden Firefox, I am currently using the following Add-ons:

- [CanvasBlocker](https://addons.mozilla.org/en-US/firefox/addon/canvasblocker/) – To block canvas fingerprinting.
- [Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes/) – To block tracking by content delivery networks. [Learn more](/decentraleyes-block-cdn-tracking/).
- [Tampermonkey](https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/) – To run the [Anti-Adblock Killer](https://reek.github.io/anti-adblock-killer/) user script.
- [HTTPS Everywhere](https://addons.mozilla.org/en-US/firefox/addon/https-everywhere/) – To prioritize encrypted connections.
- [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) – In [Hard Mode](https://github.com/gorhill/uBlock/wiki/Blocking-mode:-hard-mode). To block advertisements, tracking scripts, and undesirable/unneeded content.