+++
title = "Publish your calendar and let recruiters schedule interviews themselves"
date = "2022-12-18T11:36:03+02:00"
author = "Alexander Dahl"
authorTwitter = "" #do not include @
cover = ""
tags = ["calendar", "web"]
keywords = ["", ""]
description = ""
showFullContent = true
readingTime = false
hideComments = false
+++

I am unemployed! It's great!

I have been (deliberately) out of work since June 2022.
It's been a magical time!

- I tried a lot of new recipes
- I spent time on my side-project [bostadsbussen](https://bostadsbussen.se)
- I visited Singapore, South Korea, Thailand, Nepal (Hiking Himalayas), Turkey, Italy, France
- I slept a lot
- I spent time with family

But I also used a lot of my savings.

So I'm looking for a job.

Anyone who has looked for a job has probably had this exchange:

```
<Monday>
Recruiter 1: Can I schedule a call? When are you free?
Alex: I'm free on Wednesday from 9 to 12.

Recruiter 2: Can I schedule a call? When are you free?
Alex: I'm free on Wednesday from 9 to 12.

Recruiter 1: Great, I'll call you on Wednesday at 9.

<Tuesday>
Recruiter 2: Great, I'll call you on Wednesday at 9.
```

A classic race condition! Now I need to reach out to `Recruiter 2` and re-schedule.
Then I would need to submit a time-slot again, which could create another race condition with a hypothetical `Recruiter 3`

This can result in a back and forth email chain just to find a suitable time. That is not very productive!

**So how can we solve this problem?**

If my calendar was online the recruiters could find suitable times themselves.

So let's put it on a web page!

I use Proton Calendar, which has an option to export my calender as an ICS link:

{{< image src="/img/proton.jpg" alt="proton calendar" position="center" style="border-radius: 8px" >}}

That gives us an ics subscription link, which can be imported into a calendar.
But expecting busy recruiters to figure that out is not really respecting their time.

I found [Open Web Calendar](https://github.com/niccokunzmann/open-web-calendar) which will generate an iframe
that can be pasted anywhere.

It was easily deployed to my Raspberry Pi! You can see the result below or on [dahl.dev/calendar](https://dahl.dev/calendar)

<iframe id="open-web-calendar" 
    style="background:url('https://raw.githubusercontent.com/niccokunzmann/open-web-calendar/master/static/img/loaders/circular-loader.gif') center center no-repeat;"
    src="https://calendar.dahl.dev/calendar.html?url=https%3A%2F%2Fcalendar.proton.me%2Fapi%2Fcalendar%2Fv1%2Furl%2FeYkhnRHSZGCoD-6_NduauI0dQI0wervw4BdGhAPEoE2HjgvzSEsTlSQZpKHlaCyuLMA0dsS27bSYN7mQldL8eA%3D%3D%2Fcalendar.ics%3FCacheKey%3D-A9AEDPi7ujoA_062BjTCw%253D%253D&amp;tab=month"
    sandbox="allow-scripts allow-same-origin allow-top-navigation"
    allowTransparency="true" scrolling="no" 
    frameborder="0" height="500px" width="100%"></iframe>

The UI breaks a little bit on mobile devices. But I think recruiters will be using a desktop when scheduling.
After this job hunt I might spend some time on creating a PR that fixes that 🤓!

No more scheduling conflict. I actually started using it for my job hunt and I have gotten good feedback from the recruiters. They said it
made their job so much easier!

So now all my interactions look like this instead:

```
<Monday>
Recruiter 1: Can I schedule a call? When are you free?
Alex: You can see my availability on dahl.dev/calendar

Recruiter 2: Can I schedule a call? When are you free?
Alex: You can see my availability on dahl.dev/calendar

Recruiter 1: Great, I'll call you on Wednesday at 9.

<Tuesday>
Recruiter 2: Great, I'll call you on Wednesday at 10.
```
