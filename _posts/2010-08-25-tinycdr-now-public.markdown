--- 
layout: post
title: TinyCDR now public
---

A small internal project we'd used for raw storage of FreeSWITCH CDR data has
now been resurrected.

This iteration adds the logging of the full XML posted by FreeSWITCH into a
couchdb data source, with a link to the id of the couch record stored in the
postgres (or other Sequel-supported database) record.

We'll be adding reporting over the next week or so, time for ideas is now.
Check [our github project](http://github.com/rubyists/tiny_cdr) for more info.
