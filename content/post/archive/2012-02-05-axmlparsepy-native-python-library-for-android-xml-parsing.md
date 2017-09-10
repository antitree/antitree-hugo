---
title: 'AXMLParsePY: Native Python Library for Android XML Parsing'
author: antitree
type: post
date: 2012-02-06T00:12:55+00:00
url: /2012/02/05/axmlparsepy-native-python-library-for-android-xml-parsing/
categories:
  - Android
  - AXMLParsePY
  - Manitree

---
I think it was less than a week after I announced my little Android Manifest auditor tool, [Manitree][1], that Anthony Desnos, the developer of Androguard, sent me a message in the tone of **&#8220;hey, why didn&#8217;t you use Androguard for that?**&#8221; If nothing else, why didn&#8217;t I use Andoguard&#8217;s native AXML converter?

**Andoguard is this immense Android app analysis project**. If you take a look at the first page, you may get overwhelmed pretty quickly. I hope Anthony doesn&#8217;t take this the wrong way because **it&#8217;s an impressive tool** when I&#8217;ve seen it working, and it&#8217;s great for all kinds of things besides malware analysis. For instance it can analyze apks, diff binary apps, visualize the flow of an app between classes &#8212; fun stuff. But for my dinky project, most of the work was focused on the AndroidManifest.xml file. But the simplest feature was most impressive to me: **a native python Android XML file format converter**. As of writing this, I&#8217;ve not seen someone publicly do this.

Mandatory technical background: The AndroidManifest.xml file is stored in a format called the **Android XML format or AXML**. This is an optimized binary format and not a lot of fun to look through. So tools like AXMLPrinter, <a href="http://forum.xda-developers.com/showthread.php?t=514412" target="_blank">AXMLPrinter2</a>, aapt, and <a href="http://code.google.com/p/android-apktool/" target="_blank">apktool </a>converted these files back to a standard XML format that it was originally created in. This format was created to link to the resources.arsc file without having to duplicate efforts. For instance instead of calling the name of a string value over and over in a Manifest, the resources.arsc file is linked to it so actually what you&#8217;ll see in the binary is the location of the value in this file.

For the reason above, this weekend, a few of us have started to extract Androguard&#8217;s AXML into a separate project that aims to be **a native python library for parsing AXML files**. It&#8217;s up on github and is still in progress but the goal is that it can be useful as a standalone python module without having to import all of Androguard. <https://github.com/antitree/AxmlParserPY>

Here&#8217;s a quick example that takes in AndroidManifest.xml in binary format and spits it out in xml:

<pre name="code" class="python">#!/usr/bin/python
import axmlprinter
from xml.dom import minidom
def main():
  ap = axmlprinter.AXMLPrinter(open('AndroidManifest.xml', 'rb').read())
  buff = minidom.parseString(ap.getBuff()).toxml()
  print(buff)

if __name__ == "__main__":
  print("Starting")
  main()
</pre>

 [1]: /projects/manitree/ "Manitree"