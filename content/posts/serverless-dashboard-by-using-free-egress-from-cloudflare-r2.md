+++
title = "Plotting all of Sweden's real estate prices on a heatmap with deck.gl"
date = "2022-11-27T11:36:03+02:00"
author = "Alexander Dahl"
authorTwitter = "" #do not include @
cover = ""
tags = ["infrastructure"]
keywords = ["", ""]
description = ""
showFullContent = true
readingTime = false
hideComments = false
+++

{{< image src="/img/stockholm.jpg" alt="Stockholm" position="center" style="border-radius: 8px; max-width: 50%" >}}

_A [heat map](https://bostadsbussen.se/sold/map) of apartment prices in central Stockholm_

Disclaimer: I will be linking to sites in Swedish. A translation extension might be handy.

I, like a lot of people in Stockholm, need to buy an apartment. The rental situation
is bad. Getting a "first-hand" contract is hard. I have friends who have even had to
settle for a temporary "third-hand" contract, i.e. sublet not once but two times!

So what should a data person such as myself do to identify which areas in Stockholm
are reasonably priced? Plot all the data points on a heatmap! Which is what I set out to do.

My side project [bostadsbussen](https://bostadsbussen.se) scrapes user entered
real estate listings from hemnet and archives them. You can read about the tech behind
that in my previous blogpost [LINKNAKNSKN].

All right, we have a place to host a heatmap. First we need to get the data!

Luckily the data is out there on the internet! [hemnet.se](https://hemnet.se) provides
the sold prices for most of their listings
[HERE](here). The problem is that they only return max 2500 results per search query.
So we need to craft some queries to extract all of the 1 million+ results on their site.
It was as simple as limiting the search queries by different parameters until the result
was lower than 2500. Then extracting the data from each listing was easy.

_I was also very mindful of not putting unncessary load on their servers, and deliberately
chose to not parellelize this. Getting all the listings took a week in real time._

Cool! Now we have a big JSON array with a million data points.
After some quick processing in python I had each property's coordinates, square meter price and
the date it was sold.

Now I want to vizualise this on an interactive map! And share it with the internet!

My first thought was to spin up a dashboarding solution like metabase or superset
on a rented VM. They are both great tools and it would have been a great option.
But I don't want to deal with issues if it would receive a lot of bursty traffic.
I also don't really want to deal with autoscaling stuff like kubernetes without getting paid NERD EMOJI.

With a readily built dashboarding tool out of the question, meant that
I would need to build the vizualisation myself. I found [deck.gl](https://deck.gl) which
is great for displaying large amounts of data on map. Perfect! It had support
for React as well, which is what the rest of the site is built on (and hosted
on cloudflare pages.)

(bild på en bra exempel från deck.gl)

We also need some map tiles that we can overlay the vizualisation from deck.gl on.
Mapbox has an excellentfree tier where the first 50000 views per month doesn't
cost anything. I doubt I will ever get more traffic than that.

OK, with this built locally on my machine I had a pretty cool visualization.
I think I spent an hour dragging the map around Sweden to see if my
pre-conceived notions about expensive areas was true. It was!
(The Östermalm area in Stockholm is really expensive)

Since the application is hosted on Cloudflare Pages, and everything was built
with React, hosting the visualization there was a no brainer.

But it's not really a visualization if there is no **data** to vizualise.

This leads us to the big problem of getting the data to the user.

My JSON array was 25MB compressed with gzip (125MB uncompressed). Hosting it on
an object storage like GCS would be cost nothing storage wise. The big problem
would be egress fees. If I used GCS and 1000 people visited my site, 2.5GB of egress fees
would. It would be how much? Thats more than zero! And even more if it got
super popular, which would not be a fun thing to wake up to.

Luckily Cloudfare has an object storage competitor called R2, which has 0 egress
fees. Zero! Now I could use that share the data to the user with a simple GET request.

I ran into some CORS problems but that was easily solved with some CORS guide from waslhy <link guide>

```
PUT CORS
```

Now I could share this with everyone on internet, without being worried about waking up to
a huge cloud bill! An additional benefit was that I could link the full json blob scraped.
So other interested parties don't need to hit hemnet.se servers and instead just download that file.

I shared the website on my LinkedIn and got around 10k visitors. I have an analytics dashboard in
umami cloud where you can check out the metrics. Cloudflare reported I had consumed around 30GB
of egress for the site. Nice!

My next steps is to include some line charts for analysis and also make sure the json blob
is updated with new data everyday. Kind of like a serverless dashboard!

I'm also reaching the end of my travel sabbatical (trekking in Nepal was a highlight!).
So I'm looking for a Data Engineering or Data Infrastructure job. Based in Stockholm or EU remote.
Link to [resume](https://dahl.dev/assets/Alexander_Dahl.pdf)

Shoot me an email at work@dahl.dev if you want to talk :)
