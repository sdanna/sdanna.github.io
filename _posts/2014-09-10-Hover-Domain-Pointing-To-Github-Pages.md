---
layout: post
title: Pointing My Domain at Hover to Github Pages
---

The first order of business to standing up my blog was to point my domain registered at Hover at my Github Pages repository.  Now I'm not a networking guy, but being a webby type person, I reckoned that I could figure it out.

After a little reading of the help [documentation](https://help.github.com/articles/about-custom-domains-for-github-pages-sites), I was ultimately going to need to do two things:

1.  Use A Records to point our domain to the IP addresses that Github uses to serve the Jekyll blog.
2.  Use CNAME Records to point our top level domain as well as the www subdomain to my Github Pages site at sdanna.github.io.

My config for that looks like the below:
<img src="{{ site.baseurl }}public/images/Hover-Github-Pages.png" alt="Hover Configuration for Github Pages" />

It's important to note that when you are creating the CNAME records that you put the trailing dot (.) after the url.  Otherwise you will not have a good time.
