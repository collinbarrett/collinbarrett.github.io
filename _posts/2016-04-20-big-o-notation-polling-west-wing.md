---
title: 'Example of Big-O Notation Inspired by <em>The West Wing</em>'
date: '2016-04-20T22:27:04-05:00'
author: 'Collin M. Barrett'
excerpt: 'A practical, real-world example of Big-O Notation inspired by Joey Lucas and Josh Lyman''s polling in The West
Wing.'
layout: post-wp-import
permalink: /big-o-notation-polling-west-wing
image: /assets/img/bigONotationPollingWestWing_collinmbarrett.jpg
categories:
- Code
tags:
- Algorithms
- Career
- 'fred''s'
- Friends
- Math
- Politics
- Travel
---

My wife and I are making our way through *The West Wing*. I have seen it before (…twice), but it is a classic and
definitely worth watching. One of my favorite [scenes from Season 1](https://youtu.be/pj4PwyfDNuI) predicted some of the
most controversial issues surrounding privacy that we are discussing now, 15 years later. Anyway, last night we watched
Season 2, Episode 13: “Bartlet's Third State of the Union.”

I have been refreshing my memory on Big-O Notation, so I wanted to attempt a quick mashup with a practical example from
the show. Here is a loose application of Big-O complexity analysis to the scale of sample results from Josh and Joey's
actual and hypothetical political polling.

## O(n)

Joey's polling team, watched annoyingly close by Josh, was tasked with collecting survey data immediately following
President Bartlet's address. To do so, they called a sampling of homes in shifting time zones as the evening progressed.
In this scenario, to achieve a result size of `x`, they had to call `n` homes and receive a response rate of `r`.

```
x = n * r

```

We can assume a constant `r` (17% in the show) so that `x` is directly related to `n`. A differing response rate would
adjust the slope of the line, but `x` is at most linearly proportionate to the number of calls made. Therefore, we could
say that the algorithm generating the result size is O(n) in complexity.

## O(1)

Before the initial results started to roll in, however, Josh's growing impatience was pushed over the edge when the call
center was struck with a power outage. Now, no matter how many numbers they might attempt to dial, the new response rate
`r'` would remain a constant 0.

```
r' = 0
x = n * r'

```

We could say that the function producing the result size is now O(1) in complexity since the maximum number of results
is fixed at zero despite the number of attempted calls.

## O(n<sup>2</sup>)

This show was aired a little before Web 2.0 took off, but what if each polled participant was allowed to share questions
asked of them through social media and results were collected from up to an additional `n` “friends” and “followers” per
called home? Setting aside the fact that this would be poor polling practice as it would no longer be a random sample,
the maximum size of the poll result set could now be calculated as follows.

```
x = n² * r

```

In this scenario, the largest possible number of responses that could be collected to analyze is O(n<sup>2</sup>) in
complexity.

## O(n!)

What if the polling was instead set up as a sort of phone tree (again, unrealistic, but bear with me). Joey's primary
team would be responsible for calling `n` homes to collect sample data. Each of those participants would then be
required to call `n-1` new participants. This chain would continue until a participant in the tree was only required to
call one final participant in their branch. In this example, the maximum number of polling results that would need to be
stored could be calculated as follows:

```
x = (rn) * [r*(n-1)] * [r*(n-2)] * ... = (rn)!

```

Assuming a constant `r` at all levels of the tree, we could say that the maximum size of the database would be O(n!) in
complexity.

## The Point?

Big-O provides a loose way to determine the upper bound of complexity as a data set or process scales. The database
administrators working for Joey could theoretically determine the Big-O for a given poll scenario such as those
described above to determine the infrastructure needed to house the results. The O(n) poll would require significantly
less storage than the O(n<sup>2</sup>) poll. If the power was out such as the O(1) scenario, he or she could take a
vacation.

This is a silly example to help solidify the concept of Big-O. When designing new projects or subroutines, I can take a
few minutes before banging out the code to analyze the complexity of the overarching solution to ensure that it is not
more than required.