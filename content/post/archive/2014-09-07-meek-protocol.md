---
title: Meek Protocol
author: antitree
type: post
date: 2014-09-07T18:42:31+00:00
url: /2014/09/07/meek-protocol/
categories:
  - Censorship
  - Tor

---
The [Meek Protocol][1] has recently been getting a lot of attention since the Tor project made a few blog posts about it. Meek is a censorship evasion protocol that users a tactic called &#8220;domain fronting&#8221; to evade DPI-based censorship tactics. The idea is that using a CDN such as Google, Akamai, or Cloudflare, you can proxy connections (using the TLS SNI extension) so that if an adversary wanted to block or drop your connection, they would need to block connections to the CDN, like Google; mutually assured destruction. The goal being, a way of connecting to the Tor Network that is unblockable even from nation state adversaries.

# SNI and Domain Fronting

[SNI][2] is a TLS extension that&#8217;s been around for about nine years, and has been implemented in all modern browsers at this point. This is the TLS version of virtual hosting where you send an HTTP request to a server, and inside is a request to another host. Similar to virtual hosting&#8217;s host headers, SNI provides a host inside it&#8217;s extension during the client hello request:

<pre class="lang:default mark:11 decode:true " title="TLS handshake with SNI">Extension: server_name
  Type: server_name (0x0000)
  Length: 21
  Server Name Indicator extension
    Server Name list length: 19
    Length: 21
    Server Name Indication extension
      Server Name list length: 19
      Server Name Type: host_name (0)
      Server Name length: 16
      Server Name: www.antitree.com</pre>

This would be a request to https://www.google.com but the server receiving this request would look up the record to www.antitree.com to see if it was fronted, and forward the request to that host.

You can try this using the actual Meek server that Tor uses:

<pre class="wiki">wget -O - -q https://www.google.com/ --header 'Host: meek-reflect.appspot.com'</pre>

You should get a response of &#8220;I&#8217;m just a happy little web server.&#8221; which is what the meek-server default response is.

In terms of Internet censorship, the idea of using SNI to proxy a request through a CDN is called Domain Fronting and AFAIK, is currently only implemented by the Meek Protocol. (That being said, the idea can apply to just about any other protocol or tool. I&#8217;ve seen other projects use Meek or something like it. ) What Meek provides is a way of using Domain Fronting to create a tunnel for any protocol that needs to be proxied.

# Tor and Meek

The Meek Protocol was designed by some of the people involved with the Tor Project as one of the [pluggable transports][3] and is currently used to send the entire Tor protocol over a Meek tunnel. It does this using a little bit of infrastructure:

  * meek-client: This is what a client will use to initiate a tunnel over the Meek protocol
  * meek-server: corresponding server portion that will funnel requests and responses back over the Meek tunnel
  * web reflector: In its current form, this takes an SNI request, sees that it is a Meek request, and redirects it to the meek-server. This also makes sure that the tunnel is still running using polling requests.
  * CDN: the important cloud service that will be fronting the domain. The most common example is Google&#8217;s App-Spot.
  * Meek Browser Plugin: In order to make a meek-client request look like a standard SNI request (same TLS extensions) that your browser would make, a browser plugin is used.

Here&#8217;s a diagram of it all wrapped together:

[<img class="alignnone wp-image-798" src="http://www.antitree.com/wp-content/uploads/2014/09/meek-1024x423.png" alt="meek" width="800" height="331" />][4]

&nbsp;

This is how just a request is made to a Tor Bridge Node that&#8217;s running the meek-server software. Right now, if you download the latest Alpha release of the Tor Browser Bundle, this is how you could optionally connect using Meek.

# Polling

You might notice, that due to the fact HTTP (by design) doesn&#8217;t maintain any kind of state to keep a connection open for as long as you would like to tunnel your Tor traffic, the Meek protocol needs to compensate. It does this by implementing a polling method where a POST request is sent from the client to the server at a specified (algorithmic) interval. This is the main way that data is delivered once the connection has been established. If the server has something to send, it&#8217;s done in the POST response body, otherwise the message is still sent with a 0 byte body.

# Success Rate

You might notice that there are a few extra hops in your circuit and it&#8217;s true that there is a decent amount of overhead, but for those in China, Iran, Egypt or the ever-expanding list of other nations implementing [DPI based blocking as well as active probing][5], this is the difference between being able to use Tor, and not. The benefit here is that if you&#8217;re watching the connection, you&#8217;ll be able to see that a client IP made an HTTPS connection to a server IP owned by Google or Akamai. You cannot see if TLS handshake decide to support the SNI extension, and you cannot see whether or not the client HELLO contained a SNI &#8220;server_name&#8221; value. Without this, the connection is indistinguishable from a request to say Youtube or Google.

As of now, there does not seem to be a lot (compared to all Tor users) of users connecting over the Meek bridge but it does seem to be increasing in popularity.

[<img class="alignnone size-full wp-image-803" src="http://www.antitree.com/wp-content/uploads/2014/09/userstats-bridge-transport1.png" alt="userstats-bridge-transport[1]" width="576" height="360" />][6]

<p style="text-align: center;">
  <a href="https://metrics.torproject.org/userstats-bridge-transport.png?transport=meek">Updated Graph</a>
</p>

# Attacks

While no known attacks exist (besides an adversary blocking the entire CDN), there are some potential weaknesses that are being reviewed. One of the interesting ones is if an adversary is able to inject a RST packet into the connection, the tunnel would collapse and not re-establish itself. This is unlike a normal HTTP/S request that would just re-issue the request, and not care. This may be a way of fingerprinting the connections over time but there would be a fairly large cost to other connections in order to perform an attack like this. The other attack of note is traffic correlation based on the polling interval. If the polling interval was static at, for example, 50ms, it would be fairly easy to define a pattern for the meek protocol over time. Of course that&#8217;s not the case in the current implementation as the polling interval dynamically changes. The other attacks and mitigations can be found on the [Tor wiki page][7].

# Resources:

<https://trac.torproject.org/projects/tor/wiki/doc/meek> &#8211; main wiki page documenting how to use Tor with Meek

<https://trac.torproject.org/projects/tor/wiki/doc/AChildsGardenOfPluggableTransports#meek> &#8211; in depth explanation of the protocol compared to a standard Tor connection

 [1]: https://trac.torproject.org/projects/tor/wiki/doc/meek
 [2]: https://en.wikipedia.org/wiki/Server_Name_Indication
 [3]: https://www.torproject.org/docs/pluggable-transports
 [4]: http://www.antitree.com/wp-content/uploads/2014/09/meek.png
 [5]: http://freehaven.net/anonbib/#foci12-winter
 [6]: http://www.antitree.com/wp-content/uploads/2014/09/userstats-bridge-transport1.png
 [7]: https://trac.torproject.org/projects/tor/wiki/doc/meek#Barrierstoindistinguishability