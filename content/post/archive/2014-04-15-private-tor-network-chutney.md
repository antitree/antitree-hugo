---
title: 'Private Tor Network: Chutney'
author: antitree
type: post
date: 2014-04-15T20:08:58+00:00
url: /2014/04/15/private-tor-network-chutney/
categories:
  - Tor

---
[Chutney][1] is a tool designed to let you build your own private Tor network instance in minutes. It does so by using configuration templates of directory authorities, bridges, relays, clients, and a variety of combinations in between. It comes with a few examples to get you started. Executing this command, for example

<pre class="lang:default decode:true">./chutney configure networks/basic</pre>

will build a ten client network made up of three directory authorities, five relays, and two clients. Each of which have their own TORRC file, their own logs, and in the case of the relays and authorities, their own private keys. Starting them all up is as easy as

<pre>./chutney start networks/basic</pre>

[<img class="aligncenter size-medium wp-image-753" alt="Capture" src="/wp-content/uploads/2014/04/Capture1-300x144.png" width="300" height="144" />][2]

So bam! You&#8217;ve created a Tor network in less than a few minutes. One item to note, the configuration options in the latest version of Chutney use initiatives like &#8220;TestingV3AuthInitialVotingInterval&#8221; which require you to have a current version of Tor. In my case, I compiled Tor from source to make sure it included these options.

# Building custom torrc files

It&#8217;s first job during configuration is to build a bunch of torrc files to your specification. If you tell it you&#8217;d like to have a bridge as one of your nodes, it will use the bridge template on file, to create a custom instance of a bridge. It will automatically name it with a four digit hexadecimal value, set the custom configuration options you&#8217;ve dictated, and place it into its own directory under net/nodes/.

The very useful part here is that it builds an environment that is already designed to connect between one-another. When you create a client, that client is configured to use the directory authorities that you&#8217;ve already built and that directory authority is configured to serve information about the relays you&#8217;ve built. The real value here is that if you were to do this manually, you&#8217;d be left to the time consuming process of manually configuring each of these configuration files on your own. Do-able, but prone to human errors.

# Running multiples instances of Tor

Chutney builds a private Tor network instance by executing separate Tor processes on the same box. Each node is configured to specifically be hosted on a certain port, so for a ten node network, you can expect 20 more more ports being used by the processes.

# [<img class="aligncenter size-medium wp-image-752" alt="Capture" src="/wp-content/uploads/2014/04/Capture-300x160.png" width="300" height="160" />][3]Use case

Chutney is perfect for quickly spinning up a private Tor network to test the latest exploit, learn how the Tor software works, or just muck around with various configurations. Compared to [Shadow][4] which focuses more on simulating network events at a large scale, Chutney is more of a real-world emulator. One weakness is that it is not a one-to-one relationship between The Tor Network&#8217;s threat model and a Chutney configured one. For example, the entire network is running on a single box so exploiting one instance of Tor would most likely result in the entire network being compromised. That being said, depending on your needs, it offers a great way of  building a test environment at home with limited processing power. It&#8217;s still a rough-cut tool and it sounds like [NickM][5] is looking for contributors.

&nbsp;

 [1]: https://gitweb.torproject.org/chutney.git
 [2]: /wp-content/uploads/2014/04/Capture1.png
 [3]: /wp-content/uploads/2014/04/Capture.png
 [4]: http://shadow.github.io/
 [5]: https://twitter.com/nickm_tor