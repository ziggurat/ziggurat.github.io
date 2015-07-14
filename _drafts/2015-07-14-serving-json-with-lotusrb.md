---
title:  "Building a JSON API with lotusrb"
date:   2015-07-14
categories: lotusrb jsonapi-serializer
categories: lotusrb jsonapi-serializer
---

**Motivation**

I have a project I use to try frameworks and technologies. The project is a sports forecasting web app to play with friends, nothing fancy. My first implementation was using Rails. It was many years ago, it was ugly but worked. I deployed that thing to Heroku and we could use it to play for a couple competitions.

After that, I discovered the Single Page Applications worlds and fell in love with Ember.js. Being a JS newbie, it took me a while to grasp the new concepts, buy finally I implemented a JSON REST API using Sinatra along with Sequel. On the client side of the API, I built an Ember SPA. Again deployed, but this time in openshift, the web application was much better designed and I liked the SPA with a JSON API a lot.
It was time to rewrite the application again. I wanted to give lotusrb a try for a while now, so I decided to implement the REST API using it and keep the current Ember app in the client side.

This is the first post, with the initial problems I found. By all means this do not pretend to be a lotus tutorial, but a guide to setup a lotusrb project to serve a REST API serving JSON.

**Creating the app**

Lets create the lotus application
{% highlight bash %}
  lotus new bookshelf
{% endhighlight %}

If you are a little familiarized with lotus, you know that you have an action, a view and a template. To serve a JSON response we only need the action. We can create only what we need (the route and the action) or we can use a lotus generator and remove the things we don't need. Let's use the first approach:

Add the route to the routes.rb file:
{% highlight ruby %}
# apps/web/config/routes.rb
get '/books', to: 'books#index'
{% endhighlight %}

The next step is to create an action. As documented here: http://lotusrb.org/guides/actions/basic-usage/, lotus allows to bypass the rendering so we can build a response directly from the action and remove the view and template all togheter if we don't need it. For that, we have to assign the self.body. When we set the body of the response in an action, the application will ignore the view.

{% highlight ruby %}
# apps/web/controllers/books/index.rb
module Web::Controllers::Books
  class Index
    include Web::Action

    def call(params)
      self.body = raw "{'data': 'Hello world!'}"
    end
  end
end
{% endhighlight %}
