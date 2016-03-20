---
layout: page
title: Hello!
tagline: index
---
{% include JB/setup %}

<img class="inset right" src="{{site.url}}/assets/images/chris128.jpg">

My name's Christopher Brooks, and I'm a software developer. I taught myself how to program when I was a kid, on my TI/99-4a. Ever since then, I've had a passion for programming. I'm a fast-learner, and a self-reliant problem solver. I enjoy working on a small tight-knit team, on projects that matter to people.

Much of my career I've worked in games, usually with C++ and scripting languages such as lua and python. 

I've also worked on significant full-stack web development projects. For example, I built the backend for the social impact finance searching and matching site [Enable Impact](http://search.enableimpact.com), using Ruby on Rails and Javascript.

I'm currently the Tech Director at [Airship Syndicate](http://www.airshipsyndicate.com), an indie game studio I helped create, working to build [Battle Chasers: Nightwar](http://www.battlechasers.com/), a game based on Joe Madureira's famous comic book series. We raised over $850,000 on [Kickstarter](https://www.kickstarter.com/projects/1548028600/battle-chasers-nightwar), and partnered with publisher [Nordic Games](http://www.nordicgames.at/).

## Resume: [http://chris.brooks6.com/resume.html]({{ BASE_PATH }}/resume.html)

## Github: [https://github.com/csbrooks6](https://github.com/csbrooks6)

## LinkedIn: [http://www.linkedin.com/in/csbrooks](http://www.linkedin.com/in/csbrooks)

## Twitter: [@csbrooks6](https://twitter.com/csbrooks6)


## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
