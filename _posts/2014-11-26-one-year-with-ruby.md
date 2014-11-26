---
layout: post
title: "One Year With Ruby: What I Like and Dislike"
description: ""
category: 
tags: [ruby]
---
{% include JB/setup %}

I came from a long-time C++ background, working in games for many years. 

For nearly the last year, I've been working full-time building and enhancing a website implemented in Ruby on Rails. One of the major appeals of this project for me was the chance to use and build on what I'd learned doing side projects in RoR. 

Here are my thoughts about Ruby nearly one year in.

# What I Like

## Dynamically Typed

Coming from C++, it was very liberating not having to specify the type of every variable. I overestimated the number of bugs that would result: for the most part, passing the wrong type ends up causing a clear exception pretty quickly, and is probably the easiest kind of bug to track down. And it happens much less frequently than I would have guessed. The tradeoff in terms of reduction in code size has been totally worth it. 

Having said that, the code only throws an exception if it gets run. I wonder if some of the emphasis on TDD in the Rails community is to make up for the lack of static type checking, by trying to ensure every code path is run regularly. 

## Very Expressive

You can do so much with so little code. But unlike perl, the code ends up readable. So readable in fact, that self-documenting code actually seems practical a lot more often.

Having expressive code makes it so much easier to hold a complex problem in your head, when you can see all the related code on one page.

## Blocks

Being able to pass a bit of code to functions isn't new, but Ruby's block syntax makes it so simple to do that you end up using it everywhere.

## Symbols

They're like C++ enums that you don't have to declare ahead of time! I'm a big fan of symbols. Yes, you could use string constants instead, but symbols feel so much cleaner and more efficient.

## Function Calls Without Parentheses

In many places, parentheses are optional when calling a function in Ruby. 

It makes the calling code so much cleaner:

	  class Train
	    def cars()
	      [:engine, :coal, :caboose]
	    end
	  end
  
	  puts train.cars  # So clean!


# What I Dislike

## Skipping Parentheses With Function Calls Causes Ambiguity

From O'Reilly's "The Ruby Programming Language":
	  
	  puts(sum 2,2)  # Does this mean puts(sum(2,2)) or puts(sum(2),2)?

Also, because skipping the parenthesis is possible, if you wish to use the parentheses you can't leave any space between the function name and the start paren:

	  sum(1,2) # ok

	  sum (1,2) # bad

I started off skipping the parens wherever I could, but I've come around to only skipping them in limited cases. They're great for making:
	  
	  a_string.length

look like a property access, for example. Maybe not so great when you're doing:

	  my_object.do_something :now, 3, 'hello'

And just confusing if you're doing nested calls:

	  puts sum 1,2  # Gross!

## {} - Block or Hash?

	  def func(arg)
	    puts arg
	  end

	  func {} # Ruby thinks I'm passing a block; "I get ArgumentError: wrong number of arguments (1 for 0)"

	  func({}) # This actually passes the hash in

This seems like a minor nitpick, but it makes me feel like being able to skip parens, and {} being used for both blocks and hashes, leads to some unintuitive edge cases.

## Attributes vs Local Variables

What's bar do?

	  class Foo
	    attr_accessor :var

	    def bar
	      var = 5
	    end
	  end

That's right! It creates a local variable called "var" and throws it away! 

You have to do `self.var = 5`. This is kinda crummy because most of the time, you don't need the `self.` But in this one case, because calling a member assignment function would look just like assigning to a new local variable, leaving out the `self.` results in a silent failure that can be a pain to track down.

## Unused Blocks are Ignored

It's easy to do this by mistake:

	  MyModel.where(name: 'bob') { |m| puts m }

The block never gets called, because where() doesn't use its block; you need to do:

	  MyModel.where(name: 'bob').each { |m| puts m }

It feels like that block passed in where it isn't needed should result in an error, just like passing an argument to a function where it isn't needed does. But because the `yield` syntax for using a block doesn't require anything in the function declaration line, Ruby doesn't have an easy way to detect if the block is ever used. 


## DSLs (Domain Specific Languages)

Ruby makes DSLs practical to create, and powerful. As long as you're using the DSL more or less as its creator intended, everything's great, and you get to deal with things at a higher level of abstraction.

The problem can come when, as a user of the DSL, you try to DRY up your code, because you have the same set of steps with tiny differences happening in multiple places. You may end up having to learn how the DSL is actually implemented, which busts the abstraction wide open.

# Summary

I've had a great time learning and using Ruby. It's a wonderful language, and I use it everywhere I used to use Python. Yes, there's some quirks, but the positives far outweigh the negatives.
