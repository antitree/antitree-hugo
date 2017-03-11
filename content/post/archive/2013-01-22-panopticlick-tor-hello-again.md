---
title: Panopticlick, Tor, Hello Again
author: antitree
type: post
date: 2013-01-22T10:26:32+00:00
url: /2013/01/22/panopticlick-tor-hello-again/
categories:
  - OSINT
  - privacy
  - Tor

---
[Panopticlick][1] is a project run by the EFF that highlights the privacy concerns related to being able to fingerprint your browser. It suddenly popped back up in [/r/netsec][2] like it was a  new project. **The site works by showing you the results of a full fledge browser fingerprint tool, letting you compare how similar or dissimilar you are to other visitors.** This is done in a variety of ways. By looking at the user agent, screen resolution, fonts installed, plugins installed, versions of those plugins, and much more. You can read the <a href="https://panopticlick.eff.org/browser-uniqueness.pdf" target="_blank">Panopticlick whitepaper</a> if you want to understand more about how it works.

# Hipster Tor: Privacy before it was cool

The issue was discussed years ago at Defcon XV where I first got interested in the project. They identified browser fingerprinting as concern that needed to be addressed in Tor. Their answer at the time was to use something they had just released called &#8220;TorButton.&#8221; TorButton, back in the day, was a Firefox plugin that when enabled, changed all the settings in your Firefox browser to stop leaking private information like those that Panopticlick checks.

TorButton ([Mike Perry][3]) soon realized that this was a loosing battle with Firefox who were trying to compete with sexy new browsers by adding in all kinds of automatic, privacy blind, features like live bookmarks. These things would just constantly query your bookmarks for updated content and had no way of reliably forwarding through a SOCKS proxy and anonymized, making it a major concern. This lead to the advent of the Tor Browser bundle which is a forked version Firefox, compiled specifically with privacy in mind, and the recommended way of using Tor today.

# Panopticlick v. Tor

Back to Panopticlick: Tor&#8217;s Browser bundle (along with integrated TorButton) tries to defend you against this type of attack. It changes the user agent to the most common one at the time, disables JavaScript completely, spoofs your timezone, and more. Take a look at the comparison between the Tor Browser bundle, Chrome, and Chrome for Android:

<div align="center">
  <table width="486" border="0" cellspacing="0" cellpadding="0">
    <col width="184" /> <col width="63" /> <col width="129" /> <col width="110" /> <tr>
      <td style="text-align: left" width="184" height="20">
        Browser Characteristic
      </td>
      
      <td style="text-align: right" width="63">
        Tor
      </td>
      
      <td style="text-align: right" width="129">
        Windows 7 Chrome
      </td>
      
      <td style="text-align: right" width="110">
        Android Chrome
      </td>
    </tr>
    
    <tr>
      <td height="20">
        User Agent
      </td>
      
      <td align="right">
        78.88
      </td>
      
      <td align="right">
        1489.11
      </td>
      
      <td align="right">
        36249.45
      </td>
    </tr>
    
    <tr>
      <td height="20">
        HTTP_ACCEPT Headers
      </td>
      
      <td align="right">
        31.66
      </td>
      
      <td align="right">
        12.76
      </td>
      
      <td align="right">
        12.76
      </td>
    </tr>
    
    <tr>
      <td height="20">
        Browser Plugin Details
      </td>
      
      <td align="right">
        25.89
      </td>
      
      <td align="right">
        2646146
      </td>
      
      <td align="right">
        25.89
      </td>
    </tr>
    
    <tr>
      <td height="20">
        Time Zone
      </td>
      
      <td align="right">
        21.63
      </td>
      
      <td align="right">
        11.04
      </td>
      
      <td align="right">
        11.04
      </td>
    </tr>
    
    <tr>
      <td height="20">
        Screen Size and Color Depth
      </td>
      
      <td align="right">
        46.78
      </td>
      
      <td align="right">
        46.78
      </td>
      
      <td align="right">
        7714.9
      </td>
    </tr>
    
    <tr>
      <td height="20">
        System Fonts
      </td>
      
      <td align="right">
        8.5
      </td>
      
      <td align="right">
        2646146
      </td>
      
      <td align="right">
        8.5
      </td>
    </tr>
    
    <tr>
      <td height="20">
        Are Cookies Enabled?
      </td>
      
      <td align="right">
        1.34
      </td>
      
      <td align="right">
        1.34
      </td>
      
      <td align="right">
        1.34
      </td>
    </tr>
    
    <tr>
      <td height="20">
        Limited supercookie test
      </td>
      
      <td align="right">
        8.91
      </td>
      
      <td align="right">
        2
      </td>
      
      <td align="right">
        2
      </td>
    </tr>
  </table>
  
  <p>
    Numbers based on 1 in x visitors have the same value as your browser
  </p>
</div>

# Feel safer? Don&#8217;t.

The EFF&#8217;s project has been really good at increasing the public understanding of the risks of browser fingerprint style attacks, but risks definitely remain. One of the nastier ones, which has yet to be fully addressed, has been only theorized until last year. **The scenario is that someone watching a user&#8217;s activities, can fingerprint their online activities.** A presentation at last year&#8217;s 28C3 highlighted this issue. In it, they discussed how a user will usually go to the same groups of websites pretty consistently: Reddit, Google News, Wikipedia. Those activities can be used as a fingerprint for your online identity. **Tor is coming up with an answer to this with their Moduler Transports initiative** which allows Tor users to customize the traffic footprint using plugins.

My next post will highlight how to use Panopticlick for some operational security measures. 🙂

 [1]: https://panopticlick.eff.org
 [2]: http://www.reddit.com/r/netsec
 [3]: http://www.fscked.org