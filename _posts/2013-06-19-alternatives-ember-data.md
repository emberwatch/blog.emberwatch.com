---
layout: post
title:  Alternatives to Ember Data
date:   2013-06-19 20:45:00
author: Philip Poots
tags: emberdata
---

When considering whether or not to use Ember.js, some stumble because Ember Data isn’t considered production ready. This is a little unfortunate given that you can use [Ember without Ember Data][nodata] by communicating directly with the server via AJAX.

However, for those who want want more than jQuery’s `$.ajax()`, here are five alternatives worth checking out as a stop gap or replacement.

### Ember-Model

Ember-Model is a lightweight model library extracted from a real application by Ember.js core-team member [Erik Bryn][erikbryn]. It supports a limited feature set in favour of speed and simplicity.

<p class="code-caption"><span>model_store.js</span></p>
{% highlight js %}
App.User = Ember.Model.extend({
  id: Ember.attr(),
  name: Ember.attr()
});

App.User.url = "/users";
App.User.adapter = Ember.RESTAdapter.create();

// Create and save a new user
var newUser = App.user.create({name: "Erik"});
newUser.save(); // POST /users.json

// Fetch an existing user
var existingUser = App.User.find(1); // GET /users/1.json

// Update an existing user
existingUser.set('name', 'Kris');
existingUser.save() // PUT /users/1.json
{% endhighlight %}

Erik has produced a helpful [screencast covering Ember-Model][emcast] in more detail. Check out [Ember-Model on GitHub][embermodel].

### Ember-Resource

Ember-Resource has come out of Zendesk, the customer support company. It was most likely extracted from their online help desk, which is one of Ember’s earliest production applications.

<p class="code-caption"><span>resource_store.js</span></p>
{% highlight js %}
App.User = Ember.Resource.define({
  url: '/users',
  schema: {
    id:    Number,
    name:  String
  }
});

// Create and save a new user
var newUser = App.User.create({name: "James"});
newUser.save(); // POST /users.json

// Fetch an existing user
existingUser = App.User.create({id: 1}).fetch(); // GET /users/1.json

// Update an existing user
existingUser.set('name', 'Nathan');
existingUser.save(); // PUT /users/1.json

// Destroy an existing user
existingUser.destroy(); // DELETE /users/1.json
{% endhighlight %}

Grab a hold of [Ember-Resource on Github][emberresource].

### Ember-RESTless

A library with a lightweight focus on the simple features of Ember Data is Ember-RESTless.

<p class="code-caption"><span>restless_store.js</span></p>
{% highlight javascript %}
App.RESTAdapter = RL.RESTAdapter.create({
  url: 'http://api.example.com'
});

App.Client = RL.Client.create({
  adapter: App.RESTAdapter
});

App.User = RL.Model.extend({
  name: RL.attr('string')
});

// Create and save a new user
var newUser = App.User.create({name: "Garth"});
newUser.saveRecord(); // POST /users.json

// Fetch an existing user
var existingUser = App.Post.find(1); // GET /users/1.json

// Update an existing user
existingUser.set('name', 'Gopal');
existingUser.saveRecord(); // PUT /users/1.json

// Destroy an existing user
existingUser.deleteRecord(); // DELETE /users/1.json
{% endhighlight %}

Read more about [Ember-RESTless on GitHub][emberrestless].

### Emu

Emu is a simple data access library for Ember.js

<p class="code-caption"><span>emu_store.js</span></p>
{% highlight javascript %}
App.Store = Emu.Store.extend({
  revision: 1
});

App.User = Emu.Model.extend({
  name: Emu.field("string")
});

// Create and save a new user
var newUser = App.User.createRecord({name: "Charlie"});
newUser.save(); // POST /users.json

// Fetch an existing user
existingUser = App.User.find(1); // GET /users/1.json

// Update an existing user
existingUser.set('name', 'Jonnii');
existingUser.save(); // PUT /users/1.json

// Destroy an existing user
App.User.deleteRecord(existingUser); // DELETE /users/1.json
{% endhighlight %}

See [Emu on GitHub][emu].

### Ember-REST

Creator Dan Gebhardt brands Ember-REST as a very simple library for RESTful resources that he extracted from an example app. 

<p class="code-caption"><span>rest_store.js</span></p>
{% highlight javascript %}
App.User = Ember.Resource.extend({
  resourceUrl: '/users',
  resourceName: 'user',
  resourceProperties: ['name']
});

// Create and save a new user
var newUser = App.User.create({name: "Dan"});
newUser.saveResource(); // POST /users.json

// Fetch an existing user
existingUser = App.User.create({id: 1}).findResource(); // GET /users/1.json

// Update an existing user
existingUser.set('name', 'Jeremy');
existingUser.saveResource(); // PUT /users/1.json

// Destroy an existing user
existingUser.destroy(); // DELETE /users/1.json
{% endhighlight %}

Take a look at [Ember-REST on GitHub][emberrest]

### Conclusion

Although Ember Data is currently only being recommended to those who are comfortable with the cutting edge, these libraries provide viable alternatives. I haven’t gone into much detail save for the basicsbut each offers a unique (though similar) approach and a variety of different features which merit further investigation. &#9632;

[nodata]: http://eviltrout.com/2013/03/23/ember-without-data.html
[embermodel]: https://github.com/ebryn/ember-model
[emcast]: http://www.embercasts.com/episodes/getting-started-with-ember-model
[emberresource]: https://github.com/zendesk/ember-resource
[emberrest]: https://github.com/cerebris/ember-rest
[emberrestless]: https://github.com/endlessinc/ember-restless
[emu]: https://github.com/charlieridley/emu
[erikbryn]: http://erikbryn.com/
