---
title: 'GiveCamp Memphis 2020'
date: '2020-03-02T07:30:00-06:00'
author: 'Collin M. Barrett'
excerpt: 'I deeply enjoyed joining 100+ developers and designers to build websites for 20 Memphis nonprofits at GiveCamp
Memphis 2020.'
layout: post-wp-import
permalink: /givecamp-memphis-2020
image: /assets/img/giveCampMemphis_collinmbarrett.png
categories:
- Code
tags:
- Friends
- HTML
- Memphis
- MemTech
- VPN
- WordPress
---

## GiveCamp v2020

<div class="wp-block-image">
    <figure class="alignright size-medium">[![GiveCamp Memphis 2020
        Standup](/assets/img/giveCampMemphis2020_standup_collinmbarrett-300x258.jpg)](/assets/img/giveCampMemphis2020_standup_collinmbarrett.jpg)
        <figcaption>Standup Meeting</figcaption>
    </figure>
They did it again. The [GiveCamp Memphis](https://www.givecampmemphis.org) organizing team, in partnership with
the [Memphis Technology Foundation](https://memphistechnology.org/) and [AIGA Design for
Good](https://www.aiga.org/membership-community), really stepped it up for the 2020 event at the [FedEx Institute of
Technology](https://www.memphis.edu/fedex/). This year seems to have been the largest event yet with well over 100
technology and design professionals volunteering and representatives from 20 area nonprofits in attendance. These
numbers represent more than double the size of the other two years in which I was able to participate (2018 &amp; 2019).

While technologists constantly press the measurement of scale, this year's 48-hour hackathon for charity saw
improvements by all other metrics as well. Communication was succinct, yet exhaustive. Project managers (a new role this
year to help scale) kept their finger on the pulse of team needs and tore down barriers as soon as we hit them.
Logistics were executed transparently to the volunteers, facilitating distraction-free output from the teams. The DevOps
group delivered impressively rapid and stable infrastructure support to the requirements of 20 different projects. The
catering was, simply put, incredible (especially the Friday night dinner of chicken parmesan and mashed potatoes).

The recipient organizations walked away with an incredible amount of value to leverage into pursuing their mission in
Memphis, and the volunteers received a unique, time-boxed opportunity to sharpen cross-functional skills on diverse
teams that had, in most cases, just met each other. Memphis is better for it, and I am thankful to have had the
opportunity to learn and contribute. It is an exciting time to work in technology in this city.

> There is nothing about Memphis that says you can't be wildly successful here as an individual or as an entrepreneur.
>
> <cite>[Chad Fowler, TechCamp Memphis 2016](/chad-fowler-memphis-technology/)</cite>

## Team Craft

<div class="wp-block-image">
    <figure class="alignright size-medium">[![Craft Organization Team - GiveCamp Memphis
        2020](/assets/img/giveCampMemphis2020_craftOrganization_collinmbarrett-300x228.jpg)](/assets/img/giveCampMemphis2020_craftOrganization_collinmbarrett.jpg)
        <figcaption>Team Craft</figcaption>
    </figure>
I would have been very happy supporting most of the organizations that were participating, but I ended up choosing
Craft Organization because they did not seem to have as many volunteers near the end of the team formation hour (think
speed-“dating” while eating). I am quite glad to have met Milton Craft and this small band of volunteers.

Milton (a third-generation farmer) and Synolve Craft started this fairly new organization to train the urban poor on how
to subsistence garden. They are inviting community members to their 9-acre farm to learn, grow, and harvest crops that
can be sold or directly consumed to sustain their families. Aspiring gardeners can return to their backyards with the
skill to provide a low-cost source of nutrition.

### Pivoting from Wix to WordPress

The Crafts had a [Wix ](https://www.wix.com/)site that was already in good shape for a new organization. However, Milton
wanted to ensure that the site more clearly portrayed the message of their nonprofit and better-facilitated
communication and support streams with the community.

Our team of software developers and the GiveCamp Memphis community at large was more skilled with
[WordPress](/tag/wordpress/) than Wix, so we decided to rebuild the site from scratch. We were able to move quickly,
however, since we already had a lot of great copy we could migrate over from their existing site. This decision also
provided the long-term benefit of eliminating the ongoing service fees of Wix.

<div class="wp-block-image">
    <figure class="alignright size-medium">[![New Craft Organization
        Homepage](/assets/img/craftOrganizationHome_collinmbarrett-113x300.jpg)](/assets/img/craftOrganizationHome_collinmbarrett-scaled.jpg)
        <figcaption>New Craft Organization Homepage</figcaption>
    </figure>
### The Homepage

Our teammate Amy drove down from Chicago just to attend this event. She has a background in design and took charge of
capturing the vision of the Craft Organization in a compelling new homepage featuring a hero time-lapse of seedlings
growing and a photo collage tied together by a root system. This element, along with a magnificent new logo by design
volunteer Rod, proved to be the standout accomplishment of the project.

We initially wanted the photo collage to be a dynamic feed from the Crafts' Instagram feed. We were unable to
successfully filter out unrelated pictures reliably, however, and the static graphic allowed for a more cohesive
integration of the photos with the roots and program information text boxes.

### Challenge: Transactional Email from a University Campus

Our most significant technical hurdle was allowing the new WordPress site to send emails to the Crafts when people
filled out the contact form. GiveCamp Memphis' infrastructure does not support SMTP relays of any kind, so we had to use
a third-party mail service.

Normally, this is not too much of a challenge. The GiveCamp DevOps team recommended using
[MailJet](https://www.mailjet.com/). I have used [Mandrill](https://mailchimp.com/features/transactional-email/),
[SendGrid](https://sendgrid.com/), and [Mailgun ](https://www.mailgun.com/)in the past. The free tiers on many of these
platforms are more than adequate.

However, since we were working on a university campus, all of these services seemed to block us or require extensive
validation for us to use them. We tried MailJet, SendGrid, and [Sendinblue](https://www.sendinblue.com/) with no luck
getting our account validated on a Saturday evening. Signing up through my consumer [VPN](/tag/vpn/) provider did not
seem to help bypass these restrictions either.

We ended up using Mr. Craft's personal Google account to configure the new website to relay mail through Gmail. This
solution required configuring an app (secrets, callbacks, etc.) in his Google Developer Console, but we were able to get
it working without too much trouble, bypassing the extended validation requirements of the other providers.

### The Final Product

In addition to the new home page, we were able to configure a PayPal donation button on a more compelling support page
explaining how they use the funds, provide a method for collecting email addresses for a newsletter, scaffold out a blog
with some post templates, and generally provide a complete marketing tool for the Craft Organization. Milton was quite
pleased with the result.

<div class="wp-block-button aligncenter">[Preview of New Craft Organization
    Website](https://craft.wp.givecampmemphis.org/)## Value &gt; Perfection

I only got to participate for half of the weekend in 2018 due to illness. But, I left feeling a bit shocked and
discouraged at the nature of a weekend hackathon. For years I have built software for large organizations. Projects have
months or years to be completed, and customers expect an adequate balance of quality and delivery speed. GiveCamp,
however, by its nature, has to emphasize speed over quality. Not to say that quality is not important, as it certainly
is, but near-perfection is just impossible in a weekend.

This year, I finally began to feel more comfortable accepting good over perfection and limited the scope to what can be
reasonably accomplished in less than 48 hours. To ship something useful, we truly have to “move fast and (sometimes)
break things”. The final product might not be perfect, but it is great and will be very useful to its owner.

## Nonprofit Organizations

I was very encouraged by the breadth and depth of the nonprofit lineup this year. Consider connecting with and
supporting some of these groups doing great work in our city.

*Note: Some of these links are to pre-GiveCamp sites. I just wanted to provide the best way of finding these
organizations at the time this article was published.*

- [Arrows Nest](https://www.smore.com/gvdyt-arrows-nest-memphis)
- [Big Brothers Big Sisters of the Mid-South](https://msmentor.org/)
- [Blight Authority of Memphis](http://www.blightauthoritymemphis.com/)
- [Bravery Over Bullying](https://www.facebook.com/BraveryoverBullying/)
- [Clean Memphis](https://www.cleanmemphis.org/)
- [Collegiate Life Investment Foundation](https://donttxtndrv.org/)
- [Craft Organization](https://www.craftorganization.org/)
- [Fayette Cares](https://www.fayettecares.org/)
- [JUICE Orange Mound](https://www.juiceorangemound.org/)
- [Mid-South Culinary Alliance, Inc.](https://www.facebook.com/midsouthculinaryalliance/)
- [A New Day Studios](https://anewdaystudios.com/)
- [OUT Memphis](https://www.outmemphis.org/)
- People for the Enforcement of Rape Laws
- [Room in the Inn Memphis](https://ritimemphis.org/)
- [South Side Wildcats Organization](https://southsidewildcats.com/)
- [Thistle and Bee Enterprises, Inc.](https://thistleandbee.org/)
- [Total Arts Coalition](http://totalspirit.org/)
- [Unity for Animals](https://unityforanimals.com/)
- [The Women's Advocacy Center](https://www.womensadvocacycenter.org/)
- Women Rock 2

## Thanks,

- Nonprofits – For taking action to solve some tough problems in our city
- Organizers – For the incredible number of hours volunteered year-round to make this thing happen
- Volunteers – For spending a weekend enriching this city
- Sponsors – For funding tools, space, talent, food, and swag

I am sincerely looking forward to 2021.