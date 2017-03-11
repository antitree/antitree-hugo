---
title: 6 Months of Tor in the Clouds!
author: antitree
type: post
date: 2012-04-11T11:36:53+00:00
url: /2012/04/11/6-months-of-tor-in-the-clouds/
categories:
  - Tor

---
&nbsp;

**It&#8217;s been 6 month&#8217;s since I started running a Tor bridge node on an Amazon EC2 instance**. Back then, Tor had just announced an initiative to get people setting up [cloud images to run as bridge nodes][1]. This was during the then recent upheaval in the Middle East where connections to the Internet were either disabled completely, or they were extremely restricted as to what sites they were allowed to see. Tor couldn&#8217;t directly help with re-establishing network connectivity, but those that blocked Twitter and other social networking sites, could be evaded by Tor and their bridge nodes.

Skip this paragraph if you already know about bridge nodes: Tor has built in features that  make it hard to detect at the protocol level. When a user establishes a connection to a entry node, the data is encrypted and designed to be difficult to fingerprint so firewalls/network policies have trouble detecting who&#8217;s using Tor. As a result, companies/countries/fascist organizations have created a list of all the Tor entry nodes (information that is publicly available) and blocked access to them completely. To circumvent this, bridge nodes were created. **When a user finds themselves blocked from connecting to the Tor network, they can request a bridge node** through a couple of different ways but most commonly, emailing &#8220;bridges@torproject.org&#8221; will automatically reply with a current bridge node. But why am I explaining this. Go [here to learn all about them][2].

Running a bridge node works perfectly for Amazon&#8217;s Free Tier since they&#8217;re lower traffic than an exit or entry node. **In fact, I have not spent a single penny while running it.**

<p style="text-align: left;">
  <strong>Below are the days with the highest usage in bytes</strong>. November was definitely the Middle East scuffles and you can probably chalk up most of the others to the same. I was trying to correlate a specific event that happened on these days but couldn&#8217;t find any. If you notice something, let me know. I&#8217;m guessing a blog post went online showing more people over there how they can use Tor and bridge nodes. To get setup and run a bridge node of your own on an EC2 instance, you can <a href="http://cloud.torproject.org">read more here</a>.
</p>

<table class="aligncenter" width="400" border="0" cellspacing="0" cellpadding="0">
  <colgroup> <col width="111" /> <col width="64" /> </colgroup> <tr>
    <td align="left" width="300" height="20">
      11/21/2011 16:00
    </td>
    
    <td align="right" width="100">
      5034870
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      11/21/2011 17:00
    </td>
    
    <td align="right">
      3861440
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      11/26/2011 6:00
    </td>
    
    <td align="right">
      51935
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      12/6/2011 14:00
    </td>
    
    <td align="right">
      41933
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      12/11/2011 17:00
    </td>
    
    <td align="right">
      38003
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      1/8/2012 18:00
    </td>
    
    <td align="right">
      230296
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      2/2/2012 15:00
    </td>
    
    <td align="right">
      65177
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      2/28/2012 8:00
    </td>
    
    <td align="right">
      786658
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      3/1/2012 8:00
    </td>
    
    <td align="right">
      47005
    </td>
  </tr>
  
  <tr>
    <td align="left" height="20">
      3/1/2012 9:00
    </td>
    
    <td align="right">
      149672
    </td>
  </tr>
</table>

&nbsp;

 [1]: https://blog.torproject.org/blog/run-tor-bridge-amazon-cloud
 [2]: https://www.torproject.org/docs/bridges