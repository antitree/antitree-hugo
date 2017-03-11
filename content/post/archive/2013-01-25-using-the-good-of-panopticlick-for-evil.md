---
title: Using the Good Of Panopticlick For Evil
author: antitree
type: post
date: 2013-01-25T16:44:16+00:00
url: /2013/01/25/using-the-good-of-panopticlick-for-evil/
categories:
  - OPSEC
  - privacy
  - Tor

---
Browser fingerprint tactics, like the ones demonstrated in Panopticlick have been used by marketing and website analytic types for years. It&#8217;s how they track a user&#8217;s activities across domains. Just include their piece of JavaScript at the bottom of your page and poof, you&#8217;re able to track visitors in a variety of ways.

I don&#8217;t care much about using this technology for marketing, but I do care about using this type of activity for operational security purposes. Imagine using this technique as a counter-intelligence tactic. You don&#8217;t want to prevent someone from accessing information, but you do want to know who is doing it, especially if they have ill intentions in mind. IP addresses are adorable but hardly reliable when it comes to anyone that knows how to use a proxy, so using a fingerprint application, like Panopticlick, we can see who is visiting the site no matter what their locations appears to be.

Here&#8217;s a simple way of using Panopticlick&#8217;s JavaScript for your own purposes to gather fingerprint information about your browser. I&#8217;ll leave it up to you to figure out what you can do with this.

<pre class="lang:default decode:true">&lt;html&gt;&lt;head&gt;
&lt;script src="http://panopticlick.eff.org/resources/plugin-detect-0.6.3.js" type="text/javascript"&gt;&lt;/script&gt;
&lt;script&gt;
alert(identify_plugins());
&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1&gt;Panoopticlick Test&lt;/h1&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

# &#8220;More Worser&#8221;

Panopticlick&#8217;s informatino gathering techniques are very similar (see the same) as Browserspy except that they correlate the results to a dataset. If you really wanted to do all the browser fingerprinting without any of the reporting, you can take a look at the [BrowserSpy][1] code.

I&#8217;ve also worked on a technique years ago that attempts to verify your IP address using DNS. This was a pretty good technique especially for third party plugins like Flash and Java which were inconsistent when it comes to using proxies correctly. For more information about using DNS to extract an IP address and further gather information about a user, check out HD Moore&#8217;s now decommissioned [Decloak][2] project.

&nbsp;

 [1]: http://www.browserspy.dk
 [2]: http://www.decloak.net