--- 
layout: post
title: Quick ruby one liner to reboot phones attached to freeswitch
typo_id: 38
---
Needed this to reboot all my extensions from 1000-1009 every night.  Adjust as needed.

{% highlight ruby %}
irb(main):039:0> (Nokogiri(%x{fs_cli -x 'sofia xmlstatus profile internal'})/:registration).select{|x| (x/"sip-auth-user").text.to_i < 1010 }.map { |n| (n/"call-id").text }.each { |p| reboot(p) }
Sending reboot to 3c2671257674-tlf1pz01hu9g
Sending reboot to 3c26700e5573-7zw9k9793uer@snom320-0004132CC8C8

{% endhighlight %}

