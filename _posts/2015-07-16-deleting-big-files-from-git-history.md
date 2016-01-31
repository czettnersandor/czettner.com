---
layout: post
title: Deleting big files from git history
created: 1437070611
comments: true
---
I warned you: this article is about operations that will overwrite git history. Be careful what you're doing and backup your repository.

Sometimes big files are getting in the way to the git repository. This could happen because of a careless commit or the file originally was very small, but grew by time: for example a log file. Whatever the reason, you might want to get rid of it once and for all because it's slowing down others to clone the repository and uses too much space on the git server.

Deleting a file from history takes a lot of time. On big repositories with large binary blobs, it could be a memory hungry overnight process. To delete dev/database.sql from the history:

{% highlight html %}
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch dev/database.sql" --prune-empty --tag-name-filter cat -- --all
{% endhighlight %}

Optionally, to save space locally, otherwise you have to wait 90 days until git clears the garbage out from the reflog:

{% highlight html %}
git reflog expire --expire=now --all
git gc --prune=now
{% endhighlight %}

Then force push it to the remote repository:

{% highlight html %}
git push origin --force --all
git push origin --force --tags
{% endhighlight %}

There is a backup folder git creates which you can now delete:

{% highlight html %}
rm -rf .git/refs/original/
{% endhighlight %}

After the remote origin is up to date, you have to tell the other developers to get the changes, they should run:

{% highlight html %}
git fetch
git reset origin/master --hard
{% endhighlight %}
