---
title: 'Onion Porn: HashedControlPassword'
author: antitree
type: post
date: 2012-10-27T16:39:27+00:00
url: /2012/10/27/onion-porn-hashedcontrolpassword/
categories:
  - Crypto
  - Python
  - Tor

---
<p class="brush:python">
  I love excessively documented things. Design documents, protocols specs&#8230;whatever and Tor is one of those projects that I&#8217;ve always loved to randomly poke around into. I got sucked into Tor&#8217;s source code today and was entertained by the results. <strong>Beware, crypto time suck ahead.</strong>
</p>

# ControlPort

Tor is controllable by making socket connections to it&#8217;s aptly named &#8220;ControlPort&#8221; usually on port 9051. The control port is not enabled by default if you&#8217;ve just installed Tor on Ubuntu or something, but it is enabled on the Windows packages that use front-ends like Vidalia. Anyways, this gives various third parties the ability to control all aspects of Tor on-the-fly.

There was a <a title="2600 Article" href="http://www.thesprawl.org/research/tor-control-protocol/" target="_blank">2600 article years ago</a>, about using the control port and it gives a really cool summary of some of the stuff you can do. Create custom circuits, create super long circuits, generate new circuits on demand. If you&#8217;ve used Vidalia, this is what it interfaces with.

But what I got sucked into today is the fact that there&#8217;s two ways to authenticate with the control port: Cookie, and HashedPassword. (Or none if you don&#8217;t care.) . This is what your standard TORRC looks like:
  
`<br />
## The port on which Tor will listen for local connections from Tor<br />
## controller applications, as documented in control-spec.txt.<br />
ControlPort 9051<br />
## If you enable the controlport, be sure to enable one of these<br />
## authentication methods, to prevent attackers from accessing it.<br />
#HashedControlPassword 16:872860B76453A77D60CA2BB8C1A7042072093276A3D701AD684053EC4C<br />
` 

# Crypto

Normally you can generate a hashed password for saving in your TORRC file by using the &#8220;&#8211;hash-password&#8221; switch to Tor. But today I wanted to waste my time and see how it&#8217;s generated. Thankfully, even though the documentation is a bit outdated, the control spec still goes into some pretty good detail.

<https://gitweb.torproject.org/torspec.git/blob/HEAD:/control-spec.txt>

Section 5.1 covers authentication and the HashedControlPassword function which looks like this:

<pre>If the 'HashedControlPassword' option is set, it must contain the salted
  hash of a secret password.  The salted hash is computed according to the
  S2K algorithm in RFC 2440 (OpenPGP), and prefixed with the s2k specifier.
  This is then encoded in hexadecimal, prefixed by the indicator sequence
  "16:".  Thus, for example, the password 'foo' could encode to:
     16:660537E3E1CD49996044A3BF558097A981F539FEA2F9DA662B4626C1C2
        ++++++++++++++++**^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
           salt                       hashed value
                        indicator</pre>

I&#8217;m all double rainbow on this at first which made me spend more time on it. Se we see the RFC for OpenPGP and how it documents the generation of this hash. And it starts to make sense. I&#8217;ll try to explain how I understand it:

OpenPGP&#8217;s RFC explains that the S2K (string to key) algorithm takes in a secret value, a specifier (I&#8217;ll come back to), an 8 byte salt, a &#8220;count&#8221; value, and generates  a hash. The count value is an encoded hex value that affects how many times the secret is hashed. The salt is just a random 8 byte value. But the specifier dictates the type of S2K and the hashing function to use. It consists of 2 bytes, the first specifies the type of hashing function that will be performed. These options are:

**0x00 Simple: A key comes in, a hash goes out**

**0x01 Salted: A key is salted and then hashed**

**0x03 Iterated and Salted: A key, and a salt, all hashing the crap out of eachother over and over. This is what Tor uses.**

The second byte specifies the hashing algorithm. In the case of Tor, they use SHA1 so this specifier should be 0x02. So putting that together for Tor, we would expect to see the hex encoded version of 32. But you&#8217;ll notice in the control-spec, there is no 32 at all but there is a &#8220;16:&#8221;.

I spent far too long trying to rationalize how 16 fits into the hash but it turns out that Tor does not use the specifier in their implementation  of S2K. That makes sense after I think about it since Tor doesn&#8217;t need to worry about different hashing algorithms or varied number of hashing iterations. They implement a static password hashing function that uses SHA1 and iterates based on a counter value of 60 which is also static. This is why you&#8217;ll always a 60 in the middle of the HashedControlPassword value in the TORRC file.

**I really needed to find out why 16. There had to be a reason. The most amount of beers that Roger Dingledine consumed in one sitting? The number of times Nick Mathewson was arrested for dealing meth?** Someone from the Tor-talk mailing list finally pointed out what I was missing:

<pre class="brush:c">if (!strcmpstart(hashed, "16:")) {
 if (base16_decode(decoded, sizeof(decoded), hashed+3, strlen(hashed+3))&lt;0
 || strlen(hashed+3) != (S2K_SPECIFIER_LEN+DIGEST_LEN)*2) {
 goto err;
 }
} else {
 if (base64_decode(decoded, sizeof(decoded), hashed, strlen(hashed))
 != S2K_SPECIFIER_LEN+DIGEST_LEN) {
 goto err;
 }
}</pre>

16 appears to be describing whether or not it&#8217;s base16 as opposed to base64. There&#8217;s nothing in the documentation about this and attempts to use a base64 encoded password didn&#8217;t work out for me. I&#8217;ll have to just leave this alone until someone can tell me more.

# Code

Nothing useful really came out of this besides making myself happy diving into some code but I still wanted an actual output of some kind so I rolled my own version of the hashing algorithm in Python.

<pre class="lang:python decode:true">import os, binascii, hashlib

#supply password
secret = 'foo'

#static 'count' value later referenced as "c"
indicator = chr(96)

#used to generate salt
rng = os.urandom

#generate salt and append indicator value so that it
salt = "%s%s" % (rng(8), indicator)

#That's just the way it is. It's always prefixed with 16
prefix = '16:'

# swap variables just so I can make it look exactly like the RFC example
c = ord(salt[8])

# generate an even number that can be divided in subsequent sections. (Thanks Roman)
EXPBIAS = 6
count = (16+(c&15)) &lt;&lt; ((c&gt;&gt;4) + EXPBIAS)  #

d = hashlib.sha1()

#take the salt and append the password
tmp = salt[:8]+secret

#hash the salty password as many times as the length of
# the password divides into the count value
slen = len(tmp)
while count:
  if count &gt; slen:
    d.update(tmp)
    count -= slen
  else:
    d.update(tmp[:count])
    count = 0
hashed = d.digest()
#Convert to hex
salt = binascii.b2a_hex(salt[:8]).upper()
indicator = binascii.b2a_hex(indicator)
torhash = binascii.b2a_hex(hashed).upper()

#Put it all together into the proprietary Tor format.
print(prefix + salt + indicator + torhash)</pre>

This will generate a hash for the password &#8220;foo&#8221;. Place the hash in the TORRC file and restart Tor and then you can test like this:

`[me@me]# nc localhost 9051<br />
authenticate "foo"<br />
250 OK<br />
` 

# External Links

  * <https://gitweb.torproject.org/tor.git/blob?f=src/common/crypto.c#l2829> &#8211; the &#8220;secret\_to\_key&#8221; method implements most of the OpenPGP standard.
  * <https://gitweb.torproject.org/tor.git/blob?f=src/or/control.c#l1153> &#8211; portion of Tor source code that reads in the HashedControlPassword value
  * <https://gitweb.torproject.org/torspec.git/blob/HEAD:/control-spec.txt> &#8211; the Tor control spec. (Planned to be updated &#8220;soon&#8221;)
  * <https://gist.github.com/3962751> &#8211; my Tor hashing algorithm in Python
  * <http://www.thesprawl.org/research/tor-control-protocol/> &#8211; The Sprawl&#8217;s article that was posted in 2600 about using the control port for fun and profit

&nbsp;