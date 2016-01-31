---
layout: post
title: Tmux for managing servers
created: 1419107045
comments: true
---
If you are managing multiple servers through SSH, then you might find an issue when your internet connection is not stable and performing longer tasks in the terminal. For example a Magento full reindex could take 30 minutes depending on the number of products and the websites on the same installation.

When an ssh connection terminates for whatever the reason, GNU/Linux and FreeBSD is terminating the processes whom where opened from that session with a HUP signal. If you are lazy like me, you probably not starting the longer processes with a nohup prefix, so when the bad thing is happening, you'll end up with a broken index. It's even worse when doing a PHP recompile, but terminating with a HUP does not help to finish any kind of job in time.

But Tmux is not only useful on server, I found it very helpful on my desktop and laptop as well. I wrote this article for myself too, so I can come back and read the keybindings if I forget. It's a not easy learning curve as it requires a wee bit different thinking than using the traditional terminal, however, you can use any shell you want, even the most common bash or my favourite: zsh.

Tmux is a program, similar to screen you can run in your terminal. It allows you to have multiple terminal instances running in the one terminal and allows you to connect to your tmux instance from different locations. You can even leave tmux at 5'o clock, then log in to the server in the morning to see the results of the program you ran.

You can also split each terminal window into several panes. This is useful if your normal behaviour is opening multiple terminal windows. You will find the tmux approach faster to handle as you don't have to leave your keyboard to switch to an other window with a mouse.

Currently I'm a Tmux beginner, so here is how I started.

<h3>Starting Tmux</h3>

The default keyboard shortcut to enter a mode which accepts the secondary key is <strong>Ctrl+B</strong>. The man page of Tmux is referring to this key as C-b, but it means <strong>Ctrl+B</strong>, I guess this naming is coming from Emacs. If you install tmux, it will not start automatically. You can even start it by entering <strong>tmux</strong> to your shell, or you can add it to the shell's startup script. I have this one in my .zshrc:

{% highlight html %}
# TMUX
if which tmux 2>&1 >/dev/null; then
   #if not inside a tmux session, and if no session is started, start a new session
   test -z "$TMUX" && (tmux attach || tmux new-session)
fi
{% endhighlight %}

<h3>Detaching from the terminal</h3>

But now if I start more than one terminal window, the second one will be a clone of the first one. If I type something, it will appear on both. This is normal and expected. To exit tmux, just detach it by: <strong>C-b d</strong> which is the Ctrl+B key, followed by pressing d. I'll follow the man page's naming from now on.

<h3>Multiple windows</h3>

To create a new window, use <strong>C-b c</strong>. You will notice that a new tab appeared in tmux's status bar. To switch between these tabs, press <strong>C-b n</strong> or for the previous tab: <strong>C-b p</strong>. To name a tab, so you will remember what was on it: <strong>C-b ,</strong> (comma). It's also possible to switch a tab by it's index: <strong>C-b 0</strong> or <strong>C-b 1</strong> or <strong>C-b 2</strong> ...

Probably the most terrifying thing will be that you are unable to scroll with your terminal's scroll bar or with the mouse wheel. That's because tmux have it's own scroll buffer, independent from the terminal. To enter scroll mode, type <strong>C-b [</strong>. Then you can use the cursor keys or Page Up, Page Down keys to navigate. Vi like navigation keys will also work: "g" will jump to the first line in the buffer, "G" to the last line, "hjkl" to move. Enter will exit the scroll mode.

<h3>Panes</h3>

Pane is the replacement for opening multiple windows. You can simply split a window vertically with <strong>C-b "</strong> or horizontally with <strong>C-b %</strong>. Switching between panes is possible with <strong>C-b</strong> and the arrow keys depending on which direction you want to go with the focus.

<h3>Summary</h3>

Tmux seems to be a very powerful tool 

Was this write up help to you? Did I miss something? Disquss below!
