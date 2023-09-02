---
id: 6305
title: 'ReSharper vs. CodeRush: A Quick &#8216;n Dirty, Unfair Comparison'
date: '2018-03-09T03:00:01-06:00'
author: 'Collin M. Barrett'
excerpt: 'My product team is evaluating purchasing JetBrains ReSharper or DevExpress CodeRush to assist with improving
code quality in our .NET application portfolio. I jotted down some unfair observations of the two products after using
ReSharper for a couple of years and CodeRush for a couple of days.'
layout: post-wp-import
guid: '/?p=6305'
permalink: /resharper-vs-coderush
redirect-from: /resharper-vs-coderush/
image: /assets/img/reSharperCodeRush_collinmbarrett.jpg
categories:
- Code
tags:
- Career
- Dotnet
- Productivity
- Refactoring
- 'Shelby Systems'
- 'Source Control'
- 'Static Analysis'
- 'Visual Studio'
---

My product team is evaluating purchasing [JetBrains ReSharper](https://www.jetbrains.com/resharper/) or [DevExpress
CodeRush](https://www.devexpress.com/products/coderush/) to assist with improving code quality in our .NET application
portfolio. I have been a ReSharper user for several years, and have only just heard about and trialed CodeRush over the
last few days. So, what follows is indeed an unfair comparison just due to the amount of experience I have with each.

## TL;DR

I vote for ReSharper.

## Observations

I have grouped the observations below by feature in no particular order. This is certainly not a comprehensive list, and
they do not account for deviating from the default configuration settings in either product.

### CodeRush IntelliRush

Seems irrelevant since Visual Studio 2017 includes essentially the same feature.

### CodeRush Structural Highlighting

Seems irrelevant since Visual Studio 2017 includes essentially the same feature.

### Keyboard Shortcuts, Templates, Code Expansion

Very similar in concept between the two products with differing implementations. I have not leveled up to power user in
shortcuts, though, and therefore did not perform a super thorough comparison.

### Code Cleanup \[`ReSharper++`\]

Code Cleanup is my favorite feature of ReSharper, and I use it daily. CodeRush has it as well, but, in my short trial,
it seems lacking.

- ReSharper puts color-leveled squiggles under suggested refactors by default. CodeRush requires the cursor to be
directly on the code and then clicking the lightbulb to see similar suggestions. Maybe there is an option to enable
highlighting in CodeRush, but I could not find it.
- ReSharper seems to suggest a much higher number of refactors than CodeRush in the same files. Again, I have no
empirical data to back this up; I just performed a quick inspection.
- ReSharper makes things like potentially unhandled exceptions much more evident than I can get CodeRush to flag.
- CodeRush does have the ability to flag spelling errors in member names, comments, etc., which is something I have not
seen ReSharper able to do.
- CodeRush does not support running Code Cleanup just against a selection of text. This is key since I practice
refactoring only methods I am touching already (and therefore taking development “ownership” of the method). ReSharper
allows me to select a method that I am already changing (I know, O/C principle, but we are not there yet) and run Code
Cleanup just against that one method. CodeRush seems to cleanup suggestion-by-suggestion or the entire file.

### Extraction \[`ReSharper++`\]

Both provide the ability to quickly extract methods from other methods and classes from other classes. ReSharper
provides a much more thorough wizard experience when doing so, however, such as customizing return type, input/output
params, access level, etc. CodeRush seems to assume you always want to follow their opinionated pattern.

### Debug Visualizer \[`ReSharper++`\]

This feature is fairly new to ReSharper but seems pretty similar in both products. I prefer the interface of ReSharper’s
implementation a bit more, but that may very well be personal preference.

### Team Sharing \[`ReSharper++`\]

Anytime you change an option in ReSharper, you can save the setting to your local computer, to a personal
project-specific config file, or to a team-shared config file (to check into source control). This is quite important
for enforcing team style guide policies and would allow changes to team settings to be code reviewed just like any other
code change. CodeRush’s support for this kind of thing seems much less fleshed out. ([see
here](https://supportcenter.devexpress.com/Ticket/Details/T368775/coderush-enforcing-same-code-analysis-rule-set-across-team-and-integrating-with-tfs-ci))

### Test Runner

Both products have a unit test runner, but I did not compare them since implementing TDD is sadly not in our near-term
backlog.

### CodeRush Toolbar Toggles \[`CodeRush++`\]

I do like the fact that CodeRush gives quick toggles in the toolbar for enabling/disabling some common and unique
options such as:

- CodeMetrics: Sugar that I have not seen in ReSharper, not sure they provide a ton of real value, though
- Member Icons: Sugar for spotting member types, particularly handy when the code is completely folded

### Performance (&amp; Roslyn) \[`CodeRush++`\]

CodeRush has migrated to hook into Roslyn for its static analysis. ReSharper, on the other hand, continues to use its
proprietary modeling to do so. Therefore, ReSharper tends to feel a bit heavier and perform a bit slower.

### Disabling \[`ReSharper++`\]

ReSharper adds a pane in the Visual Studio options that allows you to suspend/disable it on the fly. CodeRush can be
disabled similarly via the Visual Studio Extensions and Updates modal but requires a restart of Visual Studio which is
inconvenient.

### Community \[`ReSharper++`\]

- Stack Overflow Questions: [ReSharper (~4,200)](https://stackoverflow.com/tags/resharper/info) vs. [CodeRush
(~100)](https://stackoverflow.com/tags/coderush/info)
- Google Trends: [ReSharper &gt; CodeRush](https://trends.google.com/trends/explore?date=all&q=ReSharper,CodeRush)
- In my professional circle, I have only ever met developers that use ReSharper.
- JetBrains sponsored [GiveCamp Memphis](https://www.givecampmemphis.org/) a couple of weeks ago and gave out some free
licenses to volunteer participants (including yours truly).

### Core Business Alignment \[`ReSharper++`\]

JetBrains has a proven track record in the IDE tooling space. Their core business is built on a popular portfolio of
IDEs and plugins for IDEs. DevExpress, on the other hand, is known for its UI controls.

### Pricing &amp; Licensing (Business)

The base product rates at the time of writing are as follows.

| | Year 1 | Year 2 | Year 3+ |
|---|---|---|---|
| CodeRush Ultimate | $249.99 | $249.99 | $249.99 |
| CodeRush Roslyn | $49.99 | $49.99 | $49.99 |
| ReSharper Ultimate | $399.00 | $319.00 | $239.00 |
| ReSharper | $299.00 | $239.00 | $179.00 |

CodeRush Ultimate is CodeRush Roslyn with technical support. Though my CodeRush trial was unfairly short, having access
to technical support for at least a subset of licenses seems like it would be worth having. CodeRush supports volume
discounts for 2-5 licenses: 10%, 6-10 licenses: 15%, 10+ licenses: call for custom quote.

ReSharper Ultimate is ReSharper with [C++](https://www.jetbrains.com/resharper-cpp/),
[dotTrace](https://www.jetbrains.com/profiler/) (performance profiler),
[dotMemory](https://www.jetbrains.com/dotmemory/) (memory profiler), and [dotCover](https://www.jetbrains.com/dotcover/)
(unit test runner and code coverage tool). ReSharper supports a licensing server model providing flexible, floating
licenses for teams that change frequently or where a developer only needs access to a shared license intermittently.

## Conclusion

I vote for ReSharper.

*<small>Disclaimer: Opinions expressed are solely my own and do not express the views or opinions of my
    employer.</small>*