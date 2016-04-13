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

The issue is multiple inheritance, and the fact that it conflicts with the [ViewHolder](http://developer.android.com/training/improving-layouts/smooth-scrolling.html#ViewHolder) pattern.  
To see what this means take a look at what a standard RecyclerView adapter and CursorAdpter definitions looks like:

{% highlight Java %}
class MyAdapter extends RecyclerView<MyAdapter.MyViewHolder> {

  public MyAdapter()

  //inflation and binding of view holder happens here


  public class MyViewHolder extends RecyclerView.ViewHolder {  
      // define your view holder
  }
} // adapter
{% endhighlight %}

{% highlight Java %}
class CustomDataAdapter extends CursorAdapter {

  public CustomDataAdapter()

  //bind view and new view go here

} // adapter
{% endhighlight %}


As you can see, the ViewHolder pattern is enforced by default in any RecyclerView adapter via inheritance and
a CursorAdapter also uses extension of the CursorAdapter interface in order to conform to protocols.
This creates an issue when you want your RecyclerView to use your cursor data via it's adapter.
Since Java doesn't have multiple inheritance through extension we are forced to do a little
magic with generics.

If you're like me, you've avoided generics like the plague, because lazy.
However, in this case generics provide the most elegant solution.  By creating
a base cursor recycler adapter we can abstract away a lot of the boilerplate and headache.






[^1]: <small> I know what you're going to say: "Don't use SQLite! Use ----", where ---- is one of a number of options. This post isn't for you </small>
