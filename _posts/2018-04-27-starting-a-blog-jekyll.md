---
layout: post
title:  "Starting a blog in 2018, what are the options?"
date:   2018-04-27 00:30:00 +1000
category: Review
tags: 
    - Jekyll 
    - NextJS
    - Hugo
    - Blog engine
    - Static content generator
image880x430: "2018-04-27-starting-a-blog-jekyll880x430"
image280x280: "2018-04-27-starting-a-blog-jekyll280x280"
---

Last time I started a blog I used Wordpress because it was the default back then but I wasn't sure if that was the case this days and with this new attempt to blog came a new search for the perfect blog engine.
<!--more-->

### Current state of blog engines 

The first thing you'll notice if you start that kind of search is that everything points you towards static blogging or site generators, what is this? Basically it means that your site/blog is generated at compile time vs the classic approach of generating the content dynamically per request.

### Why static generators?

I won't go to deep in this because there is plenty of info out there and I'm just crushing late to the static blog party, but to recap the benefits:

1. You don't need a DB: just generate your content in files (usually in markdown) and the compiler takes them and generates the static HTML files. Nothing gets saved to a DB. That is just mind blowing, since the dead of the XML I never thought we would go back to plain old text files. 
2. Is crazy fast: with a dynamic blog there is overhead everywhere to get just a piece of static text: you need to process the code in the server, usually that codes communicates with a DB and then you have a rendering engine on top for the presentation layer, that usually does not scale very well. Consider that vs serving a static html file.
3. Security: I had to update Wordpress from time to time because of security risks, I dare you to hack an HTML. Of course you can have an attack on your server but at least not from the application level.
4. Easily to attach the blogging process yo your current dev one: you can use the same tools you use for dev, in my case I'm using Visual Studio Code and git, that is all I need to get this thing going.
5. Functionality is all about JS: JavaScript frameworks came a long way since the old Wordpress days, now everything that you use to do in the backend site, you can do it on the front end giving you the ability to create complex applications even using static files as the backend.

### The players

I started looking for one in .Net as that is the platform I like the most but I found a couple and the most popular one, [pretzel](https://github.com/Code52/pretzel), seems to be abandoned. When I broaden my search I've found that the most popular ones are in ruby.

I started looking at [Jekyll][jekyll], [Next][next] and [Hugo][hugo] (this last one is built on [Go](https://golang.org/)). While the three make the same end result they are very different in terms of functionality and configuration.

1. [Next][next]: You can actually do a lot more than blogging with this generator, as in reality it's a framework for statically-exported React apps. So I think using this for a simple blog is too much.
2. [Hugo][hugo]: The main focus of this generator I think is it's speed, and if you take into account that when you update your blog content, you have to compile **everything** you end up relaying a lot on the speed of the generator. I know that most of the generators can do incremental generation (only parts that are changed are generated) this depends on the deploy structure that you have in place.
3. [Jekyll][jekyll]: This is the most used generator, is not that flexible but I has come a long way, it supports plugins as well.

### The winner

I'm just starting to blog in this new paradigm so I picked the most obvious one: Jekyll. I've read that it's not that flexible as the other two options but I want to test it's flexibility myself. Also one great feature is that is the enginge behind [Github pages](https://pages.github.com/) so you can actually host your Jekyll blog for free, that is a no-brainer for me, at least until I try this thing out with a couple of blog posts.

Do you have a preference for a blog engine? Let me know in the comments. 

[jekyll]:   https://github.com/jekyll/jekyll
[next]: https://learnnextjs.com/
[hugo]: http://gohugo.io/