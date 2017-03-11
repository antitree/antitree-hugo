---
title: 'Malicious Exit Nodes: Judge Dredd or Anarchy?'
author: antitree
type: post
date: 2013-03-30T03:47:01+00:00
url: /2013/03/29/malicious-exit-nodes-judge-dredd/
categories:
  - privacy
  - Tor

---
[InspecTor][1] is a .onion page that kept track of bad exit nodes on the network. And it did a pretty good job. It looked for things like:

  * <span style="line-height: 13px;">SSL Stripping: Replacing HTTPS links with HTTP</span>
  * JavaScript injection
  * iFrame injection
  * Exit nodes that have no exit policy (black holes)

Those are the easy to quantify bad properties. We can compare the results of connecting to a bad Exit Node and a good one and diff the results. These are some of the grey areas it also tries to look for:

  * <span style="line-height: 13px;">Warning about similar nodes in the same netblock</span>
  * Watch for similar named nodes spinning up hundreds of instances
  * Look at the names of the nodes and conclude that they&#8217;re bad (e.g. NSAFortMeade)

The worst case scenario for a service like this, is that first, they&#8217;re wrong and kick off a perfectly good Exit Node. Second, they make users use custom routes to evade the bad nodes. Doing so means that your network traffic has a fingerprint. &#8220;He&#8217;s the guy that never users Iranian exits&#8221; for example.

And that&#8217;s kind of what happened with InspecTor &#8211; now celebrating the anniversary of it&#8217;s retirement a year ago. He went Judge Dredd on Tor and started making broad conclusions on what nodes were evil. For example, he said that NSAFortMeade is obviously an Exit Node owned by the NSA assumedly to catch the traffic of Americans (because they can&#8217;t do that already?). Other conclusions stated that a family of Tor nodes were from Washington DC. One of them was malicious so the conclusion was that it was probably the Government keeping an eye on us.

# Tor&#8217;s Controls

What does Tor have as a control mechanism if they do somehow come across a bad exit node? The protocol has a &#8220;bad-exit&#8221; flag in it so that authorities can let Tor users that this Exit-Node should be avoided. That flag is set by The Tor Project admins as far as I know and you have to be blatantly offensive to cause this to happen. Here is the \_total\_ list  of nodes that are blocked today:

<table width="742" border="0" cellspacing="0" cellpadding="0">
  <colgroup> <col width="226" /> <col width="219" /> <col width="147" /> <col width="150" /> </colgroup> <tr>
    <td width="226" height="20">
      agitator
    </td>
    
    <td width="219">
      agitator.towiski.de [188.40.77.107]
    </td>
    
    <td width="147">
      Directory Server
    </td>
    
    <td width="150">
      Guard Server
    </td>
  </tr>
  
  <tr>
    <td height="20">
      Unnamed
    </td>
    
    <td>
      vz14796.eurodir.ru [46.30.42.154]
    </td>
    
    <td>
      Exit Server
    </td>
    
    <td>
      Guard Server
    </td>
  </tr>
  
  <tr>
    <td height="20">
      Unnamed
    </td>
    
    <td>
      vz14794.eurodir.ru [46.30.42.152]
    </td>
    
    <td>
      Exit Server
    </td>
    
    <td>
      Tor 0.2.3.25 on Linux
    </td>
  </tr>
  
  <tr>
    <td height="20">
      Unnamed
    </td>
    
    <td>
      vz14795.eurodir.ru [46.30.42.153]
    </td>
    
    <td>
      Exit Server
    </td>
    
    <td>
      Guard Server
    </td>
  </tr>
</table>

<p style="text-align: right;">
  <a href="http://torstatus.blutmagie.de/index.php?SR=FBadExit&SO=Desc">http://torstatus.blutmagie.de/index.php?SR=FBadExit&SO=Desc</a>
</p>

This says that there are four bad nodes (one&#8217;s a bad directory server) on the network right now. I think most people would agree that is a bit low. You can take a look at [this link][2] for a complete list of the nodes they&#8217;ve blocked in the past. You should notice that a bad-exit flag doesn&#8217;t kick them off the network, it just tells the client to never use them as an exit. So these nodes can stay online as long as they want but they&#8217;ll never be used.

# The Point

The point is not to just say everything sucks. How Tor isn&#8217;t doing a good job at monitoring for Exit Nodes or how InspecTor was doing too good of a job for it&#8217;s own good. It&#8217;s to highlight the real-world problem in Tor. Unlike the sexy theoretical attacks we like to wrap our heads around like [global adversaries correlating your traffic back to an individual IP by statistically analyzing your web history patterns][3], the most likely thing to happen to you is that [some douche nuckle is running dsniff and ulogd][4]. And the point is also to highlight a need for a replacement of Snakes On A Tor. You can tell by it&#8217;s name, it&#8217;s a bit outdated. That is something actively being worked on but it may be a while before something reliable comes out of it.

&nbsp;

 [1]: http://xqz3u5drneuzhaeo.onion/users/badtornodes/
 [2]: https://trac.torproject.org/projects/tor/wiki/doc/badRelays
 [3]: https://blog.torproject.org/blog/experimental-defense-website-traffic-fingerprinting
 [4]: https://lists.torproject.org/pipermail/tor-relays/2012-March/001252.html