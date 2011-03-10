--- 
layout: post
title: Postgresql in memory for ruby specs with bacon and sequel
typo_id: 29
---
**The Problem**
In the past with BDD and writing specifications for ruby applications which used postgresql, I'd resorted
to environment variables specifying some environment, which I'd then place a conditional somewhere so if the
environment was "test" it would use that already-existing database server and database.  The permissions problems
with this were numerous, so we eventually moved to keeping an instance of postgres up that was just a test server for these test databases.  Maintenance was not easy.  

Other tactics we've used, especially when starting a project or on one which does not use postgres-specific code, we'd use Sqlite3 databases in-memory for our specifications.  This led, at times, to inconsistencies with how postgres would handle data or functions, and again became a maintenance nightmare as soon as we wanted to put the power of postgres to work and Sqlite3 could not support the specs anymore.

On a small project using Ramaze, Sequel, and Bacon for specs, manveru and I came across a solution which, while not as simple as Sqlite in memory, accomplishes the same encapsulation with none of the security risks of the postgres test server or test-database on a multi-purpose server scenario.  

**The Solution**
Create a database in a ram filesystem (/dev/shm mounted as tmpfs on linux), initialize a postgres cluster there,
start the db server, and the user who does so has God rights to create/drop/manipulate the db without chance of affecting anyone else.  The db_helper.rb we use to accomplish this follows

{% highlight ruby %}
begin
  require "sequel"
rescue LoadError
  require "rubygems"
  require "sequel"
end
require "logger"
ENV["PGHOST"] = PGHOST = "/tmp"
ENV["PGPORT"] = PGPORT = "5433"
SHM = "/dev/shm"
ENV['PGDATA'] = PGDATA = "#{SHM}/fxc"
DB_LOG = Logger.new("/tmp/fxc_spec.log")

def runcmd(command)
  IO.popen(command) do |sout|
    out = sout.read.strip
    out.each_line { |l| DB_LOG.info(l) }
  end
  $? == 0 ? true : false
end

def startdb
  return true if runcmd %{pg_ctl status -o "-k /tmp"}
  DB_LOG.info "Starting DB"
  runcmd %{pg_ctl start -w -o "-k /tmp" -l /tmp/fxcdb.log}
end

def stopdb
  DB_LOG.info "Stopping DB"
  if runcmd %{pg_ctl status -o "-k /tmp"}
    runcmd %{pg_ctl stop -w -o "-k /tmp"}
  else
    true
  end
end

def initdb
  raise "#{SHM} not found!" unless File.directory?(SHM)
  return true if File.directory?(PGDATA)
  runcmd %{initdb}
end

def createdb
  runcmd %{dropdb fxc}
  runcmd %{createdb fxc}
end

raise "initdb failed" unless initdb
raise "startdb failed" unless startdb
raise "createdb failed" unless createdb

DB = Sequel.postgres("fxc", :user => ENV["USER"], :host => PGHOST, :port => PGPORT)
require 'sequel/extensions/migration'
require File.expand_path('../../lib/fxc', __FILE__)

# go to latest migration
Sequel::Migrator.apply(DB, FXC::MIGRATION_ROOT)

require FXC::SPEC_HELPER_PATH/:helper
{% endhighlight %}

**The Result**
Running our specs is now much faster, especially after the first run which initializes the cluster.

{% highlight text %}
## Before we run, stop the test db and wipe it out of memory
bougyman@zero:~$ pg_ctl -o "-k /tmp/" stop
waiting for server to shut down.... done
server stopped
bougyman@zero:~$ rm -rf /dev/shm/fxc/
## Run the specs (it's initting a whole new db in ram on this run)
bougyman@zero:~$ time rake
(in /home/bougyman/git_checkouts/fxc)
   1/4: spec/model/target.rb                  1 passed
   2/4: spec/model/user.rb                    1 passed
   3/4: spec/view/dialplan.rb                 1 passed
   4/4: spec/view/directory.rb                3 passed
6 specifications (29 requirements), 0 failures, 0 errors

real    0m8.176s
user    0m0.200s
sys     0m0.056s
## Run the specs again (db is initialized already here, just run specs)
bougyman@zero:~$ time rake
(in /home/bougyman/git_checkouts/fxc)
   1/4: spec/model/target.rb                  1 passed
   2/4: spec/model/user.rb                    1 passed
   3/4: spec/view/dialplan.rb                 1 passed
   4/4: spec/view/directory.rb                3 passed
6 specifications (29 requirements), 0 failures, 0 errors

real    0m2.417s
user    0m0.216s
sys     0m0.044s
## Much faster on runs after the db is initialized
{% endhighlight %}
