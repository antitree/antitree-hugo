---
title: Raspberry Bridge Project
author: antitree
type: post
date: 2014-07-13T17:50:34+00:00
url: /2014/07/13/raspberry-bridge-project/
categories:
  - Hardware
  - Raspberry Pi
  - Tor

---
Over at [rbb.antitree.com][1], you&#8217;ll see the details of a new project of mine: To build a Raspberry Pi environment to make it easy for anyone to run a Tor Bridge node. The goal here has been to release an RBP image that is minimalist (in terms of storage consumption as well as resource consumption) and provides the necessary tools to run and maintain a Tor Bridge Node on a Raspberry Pi.

## Bridges

A reminder, a [Bridge Node][2] is a type of Tor node (like relay, exit, entry) that is a way of evading censorship to join the Tor Network. This is done by secretly hosting bridges that are not shared with the public so there&#8217;s no way for a censoring tool to merely block all Tor nodes. On top of that, an Obfuscated Bridge is one that further defends against various fingerprinting attacks of the Tor protocol. With an obfuscated bridge, communications from the client to the bridge appear to be benign traffic rather than Tor traffic.

## Challenge Installing Tor

It&#8217;s odd how less-than-simple the process of running a relay on a Pi is. If you want to run a relay on a RBP, some sites will merely say install Rasbpian and run apt-get install Tor. The problem with this is that the Debian repos are very far behind from the latest version of Tor (like at least one major revision behind). The logical conclusion would be to use the Tor Project&#8217;s debian repo&#8217;s then. The problem here is that there are no repos for Rasbperry Pi&#8217;s ARM architecture. One solution was to use something similar to the Launchpad PPA hosting that lets you run a simple repo to deliver a .deb package. But launchpad does not support ARM architecture (and doesn&#8217;t seem to plan to do so in the near future).

So the result is I&#8217;ve built a [github repo][3] that hosts the Tor .deb packages for the latest stable release. It&#8217;s not pretty, but it does the job and I know that it will work well. That was the first piece of the puzzle.

## Host Hardening

The Raspberry Pi images out there are designed for people that want to learn programming in Scratch and play with GPIO pins for some kind of maker project. They&#8217;re not ideal for providing a secure operating environment. So I built a Debian-based image from the ground up, with the latest packages and only the required packages. I&#8217;ve customized the image to not log anything across reboots (mounting /var/log as a tmpfs). You can read most of the [design of the OS here][4].

I&#8217;ve also secured SSH (which many of the Raspberry Pi images don&#8217;t do) by autogenerating SSH keys the first time it&#8217;s boot. The alternative is to ship an image that has the same SSH keys allowing MITM attacks. Again, these images are designed for makers.

## Torpi-config

The part I spent the most time on, and is hopefully the most useful, is I took the structure of the raspi-config tool that is shipped with Raspbian, and I convirted it into a Tor configuration tool. This will give you a text-based wizard to guide users through configuring Tor, keeping obfsproxy up-to-date and perform basic systems administration on the device.

[<img class="alignnone size-full wp-image-793" src="/wp-content/uploads/2014/07/screen11.png" alt="screen1[1]" width="641" height="411" />][5]

## Roadmap

It&#8217;s fully functional but there are a lot of things I&#8217;d like to improve upon. I&#8217;ve released it to solicit feedback and see how much more effort is necessary to get it where I want. Here are some of the other items on the roadmap:

  * Add the ability to update Tor to the latest stable release over github (securely)
  * Improve torpi-config to cover other use cases like configuring WiFi or a hidden service
  * Print out the specific ports that need to be forwarded through the router for the obfuscated bridge
  * Clean up some of the OS configuration stuff

&nbsp;

&nbsp;

 [1]: http://rbb.antitree.com
 [2]: https://www.torproject.org/docs/bridges
 [3]: https://github.com/antitree/tor-deb-raspberry-pi
 [4]: https://github.com/antitree/tor-raspberry-pi-build-scripts/blob/master/DESIGN.md
 [5]: /wp-content/uploads/2014/07/screen11.png