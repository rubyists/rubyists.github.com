--- 
layout: post
title: Ruby loves FreeSWITCH!
typo_id: 26
---
### Ruby Freeswitch Application Programming Interface (ESL Alternative)

[freeswitcher](http://code.rubyists.com/fs) (FSR) is a ruby library for communicating with FreeSWITCH
through its event socket interface.  FreeSWITCH ruby integration
in the core just doesn't work with the thread models of either 1.8
or 1.9. On the other hand; FSR, built on ruby eventmachine, can manage
more than one instance of FreeSWITCH at a time.  

#### Using the freeswitcher gem, 2 minute tutorial
This program will play a smooth wav file and read the DTMF
tones entered by the caller.  It simply logs the received digits
to the info facility (you should see it on stdout)
 
<typo:code lang="ruby">
require "rubygems"
require "fsr"
require "fsr/outbound"
class GetDigits < FSR::Listener::Outbound
  def session_initiated
    exten = session.headers[:caller_caller_id]
    answer do
      read("/path/to/smooth.wav") do |read_var|
        FSR::Log.info("Received #{read_var} from #{exten}")
      end
    end
  end
end
FSR.start_oes! GetDigits, :port => 8084, :host => "127.0.0.1"
</typo:code>

In order to use the above script, add the following in your FreeSWITCH dialplan.  You can save it as
00_socket_test.xml in conf/dialplan/default/ if you're using the stock FreeSWITCH configuration.
<typo:code lang="xml">
<include>
  <extension name="8084">
    <condition field="destination_number" expression="^8084$">
      <action application="set" data="continue_on_fail=true" /> <!-- we still need this to continue if bridging times out -->
      <action application="set" data="call_timeout=5" />
      <action application="socket" data="127.0.0.1:8084 sync full"/>
    </condition>
  </extension>
</include>
</typo:code>

Then reloadxml on FreeSWITCH, make sure the GetDigits listener is started, and call 8084 on a phone registered to FreeSWITCH.

This is only the Outbound Event Socket handler for FreeSWITCH in ruby.  Other interfaces freeswitcher includes
are the Inbound Event Socket handler and a CommandSocket, which allows any api or bgapi command.  See the [docs](http://doc.rubyists.com/fsr) for the full scoop.
