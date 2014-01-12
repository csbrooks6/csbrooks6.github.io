---
layout: post
title: "TITLE HERE"
description: ""
category: 
tags: [ruby,rails]

---
{% include JB/setup %}

There are several thousand different cards in the wowtcg. When the game was active, new sets with a few hundred cards would be added every few months. So from the start, I knew I would need to automate everything possible. There's no way I would want to spend days entering all the new cards by hand when a new set came out, for example. 

I never even created any kind of admin interface for editing cards manually. I got my database of cards by scraping other websites. Initially I used wowtcgdb.com for the python version, but by the time I wanted to use Ruby, that site wasn't around anymore. So I switched to [WoW TCGBrowser](http://tcgbrowser.com), which has an excellent xml export feature. 

My own domain knowledge about the game was crucial. Each card can also have variants like a foil version, an "extended art" version (the picture is larger, text is smaller), alternate art. But not every card has every variant. And there's no authoritative list of which cards appear in which variants. I needed to know what each of these meant, which were important, and how to group together card prices. For example, I decided that reprinted cards should be grouped together with the originals, because there was almost no difference in their appearance, and so they should have about the same value.

I found several stores that I wanted to scrape data from. I sent out emails asking permission, and checking if there was any easier way to get the information than just scraping the site. I got only a few responses, and they were all positive. One store owner was so excited about the idea, he emailed me the password to his store to see if I could get the pricing information more easily! (I didn't use it, and replied and told him why it was a bad idea to email that to someone he doesn't know.)

All but one of the sites I ended up scraping "manually", but one was implemented using Yahoo stores. I found out that meant there was a special URL I could go to, to download all the product price and stock info via XML at once. That's such a simple feature to implement, I wonder why all stores don't do it.

When I initially implemented the site in python, I created a script that would write out the site as a bunch of static html pages, since my data only changed nightly anyway. Later when I switched to Ruby, I used Rails to make everything fully dynamic, for more flexibility. (And to solidify my Rails knowledge.)

I made a command line script I called "gather.rb", that was capable of scraping stores for prices, collecting and averaging those prices and inserting them into the database. I made each of those steps as loosely coupled as I could, so I could test and work on them in isolation. 

- The "scrape" step collects the product pricing info into a yaml file. This step didn't reference the card database at all, but some rules were encoded to decide based on the context what fields to set in the yaml. For example, if we're scraping a section of the site called "Extended Art Carts", all products we find there will have "is_ea" set to true. (The reason we don't want to map each yaml entry to an exact card here, is that some of them won't map properly until we do some sort of correction on them. I wanted the flexibility to gather information that I might not fully be able to use yet, and then later update the code or database to retroactively use that data. For example, say a new card set comes out, and I don't immediately add it to the database. Those cards' pricing info will still be written to yaml files, and when I update the db, I can go back and retroactively calculate and insert the pricing info.)

- The "insert" step takes the information in the yaml file, and maps each entry to an actual card. This can be tricky, because the names on store sites can have typos. So I use the "[FuzzyStringMatch](https://github.com/kiyoka/fuzzy-string-match)" gem to find close matches, where just a few characters have been swapped or changed. 

, for the "insert" step to use later. The "calc" step would only operate on the data that "insert" had inserted. I created a fake store scraper, that could quickly come up with a bunch of fake prices that were relatively sensible, so I could test the rest of the pipeline without waiting for real scrapes to finish.

-gather.rb to gather prices, and aggregate them, and insert into db
-fake scraper
-regexes to help normalize the names, and pick out flags
-isnt there something that corrects typos in here somewhere?
