--- 
layout: post
title: Ruby-FCGI on 1.9.x
typo_id: 30
---

While working on a project that required Ruby 1.9, I discovered that the FCGI
library had not been ported yet to 1.9.  So, I did some googling, and found
what sort of changes needed to be made to native extensions to make it work.
The result is below in a patch applied to `fcgi.c` of version 0.8.7 from the
Rubygem. If I can get time, I'll post it on github and gem it up for easier
installation.

{% highlight diff %}
225c225
<   len = FCGX_PutStr(RSTRING(str)->ptr, RSTRING(str)->len, stream);
---
>   len = FCGX_PutStr(RSTRING_PTR(str), RSTRING_LEN(str), stream);
274,275c274,275
<   for (i=0; i<RARRAY(ary)->len; i++) {
<     tmp = RARRAY(ary)->ptr[i];
---
>   for (i=0; i<RARRAY_LEN(ary); i++) {
>     tmp = RARRAY_PTR(ary)[i];
308c308
<     if (RSTRING(line)->ptr[RSTRING(line)->len-1] != '
') {
---
>     if (RSTRING_PTR(line)[RSTRING_LEN(line)-1] != '
') {
382c382
<   if (RSTRING(str)->len > 0)
---
>   if (RSTRING_LEN(str) > 0)
{% endhighlight %}

You can also grab this as a [diff from Pastr](ttp://pastr.it/16176/diff).
