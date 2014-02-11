---
layout: page
title: Hello!
tagline: index
---
{% include JB/setup %}

<img class="inset right" src="{{site.url}}/assets/images/chris128.jpg">

My name's Christopher Brooks, and I'm a software developer. I tought myself how to program when I was in fifth grade, on our TI/99-4a. Ever since then, I've had a passion for programming. I'm a fast-learner, and a self-reliant problem solver. I enjoy working on a small tight-knit team, on projects that matter to people.

Much of my career I've worked in games, usually with C++ and scripting languages such as lua and python. But I've also worked on significant web development, including designing and implementing an ecommerce engine. While at a web consultancy, I did work on toyota.com and on a prototype for orbitz.com.

I prefer working in a unix environment, and I've been using Ruby on Rails for side projects for a while.

## Resume: [http://chris.brooks6.com/resume.html]({{ BASE_PATH }}/resume.html)

## Github: [https://github.com/csbrooks6](https://github.com/csbrooks6)

## LinkedIn: [http://www.linkedin.com/in/csbrooks](http://www.linkedin.com/in/csbrooks)

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
