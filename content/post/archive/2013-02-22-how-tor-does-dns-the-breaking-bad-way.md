---
title: 'How Tor does DNS: The Breaking Bad Way'
author: antitree
type: post
date: 2013-02-22T15:14:57+00:00
url: /2013/02/22/how-tor-does-dns-the-breaking-bad-way/
categories:
  - privacy
  - Tor

---
Let me start by answering the short version of the question: Tor usually performs DNS requests using the exit node&#8217;s DNS server. Because Tor is TCP, it will only be able to handle TCP based DNS requests normally. Hidden services though are very different and rely on Hidden Service Directory Servers that do not use DNS at all. Read on if you don&#8217;t believe me or want more information.

Here&#8217;s a reference from an old mailing list entry:

> Section 6.2 of the tor-spec.txt[5] outlines the method for connecting to a specific host by name. Specifically, the Tor client creates a RELAY_BEGIN cell that includes the DNS host name. This is transported to the edge of a given circuit. The exit node at the end of the circuit does all of the heavy lifting, it performs the name resolution directly with the exit node&#8217;s system resolver. &#8230;For the purposes of DNS, it&#8217;s important to note that a node does not need to be marked as an exit in the network consensus to perform resolution services on behalf of a client. Any node that doesn&#8217;t have an exit policy of &#8216;reject \*:\*&#8217; may be used for DNS resolution purposes. [[1][1]]

# Pudding for the Proof:

<span style="line-height: 13px;"><a href="/wp-content/uploads/2013/02/exit3.png"><br /> </a>Don&#8217;t believe me? Let&#8217;s test it out. If I run an exit node and then try to use it for a circuit, my DNS requests should go through it right? I&#8217;ve spun up an exit node named &#8220;BrianCranston&#8221; and I&#8217;ll setup a client (who I&#8217;m calling Aaron Paul)  to only use this box as it&#8217;s exit node. You can do this by adding the following to your TORRC file:</span>

<pre class="lang:default decode:true">ExitNodes briancranston</pre>

And on my exit node, I do a tcpdump of all traffic on port 53. On my client I start looking for BrianCranston&#8217;s websites at briancranston.com and briancranston.org. Lets see what it looks like:

[<img class="aligncenter size-full wp-image-518" alt="exit6" src="/wp-content/uploads/2013/02/exit6.png" width="613" height="119" />][2]

You&#8217;ll notice that Tor is being a little cheeky with the way it resolves DNS records.  briancranston.org turns into bRiancRAnsTON.org. <del>I don&#8217;t know if this is just a nice way to let Exit Node operators know which hosts are being resolved by Tor or what. </del> The reason for the odd casing in the DNS response is actually thanks to the Dan Kaminskys. Back in 2008, when exploiting DNS flaws was all the rage, one of the remediations for a DNS cache poisoning attack, was to randomize the casing of a DNS request. **This was called the &#8220;[0x20 hack][3]&#8221; because if you look at an ASCII table of letters, you&#8217;ll notice that the difference between the letter A and the letter a, is 0x20.** For example &#8220;a&#8221; is 0x61, and &#8220;A&#8221; is 0x41. The point here is that if someone wanted to attempt to pull off a cache poisoning attack, they&#8217;d have to brute force the possible case combinations. I&#8217;m told that modern browsers have this function built in now, but back in 2008, Tor was on the bleeding edge and its stayed in because it really doesn&#8217;t matter if a DNS query is 0x20&#8217;d multiple times. Thanks to [Isis][4] for pointing this out.

# UDP:

[<img class="alignright" alt="Billy Mays here. " src="/wp-content/uploads/2013/02/exit3-234x300.png" width="234" height="300" />][5]Tor is a TCP only network so what happens when it you need to use UDP services like DNS? The answer is pretty simple, it just doesn&#8217;t do it. [Colin Mulliner][6] and came up with a solution to this which was to relay UDP based DNS requests using a tool he wrote called [TTDNS][7].(If you&#8217;ve ever used TAILS, this is what it uses.) In short, it takes a UDP based DNS query, converts it to TCP, sends it out over Tor, and converts the reply back to UDP once it&#8217;s been received.

Tor doesn&#8217;t natively support UDP based DNS queries, but Tor also only does two types of DNS queries: A records, and PTR records. It skips around needing to use CNAME by converting them to A records but officially, those are the only two supported.

# Tools:

There are a couple of other items to note related to DNS. One is that there is a built-in tool called &#8220;tor-resolve.&#8221; Guess what it does&#8230; make DNS queries over the Tor network. This is useful for command-line scripts that are trying to resolve a host.

The other, is a TORRC option that will open up a port to provide DNS resolution over Tor. Once enabled, you can use the local host as a DNS resolver on the port you specify. Again, this is how TAILS handles DNS resolution.

<pre class="lang:default decode:true">DNSListenAddress 127.0.0.1
DNSPort 53</pre>

# What about Hidden services?

This is fine and dandy to resolve google.com, but what about a hidden service with a .onion address. Connecting to google.com goes out an Exit Node, but connecting to an .onion address never leaves the Tor network. In fact, Tor doesn&#8217;t even use DNS to resolve .onion addresses at all. Here&#8217;s how that works.

The names generated for .onion addresses are not just random values unique to your host, they are a base32 encoded version of the public key associated to your hidden service. When you create a hidden service, you generate a priv/pub key pair. This .onion address, the port it&#8217;s listening on, some other useful classifiers, and a list of &#8220;rendezvous points&#8221; are published to Tor hidden service directory nodes. The rendezvous points are locations on the Tor network where a client can initiate a connection to the hidden service.

So, following our Breaking Bad theme, if we had [Brian (Cranston)][8] and [Aaron (Paul)][9] wanted to exchange a secret web page that keeps track of all the meth they&#8217;ve sold, this is what the flow looks like:

  1. Brian modifies the TORRC to offer a service on an IP address and port (127.0.0.1:443)
  2. Brian creates a keypair for the service and the .onion address is saved (briancranston.onion)
  3. Brian&#8217;s Tor client sends a RELAY\_COMMAND\_ESTABLISH_INTRO to start creating introduction points
  4. Brian&#8217;s client sends the descriptors (introduction points, port, etc) to the Hidden Service Directory Servers
  5. Brian then sends Aaron his .onion address (briancranson.onion)
  6. Aaron&#8217;s client checks the Hidden Service Directory Server to see if the address exists
  7. Aaron&#8217;s Tor client makes a circuit to one of the introduction points
  8. Aaron connects to the introduction point and tells it about his rendezvous point.
  9. This rendezvous point is passed to Brian
 10. Brian connects to Aaron&#8217;s rendezvous point
 11. The rendezvous point lets Aaron know that Brian&#8217;s service has been forwarded at that point
 12. Aaron finally makes a connection to Brian&#8217;s service

EDIT: Modified to differential between &#8220;rendezvous&#8221; and &#8220;introduction points&#8221; in the steps. Thanks to [Isis][10] for pointing that out.

So in short, hidden services are resolved using Hidden Server Directory Servers and the Tor client. There currently is no way (AFAIK) to manually just resolve onion addresses. That means, if you&#8217;re trying to connect to a hidden service using a script, you&#8217;ll have to properly tunnel the requests through Tor. That&#8217;ll be for another day. [<img class="aligncenter size-full wp-image-522" alt="torba" src="/wp-content/uploads/2013/02/torba.png" width="650" height="406" />][11]

If you need more information, check out these links:

<http://archives.seul.org/or/talk/Jul-2010/msg00007.html> &#8211; old mailing list message about DNS. A bit out dated but very useful

<https://gitweb.torproject.org/torspec.git?a=blob_plain;hb=HEAD;f=rend-spec.txt> &#8211; discusses the rendezvous protocol specification that is the basis of hidden services.

 [1]: http://archives.seul.org/or/talk/Jul-2010/msg00007.html
 [2]: /wp-content/uploads/2013/02/exit6.png
 [3]: http://courses.isi.jhu.edu/netsec/papers/increased_dns_resistance.pdf
 [4]: https://blog.patternsinthevoid.net/
 [5]: /wp-content/uploads/2013/02/exit3.png
 [6]: http://www.mulliner.org/blog/blosxom.cgi/index.html?find=ttdnsd&plugin=find&path=
 [7]: https://gitweb.torproject.org/ioerror/ttdnsd.git
 [8]: http://www.imdb.com/name/nm0186505/
 [9]: http://www.imdb.com/name/nm0666739/?ref_=fn_al_nm_1
 [10]: https://twitter.com/isislovecruft
 [11]: /wp-content/uploads/2013/02/torba.png