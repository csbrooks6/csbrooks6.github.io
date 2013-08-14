---
layout: post
title: "Side Project: WoWTCG Pricelist"
description: ""
category: 
tags: [wowtcg, pricelist, python]
---

{% include JB/setup %}
I got totally hooked on the World of Warcraft Trading Card Game, from the time it came out six or seven years ago. (It's like Magic: The Gathering, but without mana screw or mana flood. And the players tend to smell less bad. Plus it's the Warcraft universe!)

I went from being a casual player who didn't really understand the rules, to spending hours looking over cards online and trying to work out decks. Then I started going to local weekly tournaments and connecting with local players, and then things really spiralled out of control. I ended up driving to regional tournaments that were a couple hours away, probably once a month or so. Eventualy I ended up going to nationals at GenCon a couple times. (I didn't do terrible, but I didn't do amazingly well either: finished about middle of the field.)

If you aren't familiar with trading card games, you open "booster packs" that have a bunch of random cards. Some cards are a lot more rare, and useful, and therefore valuable than others. There are stores online that will sell "singles", so rather than buying random boosters you can just pay for the cards you want. These cards vary in price from $100 at the very extreme end, but most are 25 cents, or a dollar, or maybe five or ten dollars. 

What I found was that sometimes it was difficult to work out on-the-spot if a given card is worth a dollar, or five, or ten, which made trading tricky. So I wrote a script in Python that would scrape five or six different online stores for prices on singles, and build up an html pricelist with averages every night, so at least people would have a rough idea what a given card is worth.

The website is at [hexagog.com](hexagog.com), and I posted it under my forum name of "Conrad Hex", which I use so no one can find out my true identity. Crap, I guess that's blown.

Anyway, the site worked well for a long time. I loved it, other people loved it. I got emails of appreciation, and even got some free products from one of the developers of the game. But about a year ago it fell into disrepair. The site I used to update my card list database had gone away, and I wasn't really active in the game itself anymore. Everyday I would get a sad little cron job email with a python exception, because one of the sites had changed their page layout, and the scraper couldn't do its job any more.

Things stayed that way until I decided about a month ago to bring it back to life, this time in Ruby on Rails.

*To be continued...*

__That's me on the right with the red shirt, geekin' out hard:__  
![Chris playing wowtcg]({{site.url}}/assets/images/chris_at_realm_champs.jpg)

