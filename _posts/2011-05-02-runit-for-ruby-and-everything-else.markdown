--- 
layout: post
title: Runit for Ruby (And Everything Else)
author: TJ Vanderpoel
---
The [Runit](http://smarden.org/runit) system is a process supervisor and set of utilities surrounding supervising processes.

It can be used as a sysvinit replacement or run side-by-side with many init systems to maintain supervised process heirarchies.

We began using runit as a replacement for daemontools in November 2001, because runit was OSI licensed at the time (daemontools was not) and for the extensive documentation offerred for runit utilities (including manpages).

Fully implmenting runit, as a replacement for init/PID 1 or as a standalone supervisor, is covered in general at the runit page above and in detail for [Arch Linux](https://wiki.archlinux.org/index.php/Runit).

Some things we especially like about this system are:

### Service View

Runit offers a concise but complete view of process status, including the optional log service. 

        bougyman@jimmy:~$ sudo sv s /service/*
        run: /service/callcenter: (pid 2870) 5266009s
        run: /service/cron: (pid 3769) 7700115s
        run: /service/fxc: (pid 3714) 7700118s
        run: /service/getty-1: (pid 3713) 7700118s
        run: /service/getty-2: (pid 3730) 7700117s
        run: /service/getty-3: (pid 3716) 7700118s
        run: /service/getty-4: (pid 3690) 7700118s
        run: /service/getty-5: (pid 3691) 7700118s
        run: /service/helpdesk: (pid 3715) 7700118s
        run: /service/lighttpd: (pid 27321) 5208602s; run: log: (pid 3724) 7700117s
        run: /service/openntpd: (pid 3747) 7700116s; run: log: (pid 3741) 7700117s
        run: /service/postgres: (pid 3732) 7700117s
        run: /service/redmine: (pid 3729) 7700117s
        run: /service/socklog-klog: (pid 3746) 7700116s; run: log: (pid 3733) 7700117s
        run: /service/socklog-unix: (pid 3758) 7700116s; run: log: (pid 3756) 7700116s
        run: /service/ssh: (pid 3757) 7700116s; run: log: (pid 3731) 7700117s  


As you can see, all services live in one place, defined by `$SVDIR` or `/service` by default, which [`runsvdir`](http://smarden.org/runit/runsvdir.8.html) manages.  
The services in `/service/*` are symlinks to directories (usually in `/etc/sv/`) which must contain one executable file, named 'run'.

The `run` executable should exec the process in the foreground with stderr redirected to stdout.

Runit avoids unecessary complexity by separating the roles into small, dietlibc-friendly utilities, written in C without library dependencies.  
The above listing represents many individual services, each with one instance of the per-process supervisor [`runsv`](http://smarden.org/runit/runsv.8.html) running.

##### Example Service (sshd)

`/etc/sv/sshd/run`:

            #!/bin/sh
            exec /usr/sbin/sshd -De 2>&1

If the directory `log/` exists, it will be treated as a log service. 

runsv will create a pipe and redirect standard out of the service (and its optional `finish` script) to `log/run`.

##### Log Service (for sshd)

`/etc/sv/sshd/log/run`:

        #!/bin/sh
        exec svlogd -t /var/log/sshd/

### Svlogd Logging 

Runit includes a superb lightweight logging system in [svlogd](http://smarden.org/runit/svlogd.8.html).

When you supervise a process, the optional log service's stdout is directed to svlogd (or any other logger which accepts stdin).

Svlogd offers granular control of how to log that output, including rotation on many metrics (without stopping the process it's logging),
post-processing of logs, networked logging (both standard syslog style and offers its own network option), notifications, filtering, and more.

#### Config file for a log service

`/var/log/sshd/config`:

        s100000
        n10
        N5
        t86400
        u192.168.6.20
        pSSHD_LOG
        -bougyman
        !logwatcher

The above would set the max size of a log file to 100000 bytes, keep 10 files maximum,
5 files minimum (if the disk were full it would delete 6-10 until it had room),
rotate every 86400 seconds (even if the size limit were not exceeded),
network log to 192.168.6.20,
prepend each log message with SSHD\_LOG,
not log messages matching the pattern "bougyman",
and upon rotation run each logfile through `sh -c 'logwatcher'`.

Not very descriptive, but well documented (in `man svlogd`).  

### Process respawning and control

If a process stops, it will be started again immediately without intervention or outside process monitoring.  
`runsv` avoids a failed or faulty process hammering resources by delaying 1 second before starting a process back up if it's failing instantly.

When a process stops, if a file named 'finish' exists and is executable, `finish` will be run with two arguments, the exit code and exit status of `run`.

##### Example `finish` script

`/service/sshd/finish`:

        #!/bin/sh
        code=$1
        status=$2
        hostname=$(hostname)
        if [ $status -ne 0 ];then
          echo "SSHD FAILED on $hostname: $code, $status"|mail admins@domain.com
        else
          echo "SSHD Stopped on $hostname: $code, $status"|mail auditing@domain.com
        fi
  
#### `sv` usage examples

[`sv`](http://smarden.org/runit/sv.8.html) is used to get and change status of a particular service.  This is your swiss army knife of process control.

Get status

        # sv s sshd
        run: sshd: (pid 4040) 6435s; run: log: (pid 4028) 6435s

Send the TERM signal, which will restart a process.  This works because runsv will always keep a process started unless you tell it to stay down.

        # sv t sshd

Generally there will be no output from such commands, use -v to get some output (examples from here on out will use -v)

        # sv -v t sshd
        ok: run: sshd: (pid 26764) 0s

Stop a service and keep it down (until the next boot or you specifically tell it to come back up)

        # sv -v d sshd
        ok: down: sshd: 1s, normally up

Send USR1 to a service

        # sv -v 1 sshd
        ok: down: sshd: 56s, normally up

Remove a service (stop it and make it not start back up, even on boot)
   
        # rm /service/sshd

    Of course there will be no sv output from the above.

Removing a symlink in `/service` (or your `$SVDIR` if not `/service`) will send the equivalent of an `sv d`(own) to `runsv`, stopping the process and its log service, then stopping runsv.

### Flexible dependency system

The [sv](http://smarden.org/runit/sv.8.html) program (with the check subcommand) also allows for any dependency tree you can dream of (and script).

##### Basic dependency example

`/service/lighttpd/run`:

        #!/bin/sh -e
        sv -w7 check postgresql
        exec 2>&1 \
          lighttpd -f /etc/lighttpd/lighttpd.conf -D

This would wait 7 seconds for the postgresql service to be running, exiting with an error if that timout is reached.  
`runsv` will then run this script again.  Lighttpd will never be executed unless sv check exits without an error (postgresql is up).

#### Dependency hierarchies (Per-User service trees)

Building on the above dependency checking, we can go one step further and give a user control over their own `$SVDIR`, `/home/username/service` here.  
This gives non-root users the same ability to guarantee services are running, supervised, properly logged, and all the advantages of the main `$SVDIR`.

##### What's with all the dots?

> The log:............ is a construct runsvdir uses to log to proctitle (seen in ps aux).  Each dot represents 15 seconds of time, with any stderr/out being flushed to proctitle for each dot.  
> Thus if you saw

        root      4001  0.0  0.0    184    28 ?        Ss   08:00   0:00 runsvdir -P /service log: ed with loglevel notice?Could not load host key: /etc/ssh/ssh_host_ecdsa_key??...

You'd know sshd had a problem, but it hasn't had that problem in 45 seconds.

##### User-level supervision example (with dependency checking)

`/service/callcenter/run`:

        #!/bin/sh -e
        sv check postgresql couchdb memcached
        exec 2>&1 \
          sudo -H -u callcenter runsvdir -P /home/callcenter/service 'log:.........................................................................................................'

Now any service directory the callcenter user symlinks in ~/service/ will get its own `runsv` process, running as callcenter.  _None_ of the callcenter user's processes will be started (because `runsvdir` will not run) until postgresql, couchdb, and memcached are up and running successfully.

`/home/callcenter/service/fs2ws/run`:

        #!/bin/sh -e
        envdir=$PWD/env
        root=$(<env/TCC_Root)
        echo Starting from $root
        cd $root
        if [ -s $HOME/.rvm/scripts/rvm ]; then
          source $HOME/.rvm/scripts/rvm
          echo "Using RVM $(which rvm)"
          rvm use 1.9.2 2>&1
        fi
        exec 2>&1 \
          chpst -e $envdir ./bin/fs2ws 2>&1

Finally, a hint of Ruby!  
This script includes a check to see if rvm is installed and if so uses its 1.9.2 version.  
The -e to `chpst` specifies an environment directory, which is a way to set environment variables, but `chpst` can do much more.

### Standalone environments 

[chpst](http://smarden.org/runit/chpst.8.html) allows multiple controls around processes we run, including memory capping,
user privileges, nice levels, lock files, open files, data segments, cores, stdin/out, and all environment variables.  
It is the swiss army knife for manipulating what a running process can do.  
This command encapsulates all of the functionality  of the daemontools commands `setuidgid`, `envuidgid`, `envdir` (used above), `softlimit`, and `setlock`.
If called as one of those commands, it will behave as that utility.  
`chpst` adds the behavior of `pgrphack` and `fghack`, as well as some behavior not available in daemontools such as nice level.

That's a lot of manipulation in one tool, but most of them are rarely used.  
The -e/envdir option is the one we take advantage of most, essentially using it as a replacement for options/config files.  
A directory of files where an enviroment variable will be created for each file, with the value set to the contents of that file, may appear cumbersome at first glance.  
In practice, however, we find that changing one or two options is the most likely workflow.  
With the envdir setup, this becomes

        echo 'sofia/gateway/default/%s' > ~/service/fs2ws/env/TCC_ProxyServerFormatString

Not so cumbersome for that!  It may indeed take longer to set these variables up the first time, but maintenance is not so bad.  
One advantage of having this data in environment variables is the ability to query `/proc/PID/env/` for the options your ruby got on startup.  
The second (more important) one we found is that we make fewer options.  
That's right, it makes us think about each option we add, knowing it will be another file in `env/`.  So we don't have as many options.  Good Thing.

Now our (ruby) process has access to all of these in `ENV["TCC_*"]`, how to use them?.  
See the Tiny Call Center [default envdir](https://github.com/rubyists/tiny_call_center/tree/master/sv/env) and [options.rb](https://github.com/rubyists/tiny_call_center/blob/master/options.rb) for one way to glue them., but rolling your own should be trivial (just look at `ENV` in a `chpst -e ./env irb`).

### Parallel startup

Services all start concurrently, drasticly reducing time spent changing runlevels (including the initial boot).

#### How Fast (Benchmarks!)

The only fair benchmark available for runlevel switching, because of the differences in what arbitrary runlevels mean in various systems, was initial boot time.  
The results of the initial boot time are of course influenced by POST time, which is listed for each of the below reference systems.  Each of these systems is a default Arch Linux install with the Arch Linux default (sysvinit), runit-run-git, or initscripts-systemd installed, running getty's, a syslogger, ssh, and cron (no X).  
Because POST has varied on the systems, the initial boot times and reboot times are an average of 10 runs of each action.  A system is considered 'booted' when a user is able to log in to a getty.

##### Acer 1830t Small Form Factor Notebook

    680U i7 1.47Ghz processor
    4G Ram
    640G 7200RPM Sata HD
    Intel Graphics on board
    Post time 5-7 seconds.
    
<table>
  <thead>
    <tr>
      <th>System</th>
      <th>Initial Boot Time</th>
      <th>Reboot (init 6/shutdown -r now) Time</th>
      <th>Max Boot Time</th>
      <th>Min Boot Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Default SysV Init</td>
      <td>28.73</td>
      <td>39.41</td>
      <td>33.48</td>
      <td>27.10</td>
    </tr>
    <tr>
      <td>SystemD</td>
      <td>19.21</td>
      <td>26.77</td>
      <td>22.41</td>
      <td>18.07</td>
    </tr>
    <tr>
      <td>Runit</td>
      <td>17.25</td>
      <td>24.47</td>
      <td>18.22</td>
      <td>16.33</td>
    </tr>
  </tbody>
</table>

##### Toshiba Qosmio Portable Workstation

    740QM i7 processor
    8G Ram
    Corsair Performance III SSD
    Nvidia GTX460m 1.5G Discreet Graphics
    Post time 2-4 seconds.
    
<table>
  <thead>
    <tr>
      <th>System</th>
      <th>Initial Boot Time</th>
      <th>Reboot (init 6/shutdown -r now) Time</th>
      <th>Max Boot Time</th>
      <th>Min Boot Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Default SysV Init</td>
      <td>22.13</td>
      <td>29.41</td>
      <td>24.83</td>
      <td>19.94</td>
    </tr>
    <tr>
      <td>SystemD</td>
      <td>15.11</td>
      <td>21.53</td>
      <td>16.96</td>
      <td>14.62</td>
    </tr>
    <tr>
      <td>Runit</td>
      <td>11.94</td>
      <td>17.67</td>
      <td>12.45</td>
      <td>11.28</td>
    </tr>
  </tbody>
</table>

The main time difference we saw in the SystemD startup was the 2-3 seconds dbus itself takes to start, giving runit its advantage.  If you started dbus in runit stage 1 we expect these times would be nearly identical.

### Infinite runlevels

You are not limited to `0`-`6`, with only `current` and `previous` as reserved.

Runlevels become (unlimited amount of) directories of services (in `/etc/runit/runsvdir`) which can be switched to quickly and simply.

        # runsvchdir primary # Switch to the services described in `/etc/runit/runsvdir/primary`
        # runsvchdir failover # Switch to the services described in `/etc/runit/runsvdir/failover`

### No mysterious backgrounding

Processes run in the foreground logging to stdout/stderr.  
This requirement for well-behaved processes can be a blessing but some may see it as a curse.  
Processes that are not well behaved can be supported through a `once` service, if you absolutely cannot do without them.  
We use this as a bit of a litmus test for potential new services.  When they can not run in a well-behaved manner that's a high-priority Con, one which takes extraordinary Pros or absolutely necessary business-case to overcome.

