---
layout: post
title: Setup an Azure Virtual Machine to Test Github Pages Posts
---

I think it's no secret that I'm a Windows guy, so testing out blog posts on my Github Pages site isn't the easiest thing to do.  Rather than try to get Jekyll working on Windows, I went for another route.  I'm always wanting to expand my knowledge and stretch what I know, so I decided to get a little closer to the environment that Github uses, which is Linux.

I'm writing this post on Surface Pro 3, and running VM's on it messes with the Instant On capabilities of the device.  In light of this, I opted to try an Azure VM loaded up with the latest Ubuntu image.  I used the wizard on the Azure portal to create a new VM using the 14.10 Ubuntu server image from the gallery.  I took all the defaults in the wizard with 2 exceptions.

1. I just set up a username and password instead of setting a certificate/key file.
2. I forwarded external port 80 to internal port 4000.

The first item is just to make my life initially easier.  You could just have easily set up a key, I just chose a username/password instead.  The second item will come into play later once we have set everything up on the box and are wanting to test our blog posts.