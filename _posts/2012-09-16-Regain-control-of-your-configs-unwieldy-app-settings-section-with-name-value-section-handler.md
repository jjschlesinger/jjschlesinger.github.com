---
layout: post
title: Regain control of your config's unwieldy appSettings section with NameValueSectionHandler
category : posts
tagline: 
tags : [.net, configuration]
---
{% include JB/setup %}

That’s a long title, but it sums up the topic of this post, so let’s get right into it.

You can create custom sections in your config without having to write a custom config handler if all you need is simple name/value pairs. Here is what the config will look like:

{% gist 6086073 %}

Setup your group and section (or just a section if you don’t want a group), and set the type on the section element to *System.Configuration.NameValueSectionHandler, System*

Now when you setup your keys, it is exactly like an appSetting key, just a key and a value. Now here comes the fun part, loading a value by section/key from your code. I have a handy dandy little helper method that makes this a breeze:

{% gist 6121640 %}

To call this helper is easy:

`
ConfigHelper.GetValueFromSection("OAuthKeys", "Development", "ConsumerKey", String.Empty)
`

The cool thing about this helper is that it handles the type casting for you. Say you had a key with an integer value, to get that value out as an int would be done like so:

`
int timeout = ConfigHelper.GetValueFromSection("OAuthKeys", "Development", "Timeout", 0);
`