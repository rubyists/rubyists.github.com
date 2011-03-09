--- 
layout: post
title: Queue-Tip 0.9.2 released (formerly Queuetastic)
typo_id: 23
---
**[Queue-Tip](http://queue-tip.rubyforge.net) 0.9.2** released (formerly *Queuetastic*)

**About**

Queue-Tip is an Asterisk Queue Monitoring and reporting tool
using Adhearsion and designed with the Ruby on Rails framework.

Features include:
* Real-time, live view, of Asterisk queues updated seamlessly using Javascript
* Queue stats are collected in real-time using [Adhearsion](http://adhearsion.com)
* Load Asterisk's queue_log file into a database
* Run reports on agent and queue stats.
* Partition agents in to "groups" for reporting needs..
* Export reports in csv format

Changes include:
* Switched to Adhearsion framework to talk to Asterisk's manager interface
* Completely re-vamped reporting models.  Refactored cactions and aactions into aactions
* Changed the UI to a new theme
* Include a generic .htaccess for deploying behind Apache

Download Queue-Tip:
[Queue-Tip at Rubyforge](http://queue-tip.rubyforge.org)
[Queue-Tip at GitHub](http://github.com/thedonvaughn/queue-tip/tree/master)
