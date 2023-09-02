---
id: 5502
title: 'Incognito Debugging With Any Browser in Visual Studio'
date: '2018-01-04T04:00:49-06:00'
author: 'Collin M. Barrett'
excerpt: 'How to configure Visual Studio to debug a web application in any Windows browser''s incognito or private mode.
This solution allows every debugging session to be free of preexisting cache and cookies. Supported in Chrome, Firefox,
Internet Explorer, and Opera. Edge support is not yet available.'
layout: post-wp-import
guid: '/?p=5502'
permalink: /incognito-debug-visual-studio/
image: /assets/img/debugVisualStudioIncognito_collinmbarrett.jpg
categories:
- Code
tags:
- Cache
- Chrome
- Dotnet
- Firefox
- 'Visual Studio'
---

## Clearing the Cruft

When I develop web applications in [Visual Studio](/tag/visual-studio/), I often need to resolve issues that are
dependent on the state of the [cache](/tag/cache/) or cookies. By default, debugging in a browser from Visual Studio
opens a new browser window but does not purge any preexisting cache or cookies.

## Private/Incognito Browsing: Always Fresh

I like to configure Visual Studio to launch browser debug sessions in incognito/private browsing mode. Every debugging
session is then fresh as if a user was visiting my application for the first time.

<figure aria-describedby="caption-attachment-5611" class="wp-caption alignright" id="attachment_5611"
    style="width: 224px">[![Visual Studio Debug Target
    Menu](/assets/img/vsDebugTargetMenu_collinmbarrett.jpg)](/assets/img/vsDebugTargetMenu_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-5611">Visual Studio Debug Target Menu</figcaption>
</figure>

To configure browsers to debug in this mode:

1. Click the Debug Target control’s right-side chevron to view the list of installed browsers.
2. Click “Browse With…”.
3. Click “Add…”.
4. Configure the new target browser using the private/incognito command line argument. See the examples below.

## Sample Browser Configurations

### [Chrome](/tag/chrome/)

<dl>
    <dt>Program:</dt>
    <dd>`"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"`</dd>
    <dt>Arguments:</dt>
    <dd>`--incognito`</dd>
    <dt>Friendly Name:</dt>
    <dd>`Google Chrome - Incognito`</dd>
</dl>### [Internet
Explorer](https://support.microsoft.com/en-us/windows/internet-explorer-downloads-d49e1f0d-571c-9a7b-d97e-be248806ca70)

<dl>
    <dt>Program:</dt>
    <dd>`"C:\Program Files (x86)\Internet Explorer\iexplore.exe"`</dd>
    <dt>Arguments:</dt>
    <dd>`-private`</dd>
    <dt>Friendly Name:</dt>
    <dd>`Internet Explorer - InPrivate`</dd>
</dl>### [Firefox](/tag/firefox/)

<dl>
    <dt>Program:</dt>
    <dd>`"C:\Program Files\Mozilla Firefox\firefox.exe"`</dd>
    <dt>Arguments:</dt>
    <dd>`-private`</dd>
    <dt>Friendly Name:</dt>
    <dd>`Firefox - Private`</dd>
</dl>### [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)

<dl>
    <dt>Program:</dt>
    <dd>`"C:\Program Files\Firefox Developer Edition\firefox.exe"`</dd>
    <dt>Arguments:</dt>
    <dd>`-private`</dd>
    <dt>Friendly Name:</dt>
    <dd>`FirefoxDeveloperEdition - Private`</dd>
</dl>### [Edge](https://www.microsoft.com/en-us/edge)

Not yet supported.

### [Opera](https://www.opera.com/)

<dl>
    <dt>Program:</dt>
    <dd>`"C:\Program Files\Opera\launcher.exe"`</dd>
    <dt>Arguments:</dt>
    <dd>`--private`</dd>
    <dt>Friendly Name:</dt>
    <dd>`Opera Internet Browser - Private`</dd>
</dl>
<figure aria-describedby="caption-attachment-5620" class="wp-caption aligncenter" id="attachment_5620"
    style="width: 300px">[![Visual Studio Private/Incognito Debug
    Targets](/assets/img/vsDebugTargets_collinmbarrett-300x293.jpg)](/assets/img/vsDebugTargets_collinmbarrett.jpg)
    <figcaption class="wp-caption-text" id="caption-attachment-5620">Visual Studio Private/Incognito Debug Targets
    </figcaption>
</figure>

*via [Scott
Hanselman](https://www.hanselman.com/blog/visual-studio-web-development-tip-add-chrome-incognito-mode-as-a-browser)*