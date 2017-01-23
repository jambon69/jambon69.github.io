---
layout: post
title:  "Write UP Insomniack cryptoquizz!"
date:   2017-01-23 16:00:00 +0100
categories: jekyll update
---

For this first chall we are provided a simple  *quizz.teaser.insomnihack.ch:1031*. We try to connect to the ip and the port using netcat, and then we are confronted with this:
![cryptoQuizz]({{ site.url }}/assets/insomnihack/cryptoQuizz.png)

Looks like an easy challenge, we just have to find the birthdate of serveral people. We could just try to find them all and then do an *if-serial*, but we'll be smarter than that and make a little script
{% highlight python %}
import socket
import urllib2

host = "quizz.teaser.insomnihack.ch"
port = 1031

socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
socket.connect((host, port))
print "Connection on {}".format(port)

a = socket.recv(2048)
while True:
	a = socket.recv(2048)
	print a
	toto = a.split("of")[1].split("\n")[0].split("?")[0]
	toto = toto[1:]
	toto = toto.split(" ")
	response = ""
	try:
		url = 'https://fr.wikipedia.org/wiki/'
		for elem in toto:
			url = url + elem + "_"
		response = urllib2.urlopen(url)
		html = response.read()
		response = html.split("Naissance en ")[1].split('"')[0].split()
	except:
		url = 'https://en.wikipedia.org/wiki/'
		for elem in toto:
			url = url + elem + "_"
		response = urllib2.urlopen(url)
		html = response.read()
		response = html.split()
	for elem in response:
		if len(elem) == 4 and elem[0] == '1' and elem[1] == '9':
		socket.send(elem + "\n")
		break
{% endhighlight %}

We may want to launch the script multiple times because those guys don't always have their birthdate on Wikipedia.
I don't know if it's made on purpose or not, but the challenge's author could have chosen better people I guess.

And here we have it !

`OK, young hacker. You are now considered to be a <b>INS{GENUINE_CRYPTOGRAPHER_BUT_NOT_YET_A_PROVEN_SKILLED_ONE}</b>`
