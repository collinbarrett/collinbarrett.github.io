---
id: 6707
title: 'Applying StyleCop Ordering with ReSharper File Layout'
date: '2018-07-03T13:39:15-05:00'
author: 'Collin M. Barrett'
excerpt: 'A guide and configuration for using ReSharper''s File Layout to auto-apply StyleCop ordering rules to elements
in classes, structs, and interfaces.'
layout: post-wp-import
guid: '/?p=6707'
permalink: /stylecop-ordering-resharper
redirect-from: /stylecop-ordering-resharper/
image: /assets/img/styleCopResharper_collinmbarrett.png
categories:
- Code
tags:
- Dotnet
- Productivity
- Refactoring
- 'Static Analysis'
- 'Visual Studio'
---

I have been wrestling a bit with the ideal ordering of members in my C# classes. Ordering members is obviously a
cleanliness/maintainability factor and not a functional one, but I do want to create consistency in my classes.

As a heavy user of ReSharper (R#), I often apply their default C# [File
Layout](https://www.jetbrains.com/help/resharper/File_and_Type_Layout.html) via [Code
Cleanup](https://www.jetbrains.com/help/resharper/Code_Cleanup__Index.html) to automatically re-arrange members. R#’s
default File Layout is fairly good, but I would prefer to use Microsoft’s
[StyleCop](https://en.wikipedia.org/wiki/StyleCop) ordering guidelines instead. R#, for example, does not default to
ordering properties by access level, which I find valuable for consistency.

I found a handful of blog posts and resources around the web doing something similar, but none seemed to be recent and
exactly compliant. Additionally, JetBrains does publish a [StyleCop
extension](https://plugins.jetbrains.com/plugin/11619-stylecop-by-jetbrains) for R#, but its scope does not seem to be
directly applicable to applying a File Layout alone.

## StyleCop’s Ordering Rules

The [StyleCopAnalyzer project](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) has a subset of rules that I have
converted for use in R# File Layout. This handful of rules were cherry-picked only in the sense that they meet the
following requirements:

- are applicable inside of a class, struct, or interface
- are supported by R#’s File Layout feature
- are not [deprecated by
StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/KnownChanges.md)

### [SA1124](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1124.md): Do Not Use
Regions

In R# XAML, this is implemented by adding the `RemoveRegions` flag in the `TypePattern` node.

```
<pre class="brush: xml; light: true; title: ; notranslate" title="">
&amp;lt;TypePattern DisplayName="StyleCop Classes, Interfaces, &amp;amp;amp; Structs" RemoveRegions="All"&amp;gt;
```

### [SA1201](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1201.md): Elements Must Appear In The Correct Order

Sort elements by type in the following order:

- Fields
- Constructors
- Finalizers (Destructors)
- Delegates
- Events
- Enums
- Interfaces
- Properties
- Indexers
- Methods
- Structs
- Classes

In R# XAML, this is implemented by the order of `Entry` nodes. For example, to put fields before constructors:

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Constructors"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Constructor" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;/Entry&amp;gt;
```

### [SA1202](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1202.md): Elements Must Be Ordered By Access

Sort adjacent elements of the same type in the following order of access level:

- public
- internal
- protected internal
- protected
- private

In R# XAML, this is implemented via the `Access` node under `Entry.SortBy` like below for fields:

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
```

### [SA1203](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1203.md): Constants Must Appear Before Fields

In R# XAML, this is implemented by the order of the constant and field `Entry` nodes.

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;Entry DisplayName="Constants"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Constant" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;/Entry&amp;gt;
```

### [SA1204](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1204.md): Static Elements Must Appear Before Instance Elements

In R# XAML, this is implemented via the `Static` node under `Entry.SortBy` like below for fields:

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
```

### [SA1214](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1214.md): Readonly Elements Must Appear Before Non-Readonly Elements

In R# XAML, this is implemented via the `Readonly` node under `Entry.SortBy` like below for fields:

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
```

## Final ReSharper File Layout XAML

Converting the above rules directly into ReSharper C# File Layout XAML results in the following TypePattern (also available as a [Gist](https://gist.github.com/collinbarrett/fcc004c386867135c06e26e449f73ba2)):

```
<pre class="brush: xml; title: ; notranslate" title="">
&amp;lt;TypePattern DisplayName="StyleCop Classes, Interfaces, &amp;amp;amp; Structs" RemoveRegions="All"&amp;gt;
&amp;lt;TypePattern.Match&amp;gt;
&amp;lt;Or&amp;gt;
&amp;lt;Kind Is="Class" /&amp;gt;
&amp;lt;Kind Is="Struct" /&amp;gt;
&amp;lt;Kind Is="Interface" /&amp;gt;
&amp;lt;/Or&amp;gt;
&amp;lt;/TypePattern.Match&amp;gt;
&amp;lt;Entry DisplayName="Constants"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Constant" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Fields"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Field" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Constructors"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Constructor" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Destructors"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Destructor" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Delegates"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Delegate" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Events"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Event" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Enums"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Enum" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Interfaces"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Interface" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Properties"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Property" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Indexers"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Indexer" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Methods"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Method" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Structs"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Struct" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;Entry DisplayName="Classes"&amp;gt;
&amp;lt;Entry.Match&amp;gt;
&amp;lt;Kind Is="Class" /&amp;gt;
&amp;lt;/Entry.Match&amp;gt;
&amp;lt;Entry.SortBy&amp;gt;
&amp;lt;Access Order="Public Internal ProtectedInternal Protected Private" /&amp;gt;
&amp;lt;Static /&amp;gt;
&amp;lt;Readonly /&amp;gt;
&amp;lt;/Entry.SortBy&amp;gt;
&amp;lt;/Entry&amp;gt;
&amp;lt;/TypePattern&amp;gt;
```

## Saving File Layout in ReSharper

1. In Visual Studio, navigate to `ReSharper` -&gt; `Options`
2. Navigate to `Code Editing` -&gt; `C#` -&gt; `File Layout`
3. Click `XAML` in the top right corner
4. Copy and paste the above TypePattern into the XAML in the order you want it to fall (such as right before “Default Pattern”)
5. Click `Designer` in the top right corner to verify the new TypePattern displays correctly
6. Save your changes

<figure aria-describedby="caption-attachment-6763" class="wp-caption aligncenter" id="attachment_6763" style="width: 300px">[![StyleCop ReSharper File Layout TypePattern](/assets/img/styleCopResharperFileLayout_collinmbarrett-300x226.jpg)](/assets/img/styleCopResharperFileLayout_collinmbarrett.jpg)<figcaption class="wp-caption-text" id="caption-attachment-6763">StyleCop ReSharper File Layout TypePattern</figcaption></figure>