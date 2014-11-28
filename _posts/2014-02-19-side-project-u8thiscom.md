---
layout: post
title: "Side project: u8this.com"
description: ""
category: 
tags: [ruby,rails,js,capistrano,rspec,sideproject]
---
{% include JB/setup %}

Over the last month (first checkin Jan. 23), I've been working on a side project. It's a food diary site, for calorie-counting. I'm working on losing weight, and wanted to do something more complicated in rails than my [last project](/2013/12/08/timing/).

You can find the site here: [u8this.com](http://u8this.com) Feel free to make an account and mess around with it. (Put garbage data in if you want to see how it works; I won't be offended.)

The code is here: [github.com/csbrooks6/u8this](https://github.com/csbrooks6/u8this)

I've tried to use all the latest tools in building it:

- Rails 4.0
- RSpec
- Developing on osx, using Sublime Text 3
- Deploying to linode using Capistrano
- postgres
- Trello to track my TODO list

I wanted to document some of the different challenges I ran into, and techniques I used. 

##Idea

The idea, in my mind anyway, isn't revolutionary. At least in terms of getting users, chances are low I'm going to be able to peel users away from the myriad other food diary sites. And even if I could, I think monetizing is unlikely. 

<img style="border:3px solid #808080" src="{{site.url}}/assets/images/u8this_food.jpg">
*[Photo by epSos.de](https://www.flickr.com/photos/epsos/)*

There's upsides to this idea, though: 

- Solidify my Rails knowledge
- Use some different things I haven't used before, like RSpec
- It's something I personally want to use, so I can make it work exactly the way I like

## Git: Locally, linode and github.

From the start I used git for source control. First a local repository, then I created a remote on my linode server, and now Github. I haven't yet switched completely to Github, so I'm still pushing all changes both to my linode and github at the same time.

I mostly use SourceTree for osx, which I really like. Clean interface, very flexible. My only complaint is that I've been getting dramatic slowdowns at times. Other than that, it's awesome.

I've mostly been working straight out of the master branch, though as the site has gone online, I've made a dev branch and switched over to that, so I could do a quick bugfix in master and push it live without waiting for my dev changes to be complete.

##RSpec, and TDD

This is my first real foray into test-driven development. I found a good e-book called ["Everyday Rails Testing with RSpec"](https://leanpub.com/everydayrailsrspec) that I used to help me figure out what I was doing. 

I have a number of RSpec tests now. I haven't yet built any integration tests, just model and controller specs. I also don't really have 100% coverage. I guess I'm still not fully in the mode of writing the tests first and the code second. I need to focus on that more, because it's worked well when I try it. (Although one disadantage is that if you are still learning RSpec, it's easy to write tests that aren't correct. Then inevitably you end up rewriting the tests anyway as the code comes online. It's getting easier, though.)

All in all, I've really enjoyed RSpec.

##Authentication

I did some research, and settled on [Authlogic](https://github.com/binarylogic/authlogic) for account authentication. Seemed full-featured and well-supported, and there was even a decent example project on their github site that I used as a reference point. 

<img style="border:3px solid #808080" src="{{site.url}}/assets/images/he_chose_poorly.jpg">


I probably should've been warned away when I figured out the sample was written for Rails 2. It took some finagling to get it running in Rails 4. I emailed the author and offered to submit a pull request to update the sample to Rails 4, but no reply. (Another warning?)

The bottom line is, sadly, Authlogic doesn't seem to be in active development. I get a bunch of rails deprecation warnings when I run my RSpec tests against it, that I think are fixed in a pull request, but that either hasn't been integrated or something. 

The warnings look like this:

	DEPRECATION WARNING: ActiveRecord::Base#with_scope and #with_exclusive_scope 
	are deprecated. Please use ActiveRecord::Relation#scoping instead. (You can
	use #merge to merge multiple scopes together.)

Super annoying to get dozens of those when I rspec. 

Anyway, now I've figured out everyone is using [Devise](https://github.com/plataformatec/devise). It's on my TODO list to switch everything over.

##Bootstrap

I decided to use Bootstrap, to implement a responsive design. Working well on both smartphone browsers and PCs was important to me. Bootstrap also makes things look a lot slicker than I would have had time to do.

<img style="border:3px solid #808080" src="{{site.url}}/assets/images/u8this_bootstrap.jpg">

I used Bootstrap 3, but it hasn't been out terribly long, which means a lot of gems and stackoverflow questions are targetting Bootstrap 2.3 instead.

I used a gem called [bootstrap-sass](https://github.com/twbs/bootstrap-sass). In retrospect, I think it would have been simpler just to directly integrate the css and js downloaded directly from Bootstrap. There's a particular issue I'm running into with Bootstrap that I think is fixed in the latest version. So I'm planning to try updating the bootstrap-sass gem, and if that breaks, I'll look at getting rid of it and just putting Bootstrap into the asset pipeline myself.

The form controls look nice, though it took plenty of fiddling to get them to work right. I also lost all the clean Rails helper functions for views to emit form controls. There are gems like [simple_form](https://github.com/plataformatec/simple_form) that can help with that, but at the moment, it targets Bootstrap 2.3.

##Typeahead.js and Bloodhound

I used Twitter's [typeahead.js](http://twitter.github.io/typeahead.js/) library for entering foods, so ones you'd previously entered would autocomplete. (I intentionally chose not to autocomplete to stuff other users had entered, because God knows what they put in there.)

Typeahead used to be part of bootstrap, but is now its own thing. I was a little disappointed with it, just because I had the idea it was going to have something cool behind the scenes to intelligently make queries with my dataset. It doesn't do that though, and in the end my controller-side code is doing some horrible `LIKE` sql query that I'm still feeling iffy about. (It's surprisingly hard to handle all the edge cases in using user-submitted data with `LIKE` through Rails, without letting bad data through. I couldn't google up any clean database-independent way to do it.)

Typeahead is all about just knowing when it needs to request data, and showing the results that come back, and letting the user autocomplete or arrow-key select them. Basically, just the user interface side.

It can be used together with something called [Bloodhound](https://github.com/twitter/typeahead.js/blob/master/doc/bloodhound.md). Aha, so that's the part that intelligently finds search results, right? Well, not quite. It's job is to manage preloaded data, requesting more data, caching and rate-limiting those requests. But in the end you'll still be making a request to your site with a string query, and it's up to you to figure out what results match that string. `LIKE` to the rescue!

Typeahead also had some weird interactions with Bootstrap (surprising since they're both from Twitter). It's totally possible I'm doing something wrong, but the Bootstrap input text control text hints fight with the autocomplete hints that Typeahead shows. I ended up just turning off my Bootstrap hint text, for now. 

I also ran into something weird where calling the typeahead() function on my input form control made the rounded edges that Bootstrap puts on it, disappear. After some investigating I think I tracked this down to typeahead "wrapping" the form control element when that function is called, which makes the css for the rounded corners not work anymore. 

I [tried to repro this in a jsfiddle](http://jsfiddle.net/csbrooks/m4LT3/), but it doesn't seem to happen there. So I'm thinking it's something with the version of Bootstrap I'm running, which is why I want to upgrade.

Anyway, for now, after I call typeahead() I just do this: `$('.foods.typeahead').css('border-radius', '4px');`

Gross, but it works. Like Javascript! I kid, I kid.

##Javascript/Coffeescript, AJAX

I wanted to avoid having an ugly page reload when you modify or add "Servings" (my name for the ActiveRecord behind "something you ate"). So I used AJAX calls, using the "remote" method on my forms, and jquery/coffeescript listening for the AJAX to return. (Actually I didn't use XML, I used JSON.) 

The add and update controller methods would put the html for the new or modified Serving in the JSON object. Other methods would refer to the id of the Serving to be deleted, or moved up or down, or whatever. Then the coffeescript would update the page as appropriate.

One minor hitch was that I show a progress bar at the top of the page with the number of calories, and I wanted that to reflect any changes. So I took the most straightforward approach, and had the controllers just render the view partial for the progress bar and stuff that in each of the JSON return values, too.

A more major hitch I ran into was that the AJAX seemed to stop responding when I would navigate to the next or previous page. This is simpler to describe now than it was to track down, but it turns out that turbolinks makes jquery's $(document).ready not get called, which causes all sorts of madness. The answer is a gem called [jquery-turbolinks](https://github.com/kossnocorp/jquery.turbolinks), which fixes this issue. (My first impulse was to turn off turbolinks, but the site did respond much slower. Although that was with `rails server` in development on my macbook, so probably not a great test, but still.)

One thing I wanted to do was show a context menu when you click a Serving on the page. Bootstrap doesn't directly support this, but I found some code online that did the trick, mostly. I had to convert it to Coffeescript from Javascript, and make various tweaks for my needs. I also threw in some of the "glyphs" that Bootstrap comes with. I'm pretty happy with the results:

<img style="border:3px solid #808080" src="{{site.url}}/assets/images/u8this_context_menu.jpg">

##Capistrano

I heard this was the way to go for deployment. It really wasn't too difficult to setup, though I got stuck for a while getting the ssh-agent forwarding to work. (That's so that my private key on my macbook can essentially be used to issue commands to my linode server.)

I actually even got a staging server running while I was at it, so I could have an environment to test my deployment to linode before pushing it live.

Now every time I do "cap production deploy" and watch it do its thing, I pretty much feel like a badass. I've even been known to shout out *"I am become death, the deployer of worlds!"*, which no one thinks is funny but me.
