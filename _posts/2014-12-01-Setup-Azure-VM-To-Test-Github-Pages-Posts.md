---
layout: post
title: Setup an Azure Virtual Machine to Test Github Pages Posts
---

I think it's no secret that I'm a Windows guy, so testing out blog posts on my Github Pages site isn't the easiest thing to do.  Rather than try to get Jekyll working on Windows, I went for another route.  I'm always wanting to expand my knowledge and stretch what I know, so I decided to get a little closer to the environment that Github uses, which is Linux.

I'm writing this post on Surface Pro 3, and running VM's on it messes with the Instant On capabilities of the device.  In light of this, I opted to try an Azure VM loaded up with the latest Ubuntu image.  I used the wizard on the Azure portal to create a new VM using the 14.10 Ubuntu server image from the gallery.  I took all the defaults in the wizard with 2 exceptions.

1. I just set up a username and password instead of setting a certificate/key file.
2. I forwarded external port 80 to internal port 4000.

The first item is just to make my life initially easier.  You could just have easily set up a key, I just chose a username/password instead.  The second item will come into play later once we have set everything up on the box and are wanting to test our blog posts.

## Setting up the box with Jekyll

Once Azure tells us the virtual machine is up and running, we can ssh into using the username and password we set up earlier.  Use the following command substituting your username and machine name.  You'll be asked for a password; enter in what you set up during VM creation and then you'll be in!

{% highlight bash %}

ssh username@machinename.cloudapp.net

{% endhighlight %}

This is a blank virtual machine with nothing on it and the meat of this blog post is getting it to the point where you can run Jekyll.

### Update apt-get and install prerequisites

Before we can get Jekyll and ruby up and running, we need to make sure our apt-get is updated and we have a few things on the box.  Run the following commands.

{% highlight bash %}

sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get install -y git-core curl nodejs

{% endhighlight %}

Next, we'll get Ruby up and running using [RVM](https://rvm.io/).  For those just getting started, you could probably skip this step, but from what I understand from my Ruby friends, you want to use RVM to allow for different projects on a server to use different versions of Ruby.  This is a very similar setup to what ASP.NET vNext will use, so you may want to get familiar with the concept.  The instructions are paraphrased from [here](https://rvm.io/rvm/install).

{% highlight bash %}

gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable --ruby

{% endhighlight %} 

The above will put RVM on your system and install the stable version of ruby.  I found I needed to exit the linux session and then ssh back into the box to get access to the RVM command.  Once you've done that, you can finish out by installing the correct version of ruby and getting bundler installed.

{% highlight bash %}

rvm install 2.0
gem install bundler

{% endhighlight %} 

### Your Repository and Jekyll

You should clone your repository, cd into the repository root, and ensure that you have a Gemfile in your repository root with the following contents.

{% highlight bash %}

source 'https://rubygems.org'
gem 'github-pages'

{% endhighlight %} 

Once you have that, use bundler to make sure all the gems are installed from the Gemfile.

{% highlight bash %}

bundle install

{% endhighlight %} 

Once that gets finished, you should be good to go.  Just run the following command to start your server on port 4000.

{% highlight bash %}

bundle exec jekyll serve

{% endhighlight %} 

Use your browser to hit your virtual machine, using the cloudapp.net address.  This will hit port 80 on the box, which if you remember from earlier we forwarded to port 4000 internally.  You should see your github pages site!