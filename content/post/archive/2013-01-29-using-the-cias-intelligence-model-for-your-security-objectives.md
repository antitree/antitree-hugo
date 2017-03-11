---
title: Using The CIA’s Intelligence Model For Your Security Objectives
author: antitree
type: post
date: 2013-01-29T12:11:08+00:00
url: /2013/01/29/using-the-cias-intelligence-model-for-your-security-objectives/
categories:
  - Intelligence

---
I&#8217;ve been putting some time into trying to improve my intelligence gathering capabilities. Normally we would call this recon during a pen test or OSINT gathering. But I&#8217;ve been thinking about it from the perspective of the CIA who refer to it as intelligence gathering. The ideas are basically same: **collect information that provides you with some kind of insight into a target**.

For a pen test, I want to know information about the subject I&#8217;m testing. Maybe it&#8217;s network information, or job openings, or list of employees, all this type of data can be used during later phases of the assessment. For your organization, you may want to know when Anon is going to be launching an attack on your network or an employee who is leaking company secrets on her Facebook account.

# OSINT Meets OPSEC

For the CIA, intel operations are part of operational security. The intel may tell you when future attacks are planned, secret ways terrorist organizations are communicating, or weaknesses in your adversaries. These same types of operations can be applied into your own OPSEC model: Looking for discussion about future attacks on your organization, useful  information about your competitors that was accidentally leaked, potential vulnerabilities in your own systems that become publicly available.

This is what the cycle look like in the most generic form. There&#8217;s a lot of explanation that has to go into each phase but I think you can interpret each however you&#8217;d like.

[<img class="aligncenter size-medium wp-image-473" alt="intel_cycle" src="http://www.antitree.com/wp-content/uploads/2013/01/intel_cycle-296x300.png" width="296" height="300" />][1]

This cycle has many [different versions][2]. It seems like different governments interpret it in different ways but they all basically stem from this image above. People have also been applying the intelligence lifecycle to APT (yes.. I said it&#8230;) because it directly applies to targeted network attacks. Here&#8217;s a good one from a hacker organization called &#8220;Dell&#8221;:

[<img class="aligncenter" alt="" src="http://en.community.dell.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-46-04/7711.Advanced_5F00_Persistent_5F00_Threat_5F002D005F00_APT_5F002D005F00_Lifecycle.png" width="350" height="348" />][3]

# The CIA and You?

The Dell image is cute, but is meant to only highlight a small portion of the potential sources that the CIA documents. But in general, some books say there are four primary sources of intelligence:

  1. <span style="line-height: 13px;">HUMINT: Information collected from a human source</span>
  2. TECHINT: Information collected by technical means (APT OMG!)
  3. OSINT: Open source intelligence gathering
  4. Direct Action: Hiring an effing milita to take the data.

This is from the CIA&#8217;s point-of-view so I&#8217;m not suggesting that people should go and steal intelligence from your friends by gun point, or hacking into their laptops, nor am I suggesting looking for human sources of intelligence to turn into spies for you. I&#8217;m trying to highlight **a model of intel gathering that may improve your skills and capabilities** especially when working in groups. Red-teaming for example**. **

I also want to point out that whether it&#8217;s the CIA, malware writers, APT-OMGZ! hackers, or corporate spies, **the same model basically applies to any types of people with similar goals**. Target, collect, process, analyse, disseminate, repeat.

While I&#8217;m not talking out-of-my-ass on the subject, I admit I have a lot to learn especially compared to those that are in the intelligence community now. I&#8217;ll be giving a presentation about the subject at the next [Rochester 2600][4] meeting this week.

&nbsp;

 [1]: http://www.antitree.com/wp-content/uploads/2013/01/intel_cycle.png
 [2]: https://www.google.com/search?q=intelligence+cycle&hl=en&tbo=d&source=lnms&tbm=isch&sa=X&ei=ersCUYkEyODRAcPUgegM&ved=0CAoQ_AUoAA&biw=1920&bih=936
 [3]: http://en.community.dell.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-46-04/7711.Advanced_5F00_Persistent_5F00_Threat_5F002D005F00_APT_5F002D005F00_Lifecycle.png
 [4]: http://www.rochester2600.com