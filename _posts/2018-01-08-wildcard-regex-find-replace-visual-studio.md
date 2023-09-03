---
id: 5037
title: 'Wildcard Regex Find and Replace in Visual Studio'
date: '2018-01-08T04:00:07-06:00'
author: 'Collin M. Barrett'
excerpt: 'An example of how to use Visual Studio regex find and replace with a wildcard to quickly refactor many lines
of code.'
layout: post-wp-import
guid: '/?p=5037'
permalink: /wildcard-regex-find-replace-visual-studio
redirect_from: /wildcard-regex-find-replace-visual-studio/
image: /assets/img/regexFindReplaceGlassesComputer_collinmbarrett.jpg
categories:
- Code
tags:
- Dotnet
- Productivity
- 'Shelby Systems'
- 'Visual Studio'
---

## Wearing Out the Delete Key

When fixing a bug recently, there was a pattern in scope of where I was working that looked something like this:

```
<pre class="brush: csharp; title: ; notranslate" title="">
MyObjectCollection.Add(new MyObject(0, "0100", "Name"))
MyObjectCollection.Add(new MyObject(0, "0200", "Address"))
```

The method’s developer used this pattern about one hundred times in the same method! To iteratively refactor, I created an extension overload method for Add() which hid and de-duplicated the repetitive MyObject instantiations.

Refactoring the original method, however, then required much manual selecting and deleting. I would have to delete each instance of `new MyObject(` and `)`. I could use standard find and replace for all instances of `new MyObject(`, but how would I remove their corresponding closing parenthesis?

## Regex Find and Replace

Rather than spending the time to delete the refactored code manually, I decided to spend (albeit as much) time automating it with regex find and replace.

Regex is powerful, but I use it so rarely that it is not a skill in which I have developed any meaningful chops. When I finally determined a usable pattern for the issue described above, I had to jot it down to reference later.

## TL;DR Solution

In Visual Studio’s “Find” field:

```
(MyObjectCollection.Add&#40;new MyObject)(&#40;[^)]*&#41;)(&#41;)

```

In “Replace” field:

```
MyObjectCollection.Add$2

```

## Solution Explanation

I split the find pattern into three groupings by parentheses. Since parentheses carry regex significance, I encoded `&#40;` and `&#41;`, the existing parenthesis characters. Visual Studio replaces the `$2` variable in the Replace field with the contents of the second parenthetical grouping in the Find field. So, this solution replaces each entire line captured by the find regex pattern with the replace regex pattern (preserving the unique MyObject constructor parameters with the `$2` variable).

<figure aria-describedby="caption-attachment-5773" class="wp-caption aligncenter" id="attachment_5773" style="width: 630px">[![Diagram of Regex Find and Replace Solution](/assets/img/regexSolutionExplanation_collinmbarrett.jpg)](/assets/img/regexSolutionExplanation_collinmbarrett.jpg)<figcaption class="wp-caption-text" id="caption-attachment-5773">Diagram of Regex Find and Replace Solution</figcaption></figure>

Is there a more efficient way to do this? Most likely. But, this method worked.

*via [StackOverflow](https://stackoverflow.com/questions/24135006/regex-that-match-any-character-inside-a-parenthesis/24135281)*