+++
title = "Stringing together several free tiers to host an application with zero cost using fly.io, Litestream and Cloudflare"
date = "2022-08-19T11:36:03+02:00"
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

{{< image src="/img/infrastructure.jpg" alt="Infrastructure" position="center" style="border-radius: 8px; max-width: 50%" >}}

_The image was generated by putting the blog post title into DALL-E 2. Quite fitting!_

I have a side project called [bostadsbussen](https://bostadsbussen.se). It
scrapes property listings for the Swedish real estate market. The site needs to
persist data in the form of user accounts, property data and images.

At the time of writing this post I am currently on a sabbatical. With my income
at 0, I want to keep the cost of hosting my side project as low as possible. We
will be traveling a few months in Asia, so hosting the site on the home servers
is also out of the question.

That leaves us with **the cloud ☁️**

There are cloud offerings like Firebase which are a great place to host a side
project. But I want to avoid the vendor lock-in and have the option to move the
entire application to my own server in the future. So this post will skip
examining Firebase et al.

Renting a VPS (Virtual Private Server), is a good and cheap option with no
lock-in. They usually cost around $5/month for a 1GB RAM and a shared CPU. But
what if we want to do it even cheaper?

**What if we could do it for free?**

Enter [fly.io](https://fly.io). They provide a free 256MB instance that you can
spin up with a valid Dockerfile and `fly deploy`. Great developer experience!

```bash
❯ fly deploy
==> Verifying app config
--> Verified app config
==> Building image
Remote builder fly-builder-spring-snow-7814 ready
==> Creating build context
--> Creating build context done
==> Building image with Docker
...
--> Building image done
==> Pushing image to fly
...
==> Creating release
--> release v8 created
```

And we have released our application on fly.io!

All right, we got our free server, what should we do about persisting data? If
we store data on the fly.io instance and if it crashes we lose everything! The
common choice would be to spin up a separate database server and use that for
storing our data.

But with the introduction of [Litestream](https://litestream.io), we don't need
to! Litestream will replicate the changes to an SQLite database to an object
storage. Litestream will also restore the database when the server restarts. No
dedicated database service needed! [Michael Lynch has written a great blog post
on this](https://mtlynch.io/litestream/).

When it comes to cloud storage all providers are very cheap for running
Litestream. So it comes down to developer preference. I chose [Cloudflare
R2](https://www.cloudflare.com/products/r2/) because of their free tier.

{{< image src="/img/r2.png" alt="r2" position="center" style="border-radius: 8px;" >}}

Getting Litestream to communicate with R2 is quite simple:

```yaml
# The litestream config

dbs:
  - path: /pb_data/data.db
    replicas:
      - type: s3
        endpoint: ${R2_URL}
        path: ${R2_DATA_PATH}
        bucket: ${R2_BUCKET}
        access-key-id: ${R2_ACCESS_KEY}
        secret-access-key: ${R2_SECRET_KEY}
```

```bash
# The script that restores and then continously replicates the data

echo "Restore db if exists"
litestream restore -if-replica-exists /pb_data/data.db
echo "Restored successfully"

echo "replicate!"
exec litestream replicate -exec "/pocketbase serve --http 0.0.0.0:8090"
```

Now we need a backend to host on the server. I have been very productive with
[PocketBase](https://pocketbase.io/). It is a go framework with several great
features. Like user authentication, an admin panel, an extendable API and a JS
SDK for connecting it to the frontend. Best part it uses SQLite as the database,
so we can use Litestream for our replication 🎉!

We also need a frontend. I'll admit I'm not very good at the frontend stuff, I
built one with React! It was quite enjoyable. For hosting a React app there are
several free options. Like [Vercel](https://vercel.com/),
[Netlify](https://www.netlify.com/) and [Render](https://render.com/). But I
chose [Cloudflare Pages](https://pages.cloudflare.com/). I don't see much
difference between the mentioned alternatives. Since I'm already using
Cloudflare's other services (DNS, R2) the choice was easy. (And I'm lazy).

The last thing I have in my application is the scraping part. Loading 100s of images
concurrently and moving them to an object storage is quite memory intensive. At
least a 256MB instance can't handle it! I offloaded the scraping part to [Google
Cloud Run](https://cloud.google.com/run). It scales to zero, and will only run
when it gets a scraping request. It stores images in a bucket and returns the
scraped data to the PocketBase backend. It of course also has a free tier that
I use! 🤓

And here is a diagram of the architecture. Generated with [Diagrams as Code](https://diagrams.mingrammer.com/).

{{< image src="/img/architecture.png" alt="Architecture" position="center" style="border-radius: 8px;" >}}

That's it! Hope you enjoyed the post.

Check out [github.com/aleda145/pocketbase-lab](https://github.com/aleda145/pocketbase-lab) for a
lab for setting up this architecture

Disclaimer: I paid $10/year for the [bostadsbussen.se](https://bostadsbussen.se)
domain.
