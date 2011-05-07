--- 
layout: post
title: Runit for Ruby (And Everything Else)
---
The [Runit](http://smarden.org/runit) system is a process supervisor and set of utilities surrounding supervising processes.

It can be used as a sysvinit replacement or run side-by-side with many init systems to maintain supervised process heirarchies.

We began using runit as a replacement for daemontools, both because of the licensing at the time and for the extensive documentation offerred for runit utilities (including manpages).

Fully implmenting runit, as a replacement for init/PID 1 or as a standalone supervisor, is covered in general at the runit page above and in detail for [Arch Linux](https://wiki.archlinux.org/index.php/Runit).

Some things we especially like about this system are:

### Standalone environments 

[chpst](http://smarden.org/runit/chpst.8.html) allows multiple controls around processes we run, including memory capping,
user privileges, nice levels, lock files, open files, data segments, cores, stdin/out, and all environment variables.

### Service View
Concise output of process status, when using runit as an init replacement. 

    bougyman@jimmy:~$ sudo sv s /service/\*
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

Look Mom, no pidfiles!

  As you can see, all services live in one place. The services in /service/\* are symlinks to directories (usually in /etc/sv/) which must contain one executable file, named 'run'.

  This 'run' file should start the process in the foreground with stderr redirected to stdout.

#### Example Service (sshd)

/etc/sv/sshd/run:

    #!/bin/sh
    exec /usr/sbin/sshd -De 2>&1

#### Optional Log Service (for sshd)

/etc/sv/sshd/log/run:

    #!/bin/sh
    exec svlogd -t /var/log/sshd/

### Svlogd Logging 

Runit includes a superb lightweight logging system in [svlogd](http://smarden.org/runit/svlogd.8.html).
When you supervise a process, the optional log service's stdout is directed to svlogd (or any other logger which accepts stdin).
Svlogd offers granular control of how to log that output, including rotation on many metrics (without stopping the process it's logging),
post-processing of logs, networked logging (both standard syslog style and offers its own network option), notifications, filtering, and more.

* Process respawning

  if a process stops, it will be started again immediately without intervention
  or outside process monitoring.

* Flexible dependency system

  the [sv](http://smarden.org/runit/sv.8.html) program (with the check
  subcommand) allows any dependency tree you can dream of (and script).

* Parallel startup

  For services which have no dependencies, they all start concurrently,
  drasticly reducing time spent changing runlevels (including the initial
  boot).

* Infinite runlevels

  You are not limited to `0`-`7`, and nothing is reserved.
  Runlevels become (unlimited amount of) directories of services (in
  `/etc/runit/runsvdir`) which can be switched to quickly and simply.

* No mysterious backgrounding

  Processes run in the foreground logging to stdout/stderr. This requirement
  for well-behaved processes can be a blessing but some may see it as a curse;
  however, processes that are not well behaved can be supported through a
  `once` service, if you absolutely cannot do without them.

 * Process respawning, if a process stops, it will be started again immediately without intervention or outside process monitoring.
 * Flexible dependency system, the [sv](http://smarden.org/runit/sv.8.html) program (with the check subcommand) allows any dependency tree you can dream of (and script).
 * Parallel startup, for services which have no dependencies, they all start concurrently, drasticly reducing time spent changing runlevels (including the initial boot).
 * Infinite runlevels, you are not limited to 0-7, and nothing is reserved.  Runlevels become (unlimited amount of) directories of services (in /etc/runit/runsvdir) which can be switched to quickly and simply.
 * No mysterious backgrounding, processes run in the foreground logging to stdout/stderr.  This requirement for well-behaved processes can be a blessing but some may see it as a curse; however, processes that are not well behaved can be supported through a 'once' service, if you absolutely cannot do without them.

