---
id: 886
title: 'Nest Woes and the Importance of QA'
date: '2016-01-21T07:43:18-06:00'
author: 'Collin M. Barrett'
excerpt: 'An outline of our Nest Thermostat and Protect over the past six months explains the need for QA at IoT
companies to be top-notch.'
layout: post
guid: '/?p=886'
permalink: /nest-woes-importance-qa/
image: /assets/img/nestWoesImportanceQa_collinmbarrett.png
categories:
- Code
tags:
- Algorithms
- Church
- Family
- ISP
- Jenny
- Memphis
---

I rolled out of bed the other morning at a quarter to 6 am to find a chilling surprise. Thankfully, we live in the
South, but it is still a cold week here (20°s and 30°s Fahrenheit). Our [Nest
Thermostat](https://store.google.com/us/category/connected_home?hl=en-US&GoogleNest&utm_source=nest_redirect&utm_medium=google_oo&utm_campaign=homepage)
set its temperature to 59°F at a time when we had scheduled it for 69°F. When an IoT (first and last time I will use the
buzzword in this post) device fails, its effects are often immediately noticed. Quality assurance (QA) is imperative if
the “smart home” revolution is to come finally to fruition.

Let me start by acknowledging that we used the Nest Thermostat and Protect for over a year with no major issues. This
tenured success is both an affirmation of decent hardware and software, but also a confirmation the Nest team did not
perform proper testing before their major software release in December. I will also say that my personal evaluation of
Nest’s customer service is a 4 out of 5. That is a pretty solid rating, but they are not among the elite few companies
nailing it (i.e., Buffer). Find below a timeline of our Nest experience.

## The Bugs

- Mar 2, 2014 – Ordered Nest Thermostat v2 and Nest Protect
- Mar 5, 2014 – Replacement Nest Thermostat Ordered – I cannot remember the exact details, but the original unit shipped
to us was faulty in some way. I think it might have had something to do with the mounting apparatus. The issue is worth
noting here, but I recognize that some small amount of manufacturing error is possible for any product.
- Aug 14, 2015 – Protect False Positive – I was in Northern Michigan at a family reunion while my wife was home in
Memphis on this Friday night. She called me saying the Protect was alarming for smoke detection; however, she did not
notice any form of smoke or fire hazard in the house. I do not think she had even been baking that night (which is
[rather unusual](https://jennythebaker.com/ "Jenny the Baker")). After I waited on hold for well over an hour with Nest
support, and Jenny had the fire department come to check it out, Nest chalked it up to a hardware defect and sent a
replacement. False positives are better than the opposite; but, for the price of the unit, I would not expect to see a
failure after only a year of use on a device as important as smoke and carbon monoxide detector.
- Jan 3rd, 2016 – Thermostat Early On Bug – The schedule was set to 63°F overnight during sleeping hours, 69°F at 7 am
just before we would wake up, and then to drop back down to 63°F at 9:30 am during the church service. I woke up at 7:40
am to find the Nest set to 63°F, reading a current temp of 65°F, and indicating “Pre-Heating” as if it was preparing for
the 63°F at 9:30 am. However, it should remain set to 69°F until 9:30 am.
- Jan 5th, 2016 – Furnace and A/C Bug – This may or may not have been a bug with the Nest. However, we had noticed since
about the middle of December that the House took an unusual amount of time to heat up. Some days the furnace could not
even get the interior temperature up to the reasonable value of 69°F. We finally reported it to our property manager,
and the HVAC technician came out a few days later to discover that the A/C unit was running every time the furnace was
running (which, after looking at the Nest data, was nearly 24 hours a day). He verified that all of the wirings behind
the Nest Thermostat looked correct, but he suggested maybe a wire got crossed outdoors somehow during a previous service
call. We have lived in this house for three winters now, and this is the first time we had experienced this issue. We
will find out when we go to turn the A/C main back on in the Spring, but it seems like this could be a weird Nest flaw
as well.
- Jan 18th, 2016 – Thermostat Battery Drain Bug \[Report\] – [The New York Times
reported<sup>1</sup>](https://www.nytimes.com/2016/01/14/fashion/nest-thermostat-glitch-battery-dies-software-freeze.html
"Nest Thermostat Glitch Leaves Users in the Cold - The New York Times") that Nest users saw the battery on the devices
completely drained. The unit was powering off leaving their owners to freeze. All of the major media outlets were
reporting on the issue as it affected a rather large number of users. There should be failsafe after failsafe when it
comes to the power supply of a critical device like this. Consumers expect their thermostat always to be powered on and
functioning.
- Jan 19th, 2016 – Thermostat Early On Bug – This seems like it could be a recurrence of the Jan. 3rd bug, but we had
not seen the bug at all in between these dates. The schedule was set to 64°F overnight during sleeping hours, 69°F at 5
am just before we would wake up, and then to drop back down to 59°F at 6:30 am during the work day when we are away. On
this morning, when I rolled out of bed at 5:45 am, the thermostat was set to 59°F when it should have been 69°F.
Needless to say, my wife was not happy.

> If it ain’t broke, don’t fix it.
> — <cite>T. Bert Lance</cite>

## Wrapping Up

QA is something I have become quite familiar with over the past year. I have spent the majority of my tenure at
thyssenkrupp Elevator developing unit test scripts for our elevator dispatching algorithms. In systems with vast
quantities of variables and life or death repercussions, creating unit tests can be tough. If you are a manufacturer of
a device as important as even a thermostat, and the current version of the software works, you better be real sure the
hot new update you are about to push is rock solid.

I do not intend for this to be a Nest-bashing post. I love the products when they work, and Tony’s team has made huge
waves in the push towards a smarter home. I just think that it is important to note that as our light bulbs,
Crock-Pots®, and refrigerators all come online shortly, the software behind them better be rock solid. People can put up
with a traditional computer malfunctioning from time to time, but if they are locked out of their home because their
door lock is not connecting to Bluetooth correctly, the outrage will undoubtedly ensue.

<sup>1</sup>Comedic Note: To the graphic designer at The New York Times, could you not find an image of the Nest where
its current temperature was cold enough for ice? [Referenced Graphic](/assets/img/nestNYTGraphic_collinmbarrett.jpg
"Graphic - Nest Thermostat Glitch Leaves Users in the Cold - The New York Times").

*<small>I am a participant in the Amazon Services LLC Associates Program, an affiliate advertising program designed to
    provide a means for me to earn fees by linking to Amazon.com and affiliated sites.</small>*