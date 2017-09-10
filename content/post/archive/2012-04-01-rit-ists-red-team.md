---
title: RIT ISTS Red Team
author: antitree
type: post
date: 2012-04-01T20:15:14+00:00
url: /2012/04/01/rit-ists-red-team/
categories:
  - News
  - Rochester 2600

---
Here is a brain dump of what happened this weekend at [ISTS 9][1], SPARSA&#8217;s Information Security and Talent Search. A bunch of the people from 2600, [Raphael Mudge][2], [Punkrokk][3], Joe, Gerry, and others were part of the Red Team.

# Define:ISTS

The event worked like so:  There were 13 Blue Teams, groups competing in the event. Their job was to take the 5 servers that they were given, run specific services in order to get points, and, something a little different than other competitions, hack into other groups for points. If you do this, you will receive points that are tracked throughout the weekend. Finally, challenges were to be performed that were worth more points.

I really like this style for students compared to CCDC or other types that attempt to give competitors the simulation of running an enterprise network. These competitions will force competitors to stand up services, defend against attacks, and write up little reports that explain how the attackers got in and their remediation plan. This is a great simulation for real enterprise environments for students looking to get into a career as a systems administrator, **but ISTS gives you a chance to show off your security skills, even the offensive ones.**

# Day 0:

Friday, students fill up the GCCIS auditorium and the mood is really light. **Teams are wearing silly hats, the group that won last year is feeling pretty confident that they&#8217;ve earned some cred to be there again**. The red team is stalking in the corner, marking its prey. SPARSA goes over the rules, hands teams their packets, and lets them know how the next day goes. Each of the teams have the rest of the night to go home and build up their attack boxes as VM&#8217;s. That&#8217;s right, they&#8217;re allowed to bring their own VM&#8217;s, just to attack other players.

The next morning, we arrive at RIT to meet the red eyed SPARSA members who have been working through networking issues all night.

**A side note about the network this year: it was very well done**. This may seem like a silly thing to note but seriously, you have almost 100 people pummeling each other over the network, issues will occur. Compared to other years, there were very little issues. Last year we were out of a connection for a long time. This year there may have been a blip here and there, but it was quickly remedied.

[<img class="aligncenter size-full wp-image-163" title="DSC_8838" src="/wp-content/uploads/2012/04/DSC_8838.jpg" alt="" width="800" height="432" srcset="/wp-content/uploads/2012/04/DSC_8838.jpg 800w, /wp-content/uploads/2012/04/DSC_8838-300x162.jpg 300w, /wp-content/uploads/2012/04/DSC_8838-768x415.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" />][4]

The battle ground is RIT&#8217;s Innovation Center. If you&#8217;ve never seen this thing, it&#8217;s like something you&#8217;ve seen in Swordfish or the Matrix. The walls are all glass including partitions inside the space itself. **The Fish Bowl is slathered with power plugs, ports, projectors, and preposterously plush ammenities.**

# May The Odds Be Ever In Your Favor

Let me make sure I explain the insanity of the first 30 minutes. Blue teams walk in with their own VM&#8217;s loaded up with whatever scripts or automated attacks they want to launch. That&#8217;s every team, of 5 people, ready to kill the other 60 players, with all kinds of attacks. **And then there&#8217;s Raphael, prepped with his Armitage server, automated scripts, and a big smile on his face**.

If I were competing, I would have chosen a tactic for the first part of the event, just like The Hunger Games. If you&#8217;re going to battle ninja v ninja, you had better make sure your weapons and foo is strong or you&#8217;ll end up a bloody pile of  empty dreams. **If you&#8217;re relying on a strategic victory, you may decide to focus your energy on protecting yourself from others attacks; I call this the run-into-the-woods strategy**.

Let me also just say, that the Red Team had no special information related to the event&#8230; though we tried. In fact, in some ways, the Red Team is not necessary because all the other teams are already breaking into each other.

We let the students trickle into their stations and the battle begins. **We are just scanning subnets when Raphael announces &#8220;I have 10 shells!&#8221;** His scripts have automatically installed meterpreter sessions that are phoning home. His persistence scripts automatically make sure that we can come back to these accounts later. The rest of the red team is destroying boxes, the same as what the Blue Teams are doing to eachother.

Let&#8217;s be honest, the best part about being on Red Team is messing with the other teams. The Blue Teams all started with VNC open which made for some hilarious hacker watching. The first was one team that put all their attack scripts on their desktop. **This was actually pretty cool attack script which was a reverse shell that automatically tweeted a user&#8217;s password when the owned a box**. Follow ISTS Tweeter to see the fun they had.

[<img class="alignright size-medium wp-image-177" title="pdf" src="/wp-content/uploads/2012/04/pdf1-300x116.png" alt="" width="300" height="116" srcset="/wp-content/uploads/2012/04/pdf1-300x116.png 300w, /wp-content/uploads/2012/04/pdf1.png 477w" sizes="(max-width: 300px) 100vw, 300px" />][5]The other one was a group that decided to plug in a drive that contained a lot of personal information. Including a resume. **We passed the file to a nc listener that Justin Elze had running on his machine and printed it out to hand deliver to the group. One team member figured out how to break into a box and trick a hard drive into having a single sector**. No seriously. This is pretty awesome. The box was never heard from again. It couldn&#8217;t be reformatted. Raphael had some fun with one team&#8217;s SMTP server and a VNC payload. <del>I&#8217;m sure that video will show up somewhere soon.</del>

UPDATE 4/1/12 8pm: Video is up
  


# What&#8217;s Next

I usually come to the same conclusion after every year which is &#8220;damn, I wish I had planned for X.&#8221; **I think this year, the conclusion is that, &#8220;damn that was fun. Let&#8217;s do this more often.&#8221;**

Interlock has recently been donated a bunch of really good equipment. One of the servers has 20G of memory and lots of hard drive space. We&#8217;re going to be setting up a proper Warzone that will allow us to run these type of events when we want to. I&#8217;m going to rely on the other 2600 people to get something cool setup.

 [1]: http://ists.sparsa.org/
 [2]: http://www.hick.org/~raffi/
 [3]: http://twitter.com/#!/punkrokk
 [4]: /wp-content/uploads/2012/04/DSC_8838.jpg
 [5]: /wp-content/uploads/2012/04/pdf1.png