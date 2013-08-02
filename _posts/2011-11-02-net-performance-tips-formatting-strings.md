---
layout: post
title: ".Net Performance Tips: Formatting Strings"
description: ""
category: posts
tags: [.net, performance]
---
{% include JB/setup %}

There are a couple techniques I use in assisting in writing good performing .Net code. The topic I wanted to talk about in this post is one of those techniques: "What are those common framework methods actually doing?". One of the the things I love about .Net is that it provides hundreds of helpful utilities for handling common scenarios, especially when it comes to manipulating strings. I am going to focus on the String class methods since it is generally the heaviest used class in an application (just run a memory profiler on one of your apps and you'll see what I mean). There are a few different factors when it comes to performance, but in this article I will focus strictly on speed.

### "What's the difference between String.Concat and String.Format?"

I used to work at a company that had a coding standard that said "Always use string.Format over string concatenation". The reason for this is that they believed String.Format performed better. This is actually incorrect, and to see for yourself just fire up .Net Reflector and browse to System.String inside mscorlib. Open up the format method and you will see that it creates a StringBuilder object and calls AppendFormat on your string. Now take a look at the Concat method, no fat objects here, just some simple array manipulation. One thing to note is that when I say String.Concat, this also applies to when you use the + operand to concatenate strings since it gets called behind the scenes.

OK sure. StringBuilder is a pretty heavy object, but how do we really know which one of these methods is faster? Lets write a simple test app using the Stopwatch class from the System.Diagnostics namespace to collect some simple timing metrics.

{% gist 6136557 %}

All this app does is call our two string methods a million times and track how many milliseconds has elapsed. Each test is then performed 5 times (actually 6, but we throw away the first test since it can skew our times). We output each run and then average out the time across all 5 operations at the end.

So with a little help from our good friend .Net Reflector and a simple test app, we were able to prove how different these two common methods act. Now, before you go rewriting all of your code and replace String.Format with String.Concat keep in mind that in most situations the performance difference is negligible (change the value of maxLoop to 1000 and you’ll see what I mean). Sometimes I prefer the readability of one over the other which may outweigh the small performance cost you might incur. The moral of the story is to understand what your code is doing (and what Microsoft’s code is doing) so you can make an educated decision about it.