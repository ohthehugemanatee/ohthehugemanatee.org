---
categories: null
comments: true
date: "2014-07-22T15:43:30Z"
slug: get-memcache-statistics-from-the-command-line-in-one-easy-command
title: Get memcache statistics from the command line in one easy command
---
A quick one for myself to come back to. I can never find this one liner when I need it! It's simple enough:

```
watch 'php -r '"'"'$m=new Memcache;$m->connect("127.0.0.1", 11211);print_r($m->getstats());'"'"
```

Next time I need to get memcached stats, I'll check back here.
