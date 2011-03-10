--- 
layout: post
title: More FreeSWITCH ruby love - FXC on the way
---

With [FSR](http://code.rubyists.com/projects/fs) well under way and being used both internally and in the wild for FreeSWITCH application development, the next logical step will be a Configurator for FreeSWITCH itself.

FreeSWITCH has long lacked the standard Administrator/User configuration available via web or GUI, FXC is intended to allow web interfaces for configuration to be built easily with any Rack application utilizing the FreeSWITCH `xml_curl` interface.

The first pass of FXC will include some Rack middleware which turns FreeSWITCH requests (to `/`) into easy to organize routes such as `/directory/register/internal/1000`, `/dialplan/public/8885551212`, `/configuration/acl.conf`.

The goal is to eliminate the gruntwork of routing by POST variables, exposing clean Rack-app routes up the stack (for ramaze, sinatra, etc).

The following is an example with Ramaze.

### middleware.rb

{% highlight ruby %}
module FXC
  module Rack
    class Middleware
      def initialize(app)
        @app = app
      end

      def call(env)
        r = ::Rack::Request.new(env)
        return @app.call(env) unless r.params["section"]
        path = r.params["section"] + "/"
        path << case path
        when "dialplan/"
          dp_req(env, r)
        when "directory/"
          dir_req(env, r)
        when "configuration/"
          conf_req(env, r)
        end
        env["PATH_INFO"] << (env["PATH_INFO"].match(%r{/$}) ? path : "/#{path}")
        @app.call(env)
      end

      private
      def dp_req(env, r)
        s = [r.params["Caller-Context"]]
        s << r.params["Caller-Destination-Number"]
        s.join("/")
      end

      def dir_req(env, r)
        s = []
        if r.params["purpose"]
          s << r.params["purpose"].gsub("-","_")
          s << r.params["sip_profile"]
        elsif r.params["action"] and r.params["action"] == "sip_auth"
          s << "register"
          s << r.params["sip_profile"]
          s << r.params["sip_auth_username"]
        elsif r.params["user"]
          s << "voicemail"
          s << r.params["sip_profile"]
          s << r.params["user"]
        end
        s.join("/")
      end

      def conf_req(env, r)
        s = []
        if r.params["key_name"] == "name"
          s << r.params["key_value"]
        end
        s.join("/")
      end

    end
  end
end
{% endhighlight %}

### controller/dialplan.rb

{% highlight ruby %}
module FXC
  class Dialplan < Controller
    map '/dialplan'
    layout :dialplan

    def index(*args)
      Ramaze::Log.info("Got unhandled dialplan request: #{request.inspect}")
      not_found
    end

    def default(number)
      Ramaze::Log.info("got default dialplan request for #{number}")
      not_found
    end

    def public(number)
      @did = FXC::Did.first(:number.like /#{number.sub(/^1/,'1?')}/)
      if @did 
        @user = @did.user
        @targets = @did.targets
        Ramaze::Log.info("Routing #{@did.number} to #{@user.dialstring}")
        render_view(:index)
      else
        Ramaze::Log.info("Got public dialplan request for #{number}, but no DID matches: ")
        not_found
      end
    end
  end
end
{% endhighlight %}

In the FXC::Dialplan controller, each method represents a FreeSWICH [dialplan context](http://wiki.freeswitch.org/wiki/Dialplan_XML#Context).
Undefined contexts (in this ramaze controller) will fallthrough to the index method and be logged.  

Finally, the FreeSWITCH configuration to send requests to above app becomes a single line/single url

<strong><em>conf/autoload_configs/xml_curl.conf.xml</em></strong>

{% highlight xml %}
<configuration name="xml_curl.conf" description="cURL XML Gateway">
  <bindings>
    <binding name="fxc">
      <param name="gateway-url" value="http://127.0.0.1:9292/" bindings="configuration|directory|dialplan"/>
    </binding>
  </bindings>
</configuration>
{% endhighlight %}

Next step is completing all the methods available for each binding type (configuration, dialplan, directory).  Once complete FreeSWITCH web config on Rack should follow rapidly.  That will be the "Look Ma, No XML" FXC release (TBA). 

