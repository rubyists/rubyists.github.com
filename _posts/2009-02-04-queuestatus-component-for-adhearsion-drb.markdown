--- 
layout: post
title: QueueStatus component for Adhearsion DRb
typo_id: 24
---
One of my favorite things about the newest release of [Adhearsion](http://adhearsion.com) is it’s [component system](http://docs.adhearsion.com/display/adhearsion/Components).

The component system is designed to provide an easy interface to developing plug-ins for Adhearsion that may be easily shared among the Adhearsion community.

I recently had to create a new component for my app, [Queue-Tip](http://queue-tip.rubyforge.org). This component takes the result of the AMI command QueueStatus (ManagerInterface#send_action ‘QueueStatus’) via DRb and sorts it into a hash so that it can easily be used pragmatically.

Click [here to read more](http://jaysonvaughn.wordpress.com/2009/02/04/queuestatus-component-for-adhearsion/)
