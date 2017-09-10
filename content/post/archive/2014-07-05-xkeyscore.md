---
title: XKeyScore
author: antitree
type: post
date: 2014-07-06T00:25:41+00:00
url: /2014/07/05/xkeyscore/
categories:
  - Intelligence
  - OSINT
  - privacy
  - Tor

---
If you&#8217;re like me, you&#8217;re probably getting inundated with posts about how the latest revelations show that NSA specifically tracks Tor users and the [privacy conscious][1]. I wanted to provide some perspective of how [XKeyscore][2] fits into an overall surveillance system before jumping out of our collective pants. As I&#8217;ve written about <a title="Using The CIA’s Intelligence Model For Your Security Objectives" href="/using-the-cias-intelligence-model-for-your-security-objectives/" target="_blank">before</a>, the Intelligence Lifecycle (something that the NSA and other Five Eyes know all to well) consists more-or-less of these key phases: Identify, Collect, Process, Analyze, and Disseminate. Some of us are a bit up-in-arms, about Tor users specifically being targeted by the NSA, and while that&#8217;s a pretty safe conclusion, I don&#8217;t think it takes into account what the full system is really doing.

XKeyscore is part of the &#8220;Collect&#8221; and &#8220;Process&#8221;  phases of the life cycle where in this case they are collecting your habits and correlating it to an IP address. Greenwald and others will show evidence that the NSA&#8217;s goal is to, as they say &#8220;collect it all&#8221; but this isn&#8217;t a literal turn of phrase. It&#8217;s true there is a broad collection net, but the NSA is not collecting everything about you. At least not yet.  As of right now, the NSA&#8217;s collection initiatives lean more towards collecting quantifiable properties which have the highest reward and the lowest storage cost. That&#8217;s not as sexy of a phrase to repeat throughout your <a href="http://www.theguardian.com/commentisfree/2013/jul/15/crux-nsa-collect-it-all" target="_blank">book tour </a>though.

<div align="center">
  <a href="/wp-content/uploads/2014/07/521642881.jpg"><img class="alignnone wp-image-765 size-medium" src="/wp-content/uploads/2014/07/521642881-300x225.jpg" alt="52164288[1]" width="300" height="225" /></a> OR <img class="alignnone wp-image-766 size-medium" src="/wp-content/uploads/2014/07/521643321-300x225.jpg" alt="52164332[1]" width="300" height="225" />
</div>

&nbsp;

The conclusion may be (and it&#8217;s an obvious one) what you&#8217;re seeing of XKeyscore is a tiny fraction of the overall picture. Yes they are paying attention to people that are privacy conscious, yes they are targeting Tor users, yes they are paying attention to people that visit the Tor web page. But as the name implies, **this may contribute to an overall &#8220;score&#8221; to make conclusions about whether you are a high value target or not.** What other online habits do you have that they may be paying attention to. Do you have a reddit account subscribed to /r/anarchy or some other subreddit they would consider extremist. Tor users aren&#8217;t that special, but this section of the code is a great way to get people nervous.

As someone who has worked on a [collection and analysis engine at one time][3], I can say that one of the first steps during the collection process is tagging useful information, and automatically removing useless information. In this case, tagging Tor users and dropping cat videos. It appears that XKeyscore is using a whitelist of properties to what they consider suspicious activity, which would then be passed on to the &#8220;Analysis&#8221; phase to help make automated conclusions. **The analysis phase is where you get to make predictive conclusions about the properties you have collected so far.**

[<img class="aligncenter size-medium wp-image-764" src="/wp-content/uploads/2014/07/intel_lifecycle_xkeyscore-296x300.png" alt="intel_lifecycle_xkeyscore" width="296" height="300" />][4]

Take the fact that your IP address uses Tor. Add it to a list of extremist subreddits you visit. Multiply it by the number of times you searched for the phrase &#8220;how to make a bomb&#8221; and now you&#8217;re thinking of what the analytics engine of the NSA would look like.

My point is this: **If you were the NSA, why wouldn&#8217;t &#8216;you target the privacy aware?** People doing &#8220;suspicious&#8221; (for some definition of the word) activities are going to use the same tools that a &#8220;normal&#8221; (some other definition) person would. We don&#8217;t have a good understanding of what happens to the information after it&#8217;s been gathered. We know that XKeyscore will log IP&#8217;s that have visited sites of interest or performed searches for &#8220;extremist&#8221; things like privacy tools. We know that there have been cases where someone&#8217;s online activities have been used in court cases. But can&#8217;t connect the dots.  XKeyscore is just the collection/processing phase and the analytic phase is what&#8217;s more important. I think the people of the Tor Project have a pretty decent perspective on this. Their responses have generally just re-iterated that this is exactly the threat model they&#8217;ve always planned for and they will keep working on ways to improve and protect its users.

&nbsp;

&nbsp;

 [1]: https://blog.torproject.org/blog/being-targeted-nsa
 [2]: http://en.wikipedia.org/wiki/XKeyscore
 [3]: https://localhost
 [4]: /wp-content/uploads/2014/07/intel_lifecycle_xkeyscore.png