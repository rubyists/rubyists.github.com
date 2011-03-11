--- 
layout: post
title: Queuetastic released!
typo_id: 18
---

Looking for a way to monitor Asterisk queues in real-time and have queue
reporting available?  Without shelling out licensing costs?

Welcome to **Queuetastic**! 

An Asterisk Queue Real-time Monitoring, Reporting, and Analysis tool designed with Ruby on Rails.

Queuetastic primarily performs 3 major functions.
* Monitor Asterisk Queues in real-time using AJAX through the Asterisk Manager Interface
* Migrate and load Asterisk Queue data into a database of your choosing
* Extract queue and agent reports

Why Queuetastic?

1. [Asterisk Guru's Queue Stats](http://www.asteriskguru.com/tools/queue_stats.php)
   simply would not
   install in my environment.  It couldn't parse my queue_log properly.  The
   source code was closed too.  Boo, Next please.....
2. [Asterisk Queue Activity](http://sourceforge.net/projects/astacd-activity/)
   did install, but didn't have the features I was looking for.  Plus it's web
   views are in Spanish and I don't speak Spanish.
3. I did *not* want to shell out for an expensive commercial license to
   monitor and report on an open-source PBX.  No, Queuemetrics "free 2 Agent
   license" wouldn't cut it either.  
4. There should be an open-source application for this!
5. So I decided to write my own web-app using Ruby on Rails

### Screenshots

#### The Dashboard

![Dashboard](/files/dashboard.png "The Dashboard")

#### Live View

![Live View](/files/live_view.png "Live View")

### Download Queuetastic

To download a tarball release:
* [Queuetastic @ sourceforge.net](http://sourceforge.net/projects/queuetastic/)

To download from upstream using git:

{% highlight bash %}
git clone git://rubyists.com/gits/queuetastic queuetastic
{% endhighlight %}

or

{% highlight bash %}
git clone git://github.com/thedonvaughn/queuetastic.git queuetastic
{% endhighlight %}


#### Wiki

For installation and support check out [Queuetastic's Wiki](http://trac.rubyists.com/trac.fcgi/queuetastic/wiki/queuetastic/).
