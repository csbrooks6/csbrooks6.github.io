---
layout: post
title: "'Carmack Lines': Playing with Javascript"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I read [this tweet](https://twitter.com/ID_AA_Carmack/status/418976175533223936) by John Carmack:

> Visualize an image where a pixel is on if a given bit is set in the sum of the x and y coordinates. I had poor intuition for its character.

I too had "poor intuition for its character", but I'm pretty sure that's not because I'm as smart as he is or anything. But still, I was curious how it would look.

I've also been looking for an excuse to play with Javascript and html5 canvases, and I couldn't sleep at 5:30 this morning, so I popped up and had at it. 

It took me less than an hour to get this running, even though I haven't done anything significant in Javascript in years. And only maybe 20 minutes of that was the basic javascript, the rest was getting the form working right to let you dynamically change which bit is tested.

I think that's more a testament to the power of [stackoverflow](http://stackoverflow.com/) than anything else. Maybe ten times while implementing this I found the answer there, where before, it would have taken a lot more reading and experimentation. It's impressive that a single site with user generated content has had such an impact on the programming process. (At a meetup I went to, another programmer said to me "I can't imagine programming before stackoverflow!" I wanted to say "imagine what it was like before we had the internet", but I try to repress my inner old fogey when possible.)

I'm kind of amazed at how fast Javascript executes, at least in Chrome. I'm iterating over 16,384 pixels every time you hit the button, and there's no perceptable delay at all, at least on my Macbook. Makes me want to do some more experiments with graphics in javascript.

Anyway, [here it is](/static_html/bit_image.html).
