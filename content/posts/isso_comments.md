---
title: "ISSO Commenting System"
date: 2018-06-01T09:36:22-05:00
draft: false
tags: ["python", "ISSO", "cloud"]
categories: ["tech"]
---

### Setting up the ISSO Commenting System for my HUGO Static Website

After having an amazingly positive and swift experience building now two websites with the static website generator HUGO it was time to start 
focusing on the minor dynamic bits of the website.  First, was to get a commenting system up and running.  My ideal criteria were
open-source, free, python-based and would work with HUGO.

<!--more-->

On the Hugo site there is a [section on comment systems](https://gohugo.io/content-management/comments/) after reading through the 
various options, I had a [good gut feeling for ISSO](https://posativ.org/isso/).  Maybe it was the cute logo character.  But clearly Discus with the link hi-jacking
and other swarmy methods was out.

What cinched the decision to go with ISSO was the disocvery of this [awesome walk-through of how to get ISSO running with HUGO](https://stiobhart.net/2017-02-24-isso-comments/). The rest of this post is basically me walking though his walkthrough but with details that he did not 
cover as our setups differed a bit.

ISSO had all of the requirements met, but it was not available as a simply 1-click install like setting up a wikimedia wiki on my current 
hosting supplier: Dreamhost. Dreamhost is happy enough to sell you dedicated servers, but those monthly fees would be silly for just the odd
comments on a hobby blog about our sailing. Clearly, cloud computing would be the answer, but after coming back from PyCon my head was still
swimming with cloud providing offerings: Azure, AWS, Google, DigitalOcean and another dozen more.  Which one?

Kyle and I went to this really strong 2 hour overview of all of the Google Cloud products at PyCon and I was quite excited about all the amazing
tools they have to play with under the infamous hamburger menu.  Plus, Google's hyper fractional accounting appeared to be the most kind to 
my wallet with rates in the pennies for what I expected to consume.

## Creating a virtual Python server on the Google platform

So first thing is to head over to [Google's Cloud Platform](https://console.cloud.google.com/).
If you are not already logged into your google account of choice, now is the time to do so.  This should be the google account
that you would ultimately consider using your credit card and paying for your services down the road.  But I started with $300 of credit and a year to use it.

### Compute Engine
Next, open the hamburger menu and select *Compute Engine*.

### VM Instances
Now you are going to create a freaking server with a few clicks - Select the *VM Instances* sub-menu.

### What flavor of server to create?
This is where I lost some time as I do not have a web sys admin background.
For the purposes of getting ISSO to work, I am going to call out the key options you need to select:

Select the smallest server possible (as this is a hobble blog with very low traffic):
* Machine type: f1-micro (1 vCPU, 0.6 GB memory)
* OS - I selected Unbunto-18 (many possible choices that would work probably very well)
* [X] Allow HHTP Traffic  
* [X] Allow HHTPS Traffic

### Opening a firewall hole for ISSO to communicate
After your server is created, click on Network Details

We are going to need to add a firewall rule in order to allow traffic in and out of new shiny server:

#### New firewall rule
* Give it a name (isso is fine)
* Description: (whatever you want)
* Network: default (is fine)
* Priority: 1000 (is fine)
* Direction of traffic: Ingress
* Action on Match: Allow *This is important*
* Targets: All Instances in the Network
* Source Filter: IP Targetting
* Source IP Ranges: 0.0.0.0/0
* Protocols and Ports: TCP: Specify a port number for isso - (I chose 8181) TCP is the key choice here
<p>

### SSH In and get to work
Now you should have a shiny new Ubuntu instance with this one open firewall port.  Time to start setting up ISSO.

Google gives you a couple of ways to SSH into your box, for the first time around it will need to set up some secret keys 
and ask you to set a pass phrase.  It gives you a cool2D fingerprint and your secret key.  Save those off someplace safe, and remember your pass phrase.

I tried using the integrated SSH in the Google Cloud web app, but that ended up being too small of a window, so I went with 
open the SSH in a dedicated window.

SSH into your box and look around a bit with the usual unix commands like ls, ls -al, cd .., cd ~, and get a feel for this new unix box you have!

Now, to install python stuff you pretty much just do ```pip install new_package``` or ```pip3 install new package```

Unbunto has a python3 but did not seem to have pip ready to go.

A quick google search gave me this rock-solid answer on [askunbuntu on the stackechange](https://askubuntu.com/questions/672808/sudo-apt-get-install-python-pip-is-failing)

```
sudo apt-get install software-properties-common
sudo apt-add-repository universe
sudo apt-get update
sudo apt-get install python-pip
```

Okay now, you will have much more than just pip.

Next walk through [awesome walk-through of how to get ISSO running with HUGO](https://stiobhart.net/2017-02-24-isso-comments/) the steps
of setting up your python virtual environment with pipenv, get ISSO into machine and edit the isso.cfg file.  I created and edited the isso.cfg
on my local machine and then uploaded it due to the lag I was having with SSH session, and then later I installed [jove (an emacs clone)](https://en.wikipedia.org/wiki/JOVE) and just did the small edit tweaks in-situ on the virtual box.

### isso.cfg
There is not a whole lot to do here, it is a great self-documenting config file with a lot of sensible defaults.

*IMPORTANT* You must edit the [server] listen to point to the *Internal* IP address that google assigns to the machine

While on the *client* (read where you host your HUGO site) will need to proxy to the *External* IP address that google assigns to your machine

I turned off moderation to start with, so I could skip the setup of the SMTP email relays while I focused on getting the key service up and running.

### Where I am at now:
I have the isso commenting box rendering where I want when I use the dummy ```static\js\embed.min.js``` locally.

When I hit the isso server at ```http://35.237.78.217:8181/``` I get:
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>400 Bad Request</title>
<h1>Bad Request</h1>
<p>missing uri query</p>
```
As expected in the walkthrough for an ISSO server up and running and just getting a raw request on its port

When I hit my server with ```http://35.237.78.217:8181/js/embed.min.js``` the ISSO server on the virtual python box returns to me the dynamically generated embed.min.js


And on
```
201.116.2.165 - - [2018-06-02 03:42:53] "GET /isso/js/embed.min.js HTTP/1.1" 404 615 0.000529
201.116.2.165 - - [2018-06-02 03:43:06] "GET /js/embed.min.js HTTP/1.1" 200 56497 0.025108
201.116.2.165 - - [2018-06-02 03:44:35] "GET / HTTP/1.1" 400 517 0.001041
```

Checking the isso.log file, I discover that because I skipped on setting up the SMTP it is exceptioning out:

I think because I have 2-step authentication I cannot get this account to simply login here
```

Traceback (most recent call last):
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 43, in __init__
    with self:
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 64, in __enter__
    timeout=self.conf.getint('timeout'))
  File "/usr/lib/python3.6/smtplib.py", line 251, in __init__
    (code, msg) = self.connect(host, port)
  File "/usr/lib/python3.6/smtplib.py", line 336, in connect
    self.sock = self._get_socket(host, port, self.timeout)
  File "/usr/lib/python3.6/smtplib.py", line 307, in _get_socket
    self.source_address)
  File "/usr/lib/python3.6/socket.py", line 704, in create_connection
    for res in getaddrinfo(host, port, 0, SOCK_STREAM):
  File "/usr/lib/python3.6/socket.py", line 745, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
socket.gaierror: [Errno -2] Name or service not known
connected to http://sailadastra.netlify.com/
```



```
unable to connect to SMTP server
Traceback (most recent call last):
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 43, in __init__
    with self:
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 64, in __enter__
    timeout=self.conf.getint('timeout'))
  File "/usr/lib/python3.6/smtplib.py", line 251, in __init__
    (code, msg) = self.connect(host, port)
  File "/usr/lib/python3.6/smtplib.py", line 336, in connect
    self.sock = self._get_socket(host, port, self.timeout)
  File "/usr/lib/python3.6/smtplib.py", line 307, in _get_socket
    self.source_address)
  File "/usr/lib/python3.6/socket.py", line 704, in create_connection
    for res in getaddrinfo(host, port, 0, SOCK_STREAM):
  File "/usr/lib/python3.6/socket.py", line 745, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
socket.gaierror: [Errno -2] Name or service not known
connected to http://sailadastra.netlify.com/
```

Please make sure you are hitting the *gmail* SMTP server not the non-existant google SMTP server

```
host = smtp.gmail.com
```

Need to change port from 465 to 587
```
Traceback (most recent call last):
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 43, in __init__
    with self:
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 64, in __enter__
    timeout=self.conf.getint('timeout'))
  File "/usr/lib/python3.6/smtplib.py", line 251, in __init__
    (code, msg) = self.connect(host, port)
  File "/usr/lib/python3.6/smtplib.py", line 338, in connect
    (code, msg) = self.getreply()
  File "/usr/lib/python3.6/smtplib.py", line 394, in getreply
    raise SMTPServerDisconnected("Connection unexpectedly closed")
smtplib.SMTPServerDisconnected: Connection unexpectedly closed
connected to https://sailadastra.netlify.com/
unable to connect to SMTP server
```

Need to allow less secure apps to login
```
Traceback (most recent call last):
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 43, in __init__
    with self:
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 80, in __enter__
    self.client.login(username, password)
  File "/usr/lib/python3.6/smtplib.py", line 730, in login
    raise last_exception
  File "/usr/lib/python3.6/smtplib.py", line 721, in login
    initial_response_ok=initial_response_ok)
  File "/usr/lib/python3.6/smtplib.py", line 642, in auth
    raise SMTPAuthenticationError(code, resp)
smtplib.SMTPAuthenticationError: (534, b'5.7.14 <https://accounts.google.com/signin/continue?sarp=1&scc=1&plt=AKgnsbuG\n5.7.14 DaFRDB0fuUPEva
V0uYwz4tNUDueSKU6SP2sOOJz4DTwPUOqYmBupbbri68rcnTBz-zTxen\n5.7.14 PY6s7i0kqX22aNsx6gd678jVsBqrdqgywzLdQkrL4p4FcaNkhBWpmLb4vNhdWY0YfUWVyD\n5.7.
14 z-j36RKS7iQAcMId52j83Q7JfYMGgce4ZRXIYSi1W1X_hO_kgjT0_FLTKZfrLw_PrkWwfW\n5.7.14 1GaoJwER0KEb-ZLXAsZ0QBZqbgojY> Please log in via your web b
rowser and\n5.7.14 then try again.\n5.7.14  Learn more at\n5.7.14  https://support.google.com/mail/answer/78754 j9-v6sm5873468vkf.29 - gsmtp'
)
connected to https://sailadastra.netlify.com/
```
Okay well, using google for sending mail looks like it is not going to work, let's try using an the SMTP server at Dreamhost:

After changing to a Dreamhost email & SMTP server:
```

Traceback (most recent call last):
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 43, in __init__
    with self:
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/ext/notifications.py", line 64, in __enter__
    timeout=self.conf.getint('timeout'))
  File "/usr/lib/python3.6/smtplib.py", line 251, in __init__
    (code, msg) = self.connect(host, port)
  File "/usr/lib/python3.6/smtplib.py", line 336, in connect
    self.sock = self._get_socket(host, port, self.timeout)
  File "/usr/lib/python3.6/smtplib.py", line 307, in _get_socket
    self.source_address)
  File "/usr/lib/python3.6/socket.py", line 724, in create_connection
    raise err
  File "/usr/lib/python3.6/socket.py", line 713, in create_connection
    sock.connect(sa)
socket.timeout: timed out
connected to https://sailadastra.netlify.com/
```


## Still no comments...
### Problem: client getting 404 errors on all api calls
No errors in the isso.log

### Context:
Site is a HUGO static site being built and served from Netlify
Netlify's redirection instruction in netlify.toml:
[[redirects]]
  from = "/isso/*"
  to = "http://35.237.78.217:8181/:splat"  
  status = 200
  force = true

ISSO is running on a virtual compute engine on google's cloud Ubuntu 18
With a firewall ingress for tcp:8181
And a firewall egress for tcp:80

Server is up and running with this happy note in the log file:
connected to https://sailadastra.netlify.com/

Note #1 - I could not get the comment box to render until I provided my own embed.min.js at 

Manually hitting the dynamic js
http://sailadastra.netlify.com/isso/embed.min.js
Produces the missing uri query error page, which I believe means that ISSO is up and running, but simply grumpy because no API parameters are being sent

But, interestingly, if I try to hit the js manually at:
http://35.237.78.217:8181/js/embed.min.js
It does return the *dynamically* generated js!

On initial rendering of a blog post with the comment box:

    <script data-isso="sailadastra.netlify.com/isso/" src="{{ .Site.BaseURL }}js/embed.min.js"></script>

https://sailadastra.netlify.com/posts/isso_comments/sailadastra.netlify.com/isso/count
the server responded with a status of 404 ()

https://sailadastra.netlify.com/posts/isso_comments/sailadastra.netlify.com/isso/?uri=%2Fposts%2Fisso_comments%2F&nested_limit=5
the server responded with a status of 404 ()

After clicking on the submit button
https://sailadastra.netlify.com/posts/isso_comments/sailadastra.netlify.com/isso/new?uri=%2Fposts%2Fisso_comments%2F
the server responded with a status of 404 ()


Tried this variation on the send:
    <script data-isso="https://sailadastra.netlify.com/isso/" src="{{ .Site.BaseURL }}js/embed.min.js"></script>

https://sailadastra.netlify.com/isso/count
the server responded with a status of 405 ()
https://sailadastra.netlify.com/isso/?uri=%2Fposts%2Fisso_comments%2F&nested_limit=5
the server responded with a status of 404 ()
https://sailadastra.netlify.com/isso/new?uri=%2Fposts%2Fisso_comments%2F
the server responded with a status of 405 ()

And manually hitting the static ip address for
http://35.237.78.217:8181/new?uri=%2Fposts%2Fisso_comments%2F
Failed to load resource: the server responded with a status of 405 (METHOD NOT ALLOWED)

## HTTPS makes ISSO grumpy
In my isso.cfg I get this error id I try to use https:

[general]
host =
    https://sailadastra.netlify.com/

[server]
listen = https://10.142.0.2:8181/

```
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/bin/isso", line 11, in <module>
    sys.exit(main())
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/__init__.py", line 270, in main
    wsgi.SocketHTTPServer(sock, make_app(conf)).serve_forever()
  File "/home/erikbethke/.local/share/virtualenvs/isso-1X4e4EMV/lib/python3.6/site-packages/isso/wsgi.py", line 203, in __init__
    HTTPServer.__init__(self, sock, SocketWSGIRequestHandler)
  File "/usr/lib/python3.6/socketserver.py", line 453, in __init__
    self.server_bind()
  File "/usr/lib/python3.6/http/server.py", line 138, in server_bind
    self.server_name = socket.getfqdn(host)
  File "/usr/lib/python3.6/socket.py", line 669, in getfqdn
    name = name.strip()
AttributeError: 'int' object has no attribute 'strip'
```

[general]
host =
    http://sailadastra.netlify.com/

[server]
listen = http://10.142.0.2:8181/

Will produce no errors

## Boom! I needed the :splat in the redirect!
[[redirects]]
  from = "/isso/*"
  to = "http://35.237.78.217:8181/:splat"
  status = 200
  force = true

And now comments work!

#### How to stop and start the server
isso $: kill -SIGINT $(pgrep isso)

pipenv shell

isso -c ./isso.cfg run &

#### 
how to start on reboot?

### Further reading on ISSO comments

* https://matthiasadler.info/blog/isso-comment-integration/
* https://ericmathison.com/blog/how-to-install-isso-commenting-server-ubuntu-16-04/
* https://www.hallada.net/2017/11/15/isso-comments.html

