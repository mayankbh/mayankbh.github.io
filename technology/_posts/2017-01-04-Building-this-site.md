---
layout: post
title: "Building this site!"
category: technology
date: 2017-01-04
---

Thought I'd briefly cover how I got around to building this site.

## Hosting

After a lot of internal strife and angst, I finally decided to use GitHub Pages with Jekyll for hosting and static site generation (Since I wanted to avoid the headache of setting up CMS etc.). For now, the domain name is mayankbh.github.io, though hopefully I'll get around to changing it to a less verbose domain (eh, one I'll be paying for).

## Initial setup

Being a complete idiot at web development/hosting, the ability to write in Markdown is quite nice (it forces me to use my Markdown cheatsheet, for one >.>)

I followed the tutorial provided by Jonathan McGlone ([link](http://jmcglone.com/guides/github-pages/)) - if I managed to get it working, I'm pretty sure anyone can. However, after bootstrapping it, I felt the need to categorize my posts (since I will, in all likelihood, be (in an ideal world) covering a lot of topics in this website), and storing everytihng in a single \_posts/ folder didn't feel right. 

## Multiple blogs

That's when I came across another tutorial by Guillermo Garron ([link](https://www.garron.me/en/blog/multi-blog-site-jekyll.html)) discussing the exact problem - Jekyll provides a bunch of variables that can be accessed and he uses 'post.categories' to test whether a post belongs to a certain category or not, and add it to the category's index.html.

## Writing 'functions' in Jekyll

Because I'm lazy, I didn't feel like copying the code for each category. What if I want to branch out and add another one later? Copy-paste and update a single word? Ugh.

Hamish Willee has a nice tutorial on how to use \_includes as custom liquid functions ([link](http://hamishwillee.github.io/2014/11/13/jekyll-includes-are-functions/)) - and that's exactly what I followed to get the multi blog approach working without reusing those 6 lines of code for each category. Yay for laziness!

## Drawbacks

One drawback here is, of course, that I now have to create an \_posts folder in each category, and create a new index.html each time I add a new category. That, combined with the YYYY-MM-DD format that Jekyll forces you to use while naming your posts might get on my nerves eventually. Still, I guess it shouldn't be too hard to automate that procedure with a little script or something. Writing scripts to write blog posts about writing scripts to write blog posts - sounds like fun.

## Other things I should probably add

* Support for comments would be nice, I guess. Assuming I actually get any traffic. Ever.
* Also, probably finding prettier (though the default one provided is quite nice) CSS, however, I have absolutely no experience with this, and anything I come up with would probably hurt the reader's eyes
