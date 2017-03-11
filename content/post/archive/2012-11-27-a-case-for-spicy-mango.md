---
title: A Case For Spicy Mango
author: antitree
type: post
date: 2012-11-27T12:00:27+00:00
url: /2012/11/27/a-case-for-spicy-mango/
categories:
  - OSINT
  - Python
  - Tor

---
[Spicy Mango][1] is a project that Chris Centore started and presented on at Derbycon this year. It&#8217;s difficult to describe completely but in essence, it is an intelligence collection and analysis engine that helps you parse large amounts of data to extract items of interest. For example, say that you wanted to keep track of your nym and how it was used on the net. You could do something like a Google Alert that sends you an email every time &#8220;AntiTree&#8221; appears in the search engine. With Spicy Mango, you can search multiple sources (such as forums, blogs, news outlets) for relevant data and then carve out what is actionable. You probably don&#8217;t want to see Tweets that you sent yourself but you might want to see them referencing your account.

# Overview of Spicy Mango

Here&#8217;s a quick overview of how the framework is setup:

  * Modules are used to collect relevant information. Some examples of modules right now are an RSS reader, IRC client, Facebook and Twitter, scraper.
  * The data collected from these modules is saved into a database
  * The database provides the back-end to a web interface
  * The web interface controls how to present the data either High, Medium, or Low relevancy. This is done by searching for keywords in the database and applying a weight based on that keyword.

<div>
  Some screenshots:
</div>

<div>
</div>

<div>
</div>

# Maltego It Is Not

I&#8217;m not going to say that Spicy Mango is an amazing tool that fits into every intel gathering/recon/OSINT job you can think of. In fact, in many ways it starts overlapping with an already mature tool, Maltego. **The latest version of Maltego supports &#8220;Machines&#8221; which is in someways the same idea as Spicy Mango**. These machines are a recurring query for live data such as Tweets, Facebook posts, etc and is then collected in the beautiful Maltego interface that tries to visualize relationships between different pieces of intel. Very cool. But not really what I&#8217;m personally looking for.

# The Best Case Scenarios

At its most usefulness, **Spicy Mango would be an intel gathering tool that collects large pools of information from obscure locations on the net, and cuts down on the amount of time needed to find actionable intelligence.** It could help be an operation security tool to help notify you of upcoming threats that were discussed over IRC. It could be a persistent stalking machine that keeps track of your friends.

# WHY?

The &#8220;But, why?&#8221; question is the most common one I get when talking with my friends. First, I think that there is not an open-source tool today that aims to do what Spicy Mango tries. In fact, there are a bunch of secretive tools used for operation security and OSINT but we don&#8217;t know about them and they&#8217;re often a very custom design. Mostly because, **the first rule of OPSEC is that you don&#8217;t talk about OPSEC**. 

Secondly, hackers are usually pretty proud about their doxing skills. &#8220;I can find your real name and home address in 15 minutes!&#8221; Usually they&#8217;re not wrong but their skills are based on tools that they&#8217;ve developed themselves otherwise they&#8217;re just using some site that does the work for them. **I would find it interesting if the playing field was leveled so that every person had the same tools to stalk someone**. In that case, real skills would have to be developed to excel passed the baseline.

Lastly, (or primarily) it&#8217;s fun to hack on. The modules are simple to develop and the code is straight-forward.

# Future Opportunities

Since I started to help develop this framework, I&#8217;ve thought of some improvements it that would help take it from just a collection engine, to a more serious intelligence tool. I can see an advanced analysis engine that would take it above just keyword searches to support a modular framework in the same way that the collection phase works. I&#8217;ve been working on supporting **natural language processing to help be able to support n-gram structured searches and implementing spam style text analysis to automatically strip out useless information**. This is all based on my goal to better normalize the content that is collected.

 [1]: http://code.google.com/p/spicymango/