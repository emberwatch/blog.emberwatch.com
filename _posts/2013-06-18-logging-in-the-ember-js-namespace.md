---
layout: post
title:  Logging in the Ember Namespace
date:   2013-06-18 21:01:17
author: Philip Poots
tags: logging debugging ember.js
---

Ember.js has three built in logging facilities defined in the `Ember` namespace. These are in addition to those contained in the `Ember.Application` namespace that we explored in my last post, [Logging the Magic in Ember.js][magic].

### Log Version

The first is `LOG_VERSION`, which is set to appear once on page load.

{% highlight js %}
Ember.LOG_VERSION = true;
{% endhighlight %}

It produces helpful information about the version of Ember.js and its dependencies:

{% highlight console %}
DEBUG: -------------------------------
DEBUG: Ember.VERSION : 1.0.0-rc.5
DEBUG: Handlebars.VERSION : 1.0.0-rc.4
DEBUG: jQuery.VERSION : 2.0.0
DEBUG: -------------------------------
{% endhighlight %}

### Log Bindings

You will get most mileage out of `LOG_BINDINGS` given how integral bindings are to Ember applications.

{% highlight js %}
Ember.LOG_BINDINGS: true;
{% endhighlight %}

Now, when a property which is bound to another is changed, Ember triggers a log message to the console with relevant details.

Consider this simple example concerning a proxy object, which property `appName` is bound to the `name` property of our Ember application `App`.

<p class="code-caption"><span>app.js</span></p>
{% highlight js %}
var App = Ember.Application.create({
  name: "EmberWatch"
});

App.proxy = Ember.Object.create({
  appNameBinding: 'App.name'
});
{% endhighlight %}

Changing either value provokes a log message. Notice how Ember includes the data flow direction, indicating which property changed to fire the corresponding update.

{% highlight console %}
> App.set('name', "EmberWatch Blog");
Ember.Binding<ember344>(App.name -> appName) -> EmberWatch Blog
> App.proxy.set('appName', "EmberWatch");
Ember.Binding<ember344>(App.name -> appName) <- EmberWatch
{% endhighlight %}

### Log Stack Trace on Deprecation

Ember logs a deprecation warning to the console each time that you call a method that has been deprecated in the API. This is what it looks like for `Ember.Button` which has been superceded by the `action` Handlebars helper:

{% highlight console %}
DEPRECATION: Ember.Button is deprecated and will be removed from future releases. Consider using the `action` helper.
{% endhighlight %}

With `LOG_STACKTRACE_ON_DEPRECATION` enabled:

{% highlight js %}
Ember.LOG_STACKTRACE_ON_DEPRECATION: true;
{% endhighlight %}

Ember now prints the stack trace with the warning:

{% highlight console %}
DEPRECATION: Ember.Button is deprecated and will be removed from future releases. Consider using the `action` helper. at Ember.Button.Ember.View.extend.init 
(http://localhost:4000/assets/ember.js:22412:11) at superWrapper [as init] 
(http://localhost:4000/assets/ember.js:1060:16) at new Class 
(http://localhost:4000/assets/ember.js:11298:15) at Function.Mixin.create.create [as create] 
(http://localhost:4000/assets/ember.js:11597:12) at Ember.View.Ember.CoreView.extend.createChildView 
(http://localhost:4000/assets/ember.js:16950:19) at Object.Ember.merge.appendChild 
(http://localhost:4000/assets/ember.js:17452:22) at Ember.View.Ember.CoreView.extend.appendChild 
(http://localhost:4000/assets/ember.js:16830:30) at EmberHandlebars.ViewHelper.Ember.Object.create.helper 
(http://localhost:4000/assets/ember.js:20958:17) at get 
(http://localhost:4000/assets/ember.js:21136:37)
{% endhighlight %}

### Conclusion

The in-built logging capabilities in the `Ember` namespace might not be as useful as those in `Ember.Application` but it is helpful to be aware of them. The `LOG_BINDINGS` setting will end up being the most useful of the three. &#9632;

[magic]: http://blog.emberwatch.com/2013/06/13/logging-the-magic-in-ember-js.html
