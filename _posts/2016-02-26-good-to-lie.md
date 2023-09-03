---
title: '1 Time When It Is Good to Lie'
date: '2016-02-26T04:30:30-06:00'
author: 'Collin M. Barrett'
excerpt: 'I recommend lying properly on website security questions to block hacks by data miners and phishers.'
layout: post-wp-import
permalink: /good-to-lie
redirect_from: /good-to-lie/
image: /assets/img/goodToLie_collinmbarrett.jpg
categories:
- InfoSec
tags:
- Google
- Multi-Factor
- Password
- Phishing
---

I try not to lie. It is disrespectful to my neighbor and against the nature of God. There is one case in which I have no
problem being dishonest, however. When a service prompts me to provide the answers to a set of security questions, I
make up random, often unintelligible answers to their devious collection of information. Granted, the majority of
services have migrated away from this old security feature, but implementations are still commonplace. Why does eBay
need to know my mother’s maiden name or Progressive Insurance need to know my pet’s name? It is a sorry attempt at
security which is likely more harmful than good.

## Two Reasons for Lying

First, why would I want to give up that kind of PII (personally identifiable information) to XYZ service? Websites
should only collect information for an essential reason. These days, it seems like a week does not pass by in which some
large company or government database is hacked exposing the information of thousands of users to the public.

Also, providing answers that are discoverable or possibly even known by others is a horrible way to do security. While I
like to think that it would be somewhat difficult, it is likely far from impossible for any Internet citizen to
determine the make and model of my first car.

## The Research

Google performed a [fascinating study](https://research.google/pubs/pub43783/ "Secrets, Lies, and Account Recovery:
Lessons from the Use of Personal Knowledge Questions at Google") last year demonstrating the real weakness of personal
knowledge questions for security. Even amongst users who claim to be security conscious, their answers to such questions
tended to hurt their safety in the long run.

> A user survey we conducted revealed that a significant fraction of users (37%) who admitted to providing false answers
did so in an attempt to make them “harder to guess” although on aggregate this behavior had the opposite effect as
people “harden” their answers in a predictable way.
> — <cite>Research at Google</cite>

Google also found that ordinary users who answer security questions honestly still frequently have a difficult time
accessing their accounts. Users often cannot remember the name of their favorite book from two years ago; and, if they
do remember, maybe they forgot how to spell it. Websites need to strike a delicate balance between questions that are
easy for users to answer but hard for third parties to guess. Researchers at Google found that this balance is almost
never reached. Other two-factor security methods such as time-sensitive SMS codes are far more secure.

## My Recommendation for Lying

If you are going to lie, do it well. Make up a completely random answer for every question and every different instance
of the same issue across services. Furthermore, you could even use a secure random password generator (such as [Perfect
Password](https://www.grc.com/passwords.htm "Gibson Research Corporation")) for each answer, but I would avoid special
characters as they may cause breakage in some security systems. Do not think you are lying by appending a “1” or your
birth year to the end of honest answers. Hopefully, services have rate limiters in place to prevent brute force attacks,
but such simple permutations of the truth are easy for a dedicated hacker to acquire.

## Strong Warning for Converted Liars

If I have convinced you to start lying, I strongly urge you to record your answers in a safe place. I recommend a
well-secured password vault such as [LastPass](https://www.lastpass.com/) and a physical paper copy in a water-proof
container in a fire safe. The very precaution that you are taking to protect your privacy by lying will come back to
bite you if you misplace your fictitious responses when it comes time to log into XYZ service.