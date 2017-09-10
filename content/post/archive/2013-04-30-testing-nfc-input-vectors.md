---
title: Testing NFC Input Vectors
author: antitree
type: post
date: 2013-04-30T09:02:36+00:00
url: /2013/04/30/testing-nfc-input-vectors/
categories:
  - Uncategorized

---
Can we agree that NFC is here to stay? Just about every mobile platform supports it, (I&#8217;m looking at you Apple) including simple feature phones [from][1] [way][2] [back][3] [when][4] . Let me just get to the good part: NFC input vectors for pen-testing. The scenario here is a mobile application that supports some kind of NFC exchange. Maybe it&#8217;s a Windows Phone 8 tag reader or something using Android Beam &#8212; whatever. The point is that the mobile app is receiving input from an outside source (the NFC tag), and we want to make sure it&#8217;s properly validating that input. Specifically, when an application reads in the NDEF (or proprietary) content from the NFC tag, how is it used by the application? What happens when we change this value to something unexpected? In an ideal world, it will catch the exception and stop trying to read the tag, but what about in the case of &#8220;less than ideal&#8221; programming.

# Tools

To get started we need something that can read and write NFC tags. Sorry, iPhone users, but the easiest way to do this as I see it is to use an Android device and a few choice apps:

## NXP NFC TagInfo

Does exactly what its name implies. It gives you info on an NFC tag. This includes any kind of ASCII characters inside of the NDEF storage container or a hex representation of the values if that&#8217;s your thing. This is step 1 when it comes to learning about the content that&#8217;s on an NFC tag. [Play store link][5].

## NXP NFC Tag Writer

NXP&#8217;s tag writer will read, write, and copy tags. When I say copy, I mean it will copy the NDEF format. That&#8217;s the content that&#8217;s normally on an NFC tag. Hard coded values like the UID can&#8217;t be changed (unless you know where to get [sketchy NFC tags][6] and even then you need a libNFC-based tool to interface with it). [Play store link][7].

## NFC Developer

This is where the fun happens. This app allows you to design just about any NDEF formatted NFC tag you want. The nice part of this is if there is an application implementing a weird custom format, you can create it. It&#8217;s made by Thomas Skjolberg who apparently has a whole workshop on the subject that gets you started with NFC on Android.

Used in partnership with the [ndefeditor.com][8] site, the app lets you generate just about any NFC tag you can think of and then record it to a tag. Or you can use the Eclipse Plugin  that does the same thing inside of Eclipse. Very useful.

Create a new tag in Eclipse by going to New>Other>NDEF File

[<img class="aligncenter" alt="blog1" src="/wp-content/uploads/2013/04/blog1.png" width="314" height="299" />][9]

Fill the file with whatever contents you want or whatever the application can handle. This may be a specific MIME type like below or a Android Application Resource (AAR)&#8230; or many other things for that matter.
  
[<img class="aligncenter" alt="blog5" src="/wp-content/uploads/2013/04/blog5.png" width="438" height="281" />][10]

&nbsp;

Once you&#8217;re done, it&#8217;ll create a QR code for you that you can scan with the NFC Developer application installed on your device.
  
[<img class="aligncenter" alt="blog3" src="/wp-content/uploads/2013/04/blog3-576x1024.png" width="346" height="614" />][11]

You&#8217;ll now be able to load your custom NDEF message onto an NFC tag of your choice.

[Link][12]

## Tags

If you&#8217;re using Android, you don&#8217;t necessarily need to write your content to physical tags. It&#8217;s possible to manually create intents that look like the device is receiving an NFC tag. But since we&#8217;re talking about testing any NFC function on \*any\* platform, you&#8217;ll need to pick up some NFC tags. The NFC protocol itself supports a &#8220;card emulation&#8221; mode where you could theoretically turn your Android phone into a simple NFC tag, but from what I understand, it&#8217;s either extremely hard to do or impossible right now because it&#8217;s based on the NFC secure element that is manufacturer specific. If someone wants to enlighten me on that, please feel free.

You&#8217;ll want a variety of tag types. The main difference you&#8217;ll be concerned about here is just the amount of storage. The Mifare 4K have a reasonably large storage capacity and can still deliver the data in the same way that a Mifare Classic (1K). Maybe there&#8217;s a situation where you&#8217;ll need a special tag type but I haven&#8217;t run into that yet. Either way, here&#8217;s a random [link to some tags][13].<em id="__mceDel"> </em>

# Bad Code:

Lets take a look at some example code for Android that we&#8217;re trying to exploit. This is a portion of code that is reading an NFC tag, and saving to a file name based on that input. You can see that the value of &#8220;strfile1&#8221; is whatever the first NDEF record is. What happens if that payload was something like &#8220;../databases/superimportantcontent.db&#8221;.  Even worse, the app looks at the second value of the NDEF record for the content to write to.

<pre>Parcelable[] rawMsgs = intent.getParcelableArrayExtra(
        NfcAdapter.EXTRA_NDEF_MESSAGES);
NdefMessage msg = (NdefMessage) rawMsgs[0];
String payload = new String(msg.getRecords()[0].getPayload());

String strfile1 = getApplicationContext().getFilesDir().getAbsolutePath() + payload ; //is this bad? :)
File f1 = new File(strfile1);
FileWriter filewriter = new FileWriter(f1);
BufferedWriter out = new BufferedWriter(filewriter);
out.write(msg.getRecords()[1].getPayload());
out.close();</pre>

Lets imagine that this app stores a textfile of SSH hosts to connect to. In this case, we could create a custom NFC tag that would have a first record of the path we want to access (&#8220;SSH.txt&#8221;) and the second record would be the values to put inside of this file (your malicious SSH MiTM proxy). Having a user read your custom tag would redirect their connections to you.

Happy hacking.

<p style="text-align: center;">
   <a href="/wp-content/uploads/2013/04/blog3.png"><br /> </a> <a href="/wp-content/uploads/2013/04/blog1.png"><br /> </a>
</p>

 [1]: http://www.engadget.com/2011/08/18/nokia-gifts-museum-of-london-with-nfc-tags-makes-you-tap-for-mo/ "from"
 [2]: http://www.nfcworld.com/nfc-phones-list/#museum "way"
 [3]: http://www.gsmarena.com/nokia_600-4118.php "back"
 [4]: http://en.wikipedia.org/wiki/Nokia_6131 "when"
 [5]: https://play.google.com/store/apps/details?id=com.nxp.taginfolite&hl=en
 [6]: http://via.me/-2gtpp82
 [7]: https://play.google.com/store/apps/details?id=com.nxp.nfc.tagwriter&hl=en
 [8]: http://ndefeditor.com/
 [9]: /wp-content/uploads/2013/04/blog1.png
 [10]: /wp-content/uploads/2013/04/blog5.png
 [11]: /wp-content/uploads/2013/04/blog3.png
 [12]: https://play.google.com/store/apps/details?id=com.antares.nfc&hl=en
 [13]: http://rapidnfc.com/cat/15/nfc_starter_packs