---
layout: post
title: Successful git workflow
created: 1360263524
comments: true
categories: [git]
---
A lot of project I'm working on have only a 1-2 developers involved. For these projects I don’t usually take much advantage of the power of Git. It goes simple. The Larger Project method is preferred over this except it's not a messy Wordpress site. The command I'm usually using are:

{% highlight html %}
git pull
{% endhighlight %}
Make changes on the files xyz
{% highlight html %}
git add x y z
git commit -m "Made changes on file xyz"
git push
{% endhighlight %}

<h3>Larger Project</h3>

I am working with a group of other developers with a lot of tickets being handled all at once. And we have different permissions to push to the master branch depend on the seniority. To minimize the pain (and to keep nice and clean commit logs), I'm using a specified git workflow. It is namely known as the widely used "Feature Branches" and in the example below the master branch is used for internal development and a new feature branch had been created which is exists only on the developer's machine or can be pushed to the git server if interaction is required from other developers or is a longer task and needs a remote backup.

Before any work being started, I performing the following commands:

{% highlight html %}
git checkout master
git pull
{% endhighlight %}

Then I usually find a ticket to work on. Let's say it's "Add holes to the PunchController". I'm creating my own feature branch:

{% highlight html %}
git checkout -b holes_to_punch
{% endhighlight %}

I write a test, write some code, etc. Commit only what I know I want to commit.

{% highlight html %}
git add <files>*
{% endhighlight %}

I write really nice commit messages here or if I'm messing this branch, I can still rebase the whole branch. After the commit, I make sure it's not in conflict with the remote master and do a rebase:

{% highlight html %}
git checkout master
git pull origin master:master
git checkout holes_to_punch
git rebase -i master
{% endhighlight %}

At this point, either it goes well or have merge conflicts. If I have conflicts, I fix them, and keep going. I never ever do a non fast forward merge in the master branch! Every feature or a ticket response is a single (or a few) commit in the master branch.

If there are any code changes, commit, then repeat from checking out master again. The idea is to make sure that when I finally merge holes_to_punch into master, it’s going to be a clean fast-forward merge with no conflicts.

Once the holes_to_punch feature is ready, follow the above again to checkout the master just to make sure nobody committed since you and then merge your feature:

{% highlight html %}
git checkout master
git merge holes_to_punch
git push
{% endhighlight %}

The benefit of using this technology instead of cherry-pick is that the sha sum of the single commits will be the same. This is because cherry-pick is just make a copy of every commit creating a new sha id and applying the code changes again from the original commit. In my way, the commit will be saved and can be referenced, changed, rebased again.

Note I can originate and merge my feature branch from the staging branch. In this case, I will need to merge the staging branch to the master at the end after the QA department has accepted my changes.
