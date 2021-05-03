---
layout: post
title: Doorkeep's path to Google's first page of results
published: true
date:   2021-03-29 09:00:00 -0500
date_short: Mar 29
date_long: Mar 29, 2021
description: I stumbled a lot when getting Doorkeep listed in Google.  Here is how I fixed it.
excerpt: I stumbled a lot when getting Doorkeep listed in Google.  Here is how I fixed it.
---

**Originally posted on [HEY World](https://world.hey.com/andypeters/doorkeep-s-path-to-google-s-first-page-87658249)**

[Doorkeep](https://doorkeep.co) is finally on the first page of Google when you [search doorkeep](https://www.google.com/search?q=doorkeep&ie=UTF-8&oe=UTF-8&hl=en-us).  It was tougher than I thought to get to the first page for a fairly uncommon word. Here some of what I learned over the last 2 weeks:

![Search for Doorkeep](/images/doorkeep/doorkeep-search.gif)

After the site was published, I used [Google Search Console](https://search.google.com/search-console) to tell Google about it.  A day or two later, I was on the 7th page of Google.  I should have paid closer attention to the Google Search Console too because it was all over the place after that and it gave me the warning signs.

Google Search console continued to report on 3 specific mobile usability failures.  I learned this was more than a usability thing with Google.  Poor mobile sites optimization made Doorkeep rank lower for sure...

Search Console had 3 specific mobile usability failures.

![google search console](/images/doorkeep/google-search-console-2021-03-26-doorkeep.png)

Google Search Console allows you to render live URLs in the console.  I'd try to index my page but the site wasn't on the CDN because the index page was taking too long to respond.  This caused Google to render things weird and report the mobile errors.  This kept the site listed page 6 or 7 every day.

I switched hosts to see if it’d be able to render faster more reliable for Google.  It was.  Bonus the site is just faster.  Google Search Console has you “resubmit for verification”.  After that the mobile usability errors went away and site was on the first page a few hours later.

Hoping that [Doorkeep](https://search.google.com/search-console) is now setup better as I start creating content next month.