---
title: 'Code Reviews'
date: '2017-09-13T07:30:26-05:00'
author: 'Collin M. Barrett'
excerpt: 'Reflections on the first few weeks of implementing regular code reviews in our team''s development
methodology.'
layout: post-wp-import
permalink: /code-reviews
image: /assets/img/legoCoder_collinmbarrett.jpg
categories:
- Code
tags:
- Career
- Dotnet
- Productivity
- 'Shelby Systems'
- 'Source Control'
- 'Visual Studio'
---

I am nearly a month into my fourth professional software development role. It has been an excellent experience so far;
but, like any transition, there are always adaptations that I need to make when merging into a new team. In my last gig,
we had started trying to implement the practice of peer code reviews, but there was too much going on at a fast pace to
make them habitual and useful. The practice was not at all a part of the development methodology in either of my first
two positions.

Now at [Shelby Systems](https://www.shelbysystems.com/), I was brought on to fill a new seat as a third developer on a
product development team. As such, there has been a bit of a renewed effort to implement some standardized practices and
coding styles amongst the group. Management is supporting the integration of peer code reviews, so we are giving them a
solid go. Here are a few reflections from the first few weeks of implementation.

## Logistics

Our team is using [TFS](https://azure.microsoft.com/en-us/services/devops/server/) for source control with its
integrated issue tracking and code review functionality built into [Visual Studio](https://visualstudio.microsoft.com/).
So far, we have agreed to submit code review requests to both of the other team members before nearly every check-in to
the main development branch.

The TFS interface for code reviews is fairly good. We can auto-generate a shelveset from the pending changes and include
any related work items or previous code reviews. When sending the request, we can customize the recipient(s) and provide
optional notes. For the reviewer, the tool provides a clean side-by-side code comparison of the current vs. the proposed
change and the ability to comment on specific lines.

Tangentially, because we are still on TFS (vs. git), it becomes unwieldy to have personal development branches (since
TFS does not de-duplicate when branching). So far, shelvesets have worked OK as a temporary holding place for local
iterative changesets; however, for larger tasks it would be nice to have a personal branch to work against before I am
ready to submit the code review request and merge to main. Our current strategy is to incrementally request reviews and
check-in for smaller sub-tasks of a larger task rather than performing major development items in a branch and then
merging all at once. I am a subscriber to the practice of checking in as frequently as it makes sense (for a more robust
undo), so we will see how this goes long-term. Requesting iterative reviews on sub-tasks could lead to more frequent and
verbose code reviews than needed.

## Timing

Undoubtedly, one of the greatest concerns with frequent code reviews is the amount of time it takes. In the first couple
weeks of this practice, each developer has been receiving an average of one to two code review requests per day. The
development life cycle is undoubtedly slowed somewhat, at least in the beginning, as we spend a portion of our time
reviewing peer code.

In addition to the amount of time, we have to balance the tradeoff between context switching and timely completion. When
I submit a code review request, I desire a somewhat tight feedback loop from my colleagues (preferably within 4-6
hours). However, on the receiving end, if I am neck-deep in a complicated bug or feature of my own, I hate to switch out
of my mental space to provide timely comments on my peer's issue.

We have not discussed a target turnaround time as a team yet. We have not needed to, as everyone has been providing
feedback in half a day, a full day at the most. The time could come, though, when the process is no longer new and at
the forefront of our minds, that this timeline could begin slipping. It is certainly something we will need to monitor.

## Feedback Types

Though never explicitly discussed, the comments we have been making on reviews have been around code style and
architecture. Since we have a QA team (an amazing new experience that I have not had previously), rigorously testing the
functionality is not something we necessarily need to do as a part of the code review. Our process expects that both the
original developer and the QA team will test changes before release.

Examples of feedback intentions have been to suggest simpler syntax for equivalent logic, recommend the application of
best practices, or question whether a change is needed or is directly related to the issue at hand. More importantly,
though, I had made a critical mistake by modifying the signature of an already published interface method a couple of
weeks ago, and the code review helped catch a potential breaking change. (In my defense, the interface was in the same
file as several other classes, so it was not obvious when performing the refactoring. However, it was certainly a
mistake I should have still caught.)

Comments have almost exclusively been suggestions for changes. There has been very little positive feedback given. The
assumption is that where there is no comment, we did a decent/good job. However, I am going to try to throw in a
positive comment now and then when a solution seems remarkably elegant or ingenious.

## Value

Code reviews performed consistently will increase the quality and uniformity of our code base. Having the other members
of the team so much as glance at my code before checking it in will make sure I am not re-inventing the functionality of
an existing extension method, deviating from a pattern used elsewhere in the application, or choosing a design with
adverse security or performance impact.

Secondly, reviews will level up my personal development skills much faster. Code during my undergraduate program was
examined for a grade, but I have used very little of the languages/platforms I learned in school since joining the
workforce. By having colleagues monitor and comment on my changesets, I will learn better ways of doing things and
hidden gotchas that I may have not experienced yet. Similarly, seeing the code my peers write will produce the same
refinement of my skills.

## Your Experience

What has been your experience with code reviews? What tips do you have that could make them even more valuable to our
team?

*<small>Disclaimer: Opinions expressed are solely my own and do not express the views or opinions of my
    employer.</small>*