---
layout: post
title: Rails 3 Relation object
created: 1319581030
comments: true
categories: [ruby]
---
Normally in a Rails application, you want to cache three things: rendered template snippets, controller logic and model results. Rails has solutions for each one, but it was difficult to balance each tier. Often times, people will cache templates but still let their controller logic run.

With the new Active Record functionality and Relation objects, it makes simple to get a lot of speed out of just changing a little application logic. The trick is that queries are not run until you call it from the view with the .all .each methods on the Relation object, so if you would like to push the cache logic to the view layer, simply use a model query without these methods and use as like this:

In the controller:

<code class="ruby">
def index
  @posts = Post.where(:published => true).limit(20)
end
</code>

In the view:

<code class="ruby">
<% @posts.each do |post| %>
  <%= post.title %>
<% end %>
</code>

In this case, the query will be executed when we call .each. If we wrap that in a cache block, it will be executed only the cache is invalidated.
