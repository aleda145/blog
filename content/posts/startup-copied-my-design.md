+++
title = "A startup copied my landing page (and then gave me great feedback on it)"
date = "2026-02-05T11:36:03+02:00"
author = "Alex Dahl"
authorTwitter = "alexdahl145" #do not include @
cover = ""
keywords = ["", ""]
description = "I caught a startup copying my design. When I confronted them, they admitted. And then gave me valuable feedback for Kavla"
showFullContent = true
readingTime = false
hideComments = false
+++

I stumbled upon a website that had something uncanny about it. This was the Hero:

{{< image src="/img/copyhero.png" alt="landing page hero" position="center" style="border-radius: 8px; width: 50%;" >}}

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  Their Hero.
</p>
After some more scrolling:
<p></p>

{{< image src="/img/copyworks.png" alt="how it works section" position="center" style="border-radius: 8px; width: 80%;" >}}

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  Their "How it Works" Section.
</p>
.

Wait.

This is getting really similar to [**Kavla**](https://kavla.dev) (my sideproject) "How it Works" section.

{{< image src="/img/kavlaworks.png" alt="Kavla how it works section" position="center" style="border-radius: 8px; width: 80%;" >}}

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  Kavla's section (now replaced)
</p>
<p></p>

I'm not the one to jump to conclusions though. This could just be a huge coincidence!

Then I checked their css:

```css
/* Yellow underline highlight 
like Kavla */
h3::after {
  content: "";
  position: absolute;
  left: -4%;
  right: -4%;
  bottom: -3px;
  height: 14px;
  background: var(--yellow);
  z-index: -1;
  transform: rotate(-0.6deg);
}
```

Hmmm. 🤔

To comfort myself on this ridiculous inspect-elemented theft I made some bad memes.

{{< image src="/img/athome.png" alt="landing page hero" position="center" style="border-radius: 8px; width: 80%;" >}}

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  sorry
</p>
<p></p>

That could have been the end of the story.

.

.

But I sent them a snarky email.

<div style="background: rgba(255, 255, 255, 0.05); border: 1px solid #444; border-radius: 6px; padding: 20px; margin: 20px 0; color: #ddd;">
    <div style="font-weight: bold; font-size: 0.9rem; margin-bottom: 8px; color: #fff;">
      Alex (Me)
    </div>
    <div style="font-size: 0.95rem; line-height: 1.6;">
      Hello! I'm building Kavla. I found your site and we have a very similar design! Coincidence!<br><br>
      Have you tried it out? What did you think?
    </div>
</div>

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  Might as well ask for some product feedback right?
</p>

<p>
  Instead, I got a quick and honest reply.
</p>

<div style="background: rgba(255, 255, 255, 0.05); border: 1px solid #444; border-radius: 6px; padding: 20px; margin: 20px 0; color: #ddd;">
    <div style="font-weight: bold; font-size: 0.9rem; margin-bottom: 8px; color: #fff;">
      Them
    </div>
    <div style="font-size: 0.95rem; line-height: 1.6;">
      Heyy! Yes i may have used you guys as an inspiration base... sorry im not even sure what your product does I just loved the landing page design.<br><br>
      Imitation is flattery right ?? My landing page is still evolving.. but yes for now I definitely took inspiration.
    </div>
</div>

Ok so they admitted to copying my design. Good for my ego!

But that they don't understand what Kavla does is concerning. I sent them a follow up email asking for more and got this response:

<div style="background: rgba(255, 255, 255, 0.05); border: 1px solid #444; border-radius: 6px; padding: 20px; margin: 20px 0; color: #ddd;">
    <div style="font-weight: bold; font-size: 0.9rem; margin-bottom: 8px; color: #fff;">
      Them
    </div>
    <div style="font-size: 0.95rem; line-height: 1.6;">
        <p>Honestly to tell you the truth looking at Kavla again I still don't understand what service you are providing. And I've been on the site a fair few times as I was using the look & feel as a direct inspiration.
        </p>
        <p>I'm a solo developer and recent founder. I've been working as a developer for quite some time. <strong>I honestly still have no idea from the landing page what the product is actually for.</strong></p>
        You put a lot of care and attention to this I can see and I really like the UI!        
        </div>
</div>

<p>
  Ouch.
</p>

<p>
  So we have this person, who is a former dev, now founder, that after looking at my landing page for hours to copy it, still doesn't understand <strong>wtf</strong> Kavla is about. 
</p>

That is... not good.

I'm aiming to wow people that work professionally with SQL (Data Engineers, Analysts, Data Scientists). They didn't fit this persona. But they should still be able to figure out what it is I'm actually building.

<p>
  We ended up exchanging a few more emails. I was gaining insight after insight, such a treasure trove!
</p>
{{< image src="/img/treasure_island.png" alt="Treasure island book cover" position="center" style="border-radius: 8px; width: 30%;" >}}

<p style="width: 80%; margin: 10px auto 0; text-align: center; font-style: italic; font-size: 0.9em; color: #666;">
  My own personal treasure, available over email! Long John Silver would be super jealous
</p>

I came to realize I had been over indexing on selling collaboration. I am super excited about how awesome tldraw (the canvas I'm using) makes multiplayer. I really wanted to go all in on showcasing it.

However, this led me to neglect selling Kavla's core utility. After all, multiplayer is something you only care about after you enjoy the product in single player.

I also tried to tighten it so it's clearer what the value prop is:

> Kavla is for messy, non-linear analytics where notes, screenshots and questions live just next to the SQL and charts.

The new landing page is available on [kavla.dev](https://kavla.dev). Check it out!

Shoot me an email, I'd love to know what you think (alex@kavla.dev). And try the product too!
