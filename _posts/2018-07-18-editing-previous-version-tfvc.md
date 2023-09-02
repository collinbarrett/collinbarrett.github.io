---
id: 6812
title: 'Editing a File&#8217;s Previous Version in TFVC'
date: '2018-07-18T04:00:46-05:00'
author: 'Collin M. Barrett'
excerpt: 'How to edit a previous version of a file in a TFVC local workspace in Visual Studio rather than automatically
getting latest on check-out.'
layout: post
guid: '/?p=6812'
permalink: /editing-previous-version-tfvc/
image: /assets/img/visualStudio_collinmbarrett.jpg
categories:
- Code
tags:
- 'Source Control'
- 'Visual Studio'
---

## Pain Point

On occasion, I have wanted to grab a specific non-latest version of a file from TFVC and fiddle around in it in a *local
workspace* to test a theory. There are lots of ways to get a specific version of a file, folder, or entire branch in
Visual Studio. However, whenever you start to edit a previous version of a file, Visual Studio automatically checks out
the *latest* version of the file.

## Solution

The fix (read workaround) is to check out the file manually before getting a previous version of it rather than relying
on the `Check out automatically` options.

## Configurable Option

It seems like it would be simple to have an option to toggle whether `Check out automatically` gets the latest version
of the file or keeps the current version. Maybe that is an option, but I have not been able to find it.

There is the option below at `Tools` -&gt; `Options` -&gt; `Source Control` -&gt; `Visual Studio Team Foundation
Server`, but it does not seem to have any effect on a *local* workspace.

> Get latest version of item on check-out in a server workspace

## Environment

My team is using Visual Studio Enterprise 2017 and Team Foundation Server 2015.

*via [StackOverflow](https://stackoverflow.com/questions/35748680/tfs-check-out-specific-version-make-changes-check-in)*