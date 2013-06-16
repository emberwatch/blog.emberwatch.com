---
layout: post
title:  Logging the Magic in Ember.js
date:   2013-06-16 18:24:17
author: Philip Poots
tags: logging debugging ember.js
---

Ember.js is heavily based around a philosophy of <em>convention over configuration</em>. For example, by adhering to strict naming conventions, you can significantly reduce the amount of boilerplate code you have to write.

This is fantastic when you are familiar with the framework, but as a newcomer it can be quite frustrating trying to work out what’s going on. However, there is help available.

Ember provides three logging capabilities that reveal what it’s doing under the hood. They are each exposed as properties on your application object.

### Log Transitions

The first is `LOG_TRANSITIONS`, which scope is limited to the application’s router. Enable it like this:

{% highlight js %}
var App = Ember.Application.create({
  LOG_TRANSITIONS: true;
});
{% endhighlight %}

Now each time your application transitions from one route to another you’ll get a log message in your browser’s JavaScript console:

{% highlight console %}
Transitioned into 'index'
{% endhighlight %}

### Log View Lookups

Toggling `LOG_VIEW_LOOKUPS` works in much the same way and is the domain of routes and views:

{% highlight js %}
var App = Ember.Application.create({
  LOG_VIEW_LOOKUPS: true;
});
{% endhighlight %}

Now, every time that a route looks for a template or view, it will log details of the search to the console:

{% highlight console %}
Could not find "index" template or view. Nothing will be rendered
Rendering application with default view <(subclass of Ember.View):ember268>
{% endhighlight %}

### Log Active Generation

When Ember doesn’t find a controller or route definition according to the naming conventions, it generates one instead. Initially this behaviour can create a lot of confusion but with `LOG_ACTIVE_GENERATION` enabled,

{% highlight js %}
var App = Ember.Application.create({
  LOG_ACTIVE_GENERATION: true;
});
{% endhighlight %}

this automatic generation of controllers and routes is logged to the console:

{% highlight console %}
generated -> controller:person Object {fullName: "controller:person"}
{% endhighlight %}


{% highlight console %}
generated -> route:person.index
{% endhighlight %}

### Conclusion

With each of these logging settings enabled you get a much better feel for some of Ember’s ‘magic’, and you can begin to get a greater sense of how everything fits together. &#9632; 
