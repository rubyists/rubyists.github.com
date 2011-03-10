--- 
layout: post
title: Queuetastic released!
typo_id: 18
---
 Looking for a way to monitor Asterisk queues in real-time and have queue reporting available? 
 Without shelling out licensing costs?

Welcome to <b>Queuetastic</b>! 
An Asterisk Queue Real-time Monitoring, Reporting, and Analysis tool designed with Ruby on Rails.

Queuetastic primarily performs 3 major functions.
 * Monitor Asterisk Queues in real-time using AJAX through the Asterisk Manager Interface
 * Migrate and load Asterisk Queue data into a database of your choosing
 * Extract queue and agent reports

Why Queuetastic?
 1. <a href="http://www.asteriskguru.com/tools/queue_stats.php">Asterisk Guru's Queue Stats</a> simply would not install in my environment.  It couldn't parse my queue_log properly.  The source code was closed too.  Boo, Next please.....
 2. <a href="http://sourceforge.net/projects/astacd-activity/"> Asterisk Queue Activity</a> did install, but didn't have the features I was looking for.  Plus it's web views are in Spanish and I don't speak Spanish.
 3. I did <b>not</b> want to shell out for an expensive commercial license to monitor and report on an open-source PBX.  No, Queuemetrics "free 2 Agent license" wouldn't cut it either.  
 4. There should be an open-source application for this!
 5. So I decided to write my own web-app using Ruby on Rails

#####Screenshots#####

* The Dashboard
<img src="/files/dashboard.png" alt="Dashboard" class="left" size="100" />

* Live View
<img src="/files/live_view.png" alt="Live View" class="left" size="100" />

#####Download Queuetastic#####

To download a tarball release:
* <a href="http://sourceforge.net/projects/queuetastic/"> Queuetastic @ sourceforge.net</a>
<br />
<br />

To download from upstream using git:
<br />
{% highlight text %}git clone git://rubyists.com/gits/queuetastic queuetastic{% endhighlight %}
<br />
or
<br />
{% highlight text %}git clone git://github.com/thedonvaughn/queuetastic.git queuetastic{% endhighlight %}
<br />

#####Wiki######
<br/>
For installation and support check out <a href="http://trac.rubyists.com/trac.fcgi/queuetastic/wiki/queuetastic/">Queuetastic's Wiki</a>
