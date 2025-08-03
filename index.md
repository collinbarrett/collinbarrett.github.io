---
layout: default
title: Home
permalink: /
---

👋🏻 Howdy! I'm a seasoned full-stack .NET software engineer based in Memphis, TN.

## 👨🏻‍💻 Software

With fifteen years of experience, I've wielded code to tackle challenges across various industries—banking, HR, manufacturing, accounting, retail, and defense. Currently, I'm on a product team at [Arcoro](https://arcoro.com/) that delivers HR software to thousands of construction companies. My tech stack includes, but is not limited to, C#, ASP.NET Core, Entity Framework Core, Azure, Angular, and React.

In 2015, I founded [FilterLists](https://filterlists.com/), born out of my passion for internet privacy and as a sandbox to learn, practice, and showcase new skills. I'm also honored to be on the Steering Committee for [GiveCamp Memphis](https://givecampmemphis.org/), helping to serve Memphis nonprofits and develop community in a unique annual hackathon.

I'm an avid learner, continuously sharpening my skills through a steady diet of [books](https://www.goodreads.com/collinbarrett), blogs, podcasts, and [meetups](https://www.meetup.com/members/186166841/). I hold a B.S. in Computer Engineering from [Cedarville University](https://www.cedarville.edu/academic-schools-and-departments/engineering-and-computer-science) and a [Microsoft Azure Developer Associate](https://learn.microsoft.com/en-us/users/collinbarrett/credentials/32c03a8f29583bce) certification.

## 🏡 Life

I was raised in Kalamazoo, MI, and met my wife during our undergraduate studies in OH. We moved to TN in 2012 so Jenny could complete her [Memphis Teacher Residency](https://memphistr.org/). She's now a middle school ELA teacher and girls' soccer coach at [The Collegiate School of Memphis](https://www.collegiatememphis.org/) and is also the mastermind behind [jennythebaker.com](https://jennythebaker.com/).

When we're not working or traveling, our time is spent enjoying our [Christ City Church](https://christcity.org/) community, walking our black lab Bailey around our Binghampton neighborhood, [exploring local eateries](https://www.google.com/maps/contrib/113780082327097075301/reviews), re-watching episodes of "The Office," and savoring quality coffee.

## 📝 Blog

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts limit:10 %}
* {{ post.date | date: "%m.%d.%y" }} &#124; [{{ post.title }}]({{ post.url }})
{% endfor %}
* [View All](/blog)
