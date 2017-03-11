---
title: Run a private tor network using Docker
author: antitree
type: post
date: 2016-07-01T21:45:18+00:00
url: /2016/07/01/run-a-private-tor-network-using-docker/
categories:
  - Tor

---
I&#8217;ve made a scalable way of building a fully private functioning tor network using Docker. Why give any back story, if it&#8217;s useful to you, then here you go:

Source: <https://github.com/antitree/private-tor-network>

Docker Hub: <https://hub.docker.com/r/antitree/private-tor/>

## Setup

All you really need to do is clone the git repo, build the image (or download from Docker Hub) and then spin up a network to your liking. What&#8217;s nice about this is you can use the docker-compose scale command to build any size network that you want. Eventually, when in the next version of Docker you&#8217;ll be able to scale across multiple hosting providers. But the current RC is too sketchy to invest any time in.

Anyways, here&#8217;s how to spin up 24 node network. That&#8217;s 15 exits, 5 relays, 1 client, and 3 dir authorities.

<pre class="lang:default decode:true">docker-compose up
docker-compose sale relay=5 exit=15</pre>

## Using It

To use this, there is a port listening on 9050 (you can change this in the docker-compose.yml file). If you point your browser to the Docker host running your containers and, in the same way you would connect to Tor, use it as a SOCKS5 proxy server, you will suddenly use it.

If you aren&#8217;t sure if it&#8217;s working, you check out the logs

<pre class="lang:default decode:true">docker-compose logs
</pre>

<span style="line-height: 1.5;">or</span>

<pre class="lang:default decode:true ">docker logs -f nameoftherelay</pre>

Or, if you want to get cool information, try setting up a [depictor][1] container that will give you data about the DA&#8217;s.

## Now what?

To answer the &#8220;now what?&#8221; question, this is a clean way of getting a tor network running so you can do your research, learn about how it works, modify configurations, run some third party tor software&#8230; whatever. If you&#8217;re not into this, there is always [Shadow][2] and [Chutney][3] or just manually [configuring hosts/processes your self.][4]

 [1]: https://gitweb.torproject.org/depictor.git
 [2]: https://shadow.github.io/
 [3]: https://gitweb.torproject.org/chutney.git/
 [4]: https://ritter.vg/blog-run_your_own_tor_network.html