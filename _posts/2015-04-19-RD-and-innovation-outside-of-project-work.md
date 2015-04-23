---
layout: post
title:R&D and Innovation outside of project work
---

My [company](http://praesesbt.com/) recently conducted its biannual [Shipit Day](https://www.atlassian.com/company/about/shipit) inspired Hackathon.  Instead of 24 hours once a quarter, we have settled on 48 hours twice per year.  It's a pretty good compromise between the original idea and being able to accommodate our family oriented environment.  Many of us have young kids and can't commit to staying overnight at the office to see our ideas brought to life.  We had some pretty awesome projects this time around and since I'm starting to blog, I figured I would highlight them here.  Before I do that, I wanted to mention that the line of business that I work for is a traditional consulting shop, so taking everyone off of billable projects to work on things like this is a pretty big deal since it has a direct impact on revenue.  The thing to remember about hackathons is that the output can be used to show potential clients of your company's capabilities and can serve as a powerful sales tool and get you projects that are outside of your typical wheelhouse.  It is effectively R&D to facilitate sales by demonstrating capabilities.

##Dynamic Forms and Mithril
We have a dynamic forms product that we put into some of our applications that allow us to easily update forms and the backing data structure simply by altering a versioned JSON document.  We have iOS and Android applications that use the technology, but until now we didn't have a web client nor did we have an editor to allow a non-developer to update the forms.  The team I was a part of decided to fix this glaring deficiency.  Using bootstrap and a client side MVC framework called [Mithril](http://lhorie.github.io/mithril/).  Since the backing store is a JSON document, it made working with the data remarkably easy.  At the end of the day we were able to support the majority of the control types that our Android and iOS applications support including composite controls.  We didn't get a chance to make a couple of of the more advanced controls like signature capture or image upload, but we have ideas on how to implement them and a pattern for where they would fit in.  On the editor side, we were able to get a UI up where a business analyst could create new forms and update existing forms by creating form sections and defining the controls on the form.


##3D Drawings in the Browser
We have one client that have a Google Earth tour of their facilities that overlays 3D models on top of the Google Earth maps as part of a virtual facility tour.  With the recent [announcement](http://googlegeodevelopers.blogspot.com/2014/12/announcing-deprecation-of-google-earth.html) that both Chrome and Firefox won't be supporting the NPAPI plugin framework any longer, it became prudent to find an alternate solution for our customer.  The team working on this found a library called [x3dom](http://www.x3dom.org/) that uses WebGL to render models in the browser.  



##Visualization and Graph Databases
Another of our teams began looking graph databases/Triple Store databases and then using the [D3.js library](http://d3js.org/) to show relations between nodes.    


##Phonelist and ActiveSync 