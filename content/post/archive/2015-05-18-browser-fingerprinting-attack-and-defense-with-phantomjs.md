---
title: Browser fingerprinting attack and defense with PhantomJS
author: antitree
type: post
date: 2015-05-18T18:04:49+00:00
url: /2015/05/18/browser-fingerprinting-attack-and-defense-with-phantomjs/
categories:
  - Censorship
  - Intelligence
  - privacy

---
PhantomJS is a headless browser that when you use Selenium, turns into a powerful, scriptable tool for scraping or automated web testing in even JavaScript heavy applications. We&#8217;ve known that browsers are being fingerprinted and used for identifying individual visits on a website for a long time. This technology is a common feature of your web analytics tools. They want to know as much as possible about their users so why not collect identifying information.

# Attack (or active defense)

The scenario here is you, as a privacy conscious Internet user, have taken the various steps to hide your IP, maybe using Tor or a VPN service, and you&#8217;ve changed the default UserAgent string in your browser but by using your browser and visiting similar pages across different IP&#8217;s, the web site can track your activities even when your IP changes. Say for instance you go on Reddit and you have your same 5 subreddits that you look at. You switch IP&#8217;s all the time but because your browser is so individualistic, they can track your visits across multiple sessions.

Lots of interesting companies are jumping on this not only for web analytics, but from the security point of view. The now Juniper owned company, [Mykonos][1], built it&#8217;s business around this idea. It would fingerprint individual users, and if one of them launched an attack, they&#8217;d be able to track them across multiple sessions or IP&#8217;s by fingerprinting those browsers. They call this an active defense tactic because they are actively collecting information about you and defending the web application.

The best proof-of-concepts I know of are [BrowserSpy.dk][2] and the EFF&#8217;s [Panopticlick][3] project. These sites show what kind of passive information can be collected from your browser and used to connect you to an individual browsing session.

# Defense

The defense to these fingerprinting attacks are in a lot of cases to disable JavaScript. But as the [Tor Project][4] accepts, disabling JavaScript in itself is a fingerprintable property. The Tor Browser has been working on this problem for years; it&#8217;s a difficult game. If you look through BrowserSpy&#8217;s library of examples, there are common and tough to fight POC&#8217;s. One is to [read the fonts][5] installed on your computer. **If you&#8217;ve ever installed that custom cute font, it suddenly makes your browser exponentially more identifiable**. One of my favorites is the screen resolution; This doesn&#8217;t refer to window size which is separate, this means the resolution of your monitor or screen. Unfortunately, in the standard browser there&#8217;s no way to control this beyond running your system as a different resolution. You might say this isn&#8217;t that big of a deal because you&#8217;re running at 1980&#215;1080 but think about mobile devices which have [model-specific resolutions][6] that could tell an attacker the exact make and model of your phone.

# PhantomJS

There&#8217;s no fix. But like all fix-less things, it&#8217;s fun to at least try. I used PhantomJS in the past for automating interactions to web applications. You can write scripts for Selenium to automate all kinds of stuff like visiting a web page, clicking a button, and taking a screenshot of the result. Security Bods (as they&#8217;re calling them now) have been using it for years.

To create a simple web page screen scraper , it&#8217;s as easy as a few lines of Python. This ends up being pretty nice especially when your friends send you all kinds of malicious stuff to see if you&#8217;ll click it. 🙂 This is very simple in Selenium but I wanted to attempt to not look so script-y. The example below is how you would change the useragent string using Selenium:

<pre class="lang:python decode:true">ua = 'Mozilla/5.0 (Windows NT 6.1; rv:31.0) Gecko/20100101 Firefox/31.0'
        dc = dict(DesiredCapabilities.PHANTOMJS)
        dc["phantomjs.page.settings.userAgent"] = ua
        browser = webdriver.PhantomJS(
            "phantomjs",
            service_args=self.proxysettings,
            desired_capabilities=dc
        )
        browser.get("https://www.antitree.com")
</pre>

Playing around with this started bring up questions like: Since PhantomJS doesn&#8217;t in fact have a screen, what would my screen resolution be? [The answer is 1024&#215;768][7].

This arbitrarily assigned value is pretty great. That means we can replace this value with something else. It should be noted that even though you set this value to something different, it doesn&#8217;t affect the size of your window. To defend against being &#8220;Actively Defended&#8221; against, you can change the PhantomJS code and recompile.

<pre class="lang:c++ mark:6-10 decode:true" title="/src/qt/qtbase/src/plugins/platforms/phantom/phantomintegration.cpp">PhantomIntegration::PhantomIntegration()
{
    PhantomScreen *mPrimaryScreen = new PhantomScreen();

    // Simulate typical desktop screen
    int widths [5] = { 1024, 1920, 1366, 1280, 1600 };
    int heights [5] = { 768, 1080, 768, 1024, 900};
    int ranres = rand() % 5 + 1;
    int width = widths[ranres];
    int height = heights[ranres];

    int dpi = 72;
    qreal physicalWidth = width * 25.4 / dpi;
    qreal physicalHeight = height * 25.4 / dpi;
    mPrimaryScreen-&gt;mGeometry = QRect(0, 0, width, height);
    mPrimaryScreen-&gt;mPhysicalSize = QSizeF(physicalWidth, physicalHeight);

    mPrimaryScreen-&gt;mDepth = 32;
    mPrimaryScreen-&gt;mFormat = QImage::Format_ARGB32_Premultiplied;
</pre>

This will take a few extra screen resolutions every time a new webdriver browser is created. You can test it back at BrowserSpy.

Old:

[<img class=" wp-image-830 size-full aligncenter" src="/wp-content/uploads/2015/05/test4-e1431971190591.png" alt="" width="550" height="235" />][8]

New:[<img class=" wp-image-829 size-full aligncenter" src="/wp-content/uploads/2015/05/test3-e1431971237215.png" alt="test3" width="520" height="241" />][9]

# And so on&#8230;

And we&#8217;ve now spoofed a single fingerprintable value only another few thousand to go. In the end, is this better than scripting something like Firefox? Unknown. But the offer still stands that if someone at Juniper wants to provide me with a demo, I&#8217;d provide free feedback on how well it stands up to edge cases like me.

&nbsp;

 [1]: http://www.mykonossoftware.com/
 [2]: http://browserspy.dk/
 [3]: https://panopticlick.eff.org/
 [4]: https://www.torproject.org/docs/faq.html.en#TBBJavaScriptEnabled
 [5]: http://browserspy.dk/fonts-flash.php
 [6]: http://www.emirweb.com/ScreenDeviceStatistics.php
 [7]: https://github.com/ariya/phantomjs/blob/7317724723639932f79c211ac40f5ca06f4d9e1a/src/qt/qtbase/src/plugins/platforms/phantom/phantomintegration.cpp#L60
 [8]: /wp-content/uploads/2015/05/test4-e1431971190591.png
 [9]: /wp-content/uploads/2015/05/test3.png