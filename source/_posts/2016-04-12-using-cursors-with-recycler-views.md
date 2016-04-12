---
layout: post
title: "Cursors & RecyclerViews, Smurf Yeah!"
permalink: using-cursors-with-recycler-views
date: 2016-04-12 15:26:27
comments: true
description: "using-cursors-with-recycler-views"
keywords: ""
categories:

tags: android recyclerview cursors adapter content provider

---
Every time I start a new Android project I know for a fact that there will be
some sort of content provider/resolver laden snake pit that I will inevitably fall
into. Most recently it was trying to get RecyclerViews to play nicely with Cursors.
Cursors, of course are what gets returned by a content resolver when it queries the SQLite
datastore that comes baked into Android [^1].  

The issue is one of multiple inheritance. a standard RecyclerView adapter definition looks like:

{% gist parkr/c08ee0f2726fd0e3909d test.md %}
[^1]: <small> I know what you're going to say: "Don't use SQLite! Use ----", where ---- is one of a </small> number of options. This post isn't for you </small>
