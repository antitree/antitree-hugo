---
title: WRT OSINT and APIs
author: antitree
type: post
date: 2013-06-22T02:44:16+00:00
url: /2013/06/21/osint_apis/
categories:
  - Uncategorized

---
Partial rant, partial useful blog post &#8212; I&#8217;m noticing that a lot of the &#8220;new&#8221; **APIs for sites are starting to restrict access to content** either by putting limits on content either by controlling how much of the data you&#8217;re allowed to access, or by not including the ability to access a certain amount of data over the API at all. This is different from a few years ago where sites like Twitter, would let you collect all the tweets from a user without issue. Maybe they&#8217;re being more privacy conscious (lulz) or maybe they want to charge a premium for this type of access, I don&#8217;t know.

One for example is Google Latitude. When I&#8217;m friends with someone, I can access their location. They have shared their location with me and have me as a trusted person. But going through the official Latitude API, you&#8217;re specifically blocked from collecting any kind of user&#8217;s private information. **This sucks. I&#8217;ll be honest, I don&#8217;t have any good uses for stalking someone on Latitude but I think it&#8217;s funny to be able to track someone and run a report on where they&#8217;ve been over the past couple of days.**

The newest &#8220;privacy aware&#8221; API is twitter. **Their 1.1 API has been out for a while, but they recently blocked access to 1.0 (breaking all my scripts)** which means that you no longer are able to easily collect tweets and other user information. That also means that those RSS/ATOM feeds you used to keep track of a person are gone. You now must have a Twitter client to access Twitter in some usable fashion.

# OAuth Hates Scripters

To add to this situation, OAuth2 (compared to it&#8217;s predecessor)requires users to make a website interaction in order to collect the token and secret values meaning that scripts are annoying. We can get around this in a couple of ways, my favorite of which is using [FoAuth.org][1]. All this service does is facilitate the web requests necessary to  collect the OAuth values, and store them. Then using your python script and the new popular [Requests library][2] we **can call FoAuth to get the info that we need and proxy the requests to the API we&#8217;re using**. As designed, this has an expiration but it&#8217;s much easier to go back to FoAuth and re-authenticate a token rather than doing it for all of your scripts. Here&#8217;s an example from their main page:

<pre class="lang:default decode:true">import requests
auth = 'email@example.com', 'password'
data = {'status': "Just signed up with https://foauth.org/ and it's awesome! Thanks @Gulopine!"}
requests.post('https://foauth.org/api.twitter.com/1.1/statuses/update.json', data=data, auth=auth)
r = requests.get('https://foauth.org/api.twitter.com/1.1/statuses/user_timeline.json', auth=auth)
print(r.json()[0]['text'])</pre>

&nbsp;

# Twitter 1.1 API Example

Anyways, here&#8217;s something potentially useful amid my rant. The Twitter 1.1 API uses Oauth to make requests and then gets rid of the whole [pagination idea][3] that was in the previous version, and relies on this &#8220;max\_id&#8221; value. Basically, max\_id is where you want to start collecting tweets from. So if I want collect all of a user&#8217;s tweets, I can collect the first 200, find the last one that I pulled, and make a request starting at that last one, looping until there are no more (or no more are given out).

Here&#8217;s how that looks:

<pre class="height-set:true lang:python decode:true">#!/usr/bin/python
#
# Author: Antitree
# Description: Example of using the new Twitter 1.1 API to 
#  collect all the tweets from a user. 
#
# Derived from tsileo
# https://gist.github.com/tsileo/4637864/raw/9ea056ffbe5bb88705e95b786332ae4c0fd7554c/mytweets.py
#

import requests
import sqlite3, sys
from requests_oauthlib import OAuth1

# Go to https://dev.twitter.com/apps and create a new application
# Paste in your information here
CONSUMER_KEY = ''
CONSUMER_SECRET = ''

#Fill this in with the keys you receive after the first run
OAUTH_TOKEN = ""
OAUTH_TOKEN_SECRET = ""   

OAUTH_TOKEN = ""
OAUTH_TOKEN_SECRET = ""

################################
if len(sys.argv) is 2:  
    SCREENNAME = sys.argv[1]
else: SCREENNAME = "rossja" 

AUTH = ''
REQUEST_TOKEN_URL = "https://api.twitter.com/oauth/request_token"
AUTHORIZE_URL = "https://api.twitter.com/oauth/authorize?oauth_token="
ACCESS_TOKEN_URL = "https://api.twitter.com/oauth/access_token"

def setup_oauth():
    # Request token
    oauth = OAuth1(CONSUMER_KEY, client_secret=CONSUMER_SECRET)
    r = requests.post(url=REQUEST_TOKEN_URL, auth=oauth)
    credentials = parse_qs(r.content)

    resource_owner_key = credentials.get('oauth_token')[0]
    resource_owner_secret = credentials.get('oauth_token_secret')[0]

    # Authorize
    authorize_url = AUTHORIZE_URL + resource_owner_key
    print 'Please go here and authorize: ' + authorize_url

    verifier = raw_input('Please input the verifier: ')
    oauth = OAuth1(CONSUMER_KEY,
                   client_secret=CONSUMER_SECRET,
                   resource_owner_key=resource_owner_key,
                   resource_owner_secret=resource_owner_secret,
                   verifier=verifier)

    # Finally, Obtain the Access Token
    r = requests.post(url=ACCESS_TOKEN_URL, auth=oauth)
    credentials = parse_qs(r.content)
    token = credentials.get('oauth_token')[0]
    secret = credentials.get('oauth_token_secret')[0]

    return token, secret

def get_oauth():
    oauth = OAuth1(CONSUMER_KEY,
                client_secret=CONSUMER_SECRET,
                resource_owner_key=OAUTH_TOKEN,
                resource_owner_secret=OAUTH_TOKEN_SECRET)
    return oauth

def get_all_tweets(screenname, auth=AUTH):

    count = 200 #Max for the 1.1 API is 200 per request
    tweets = []

    #The max_id value sets from where to start collecting tweets.
    # e.g. 1234 means that you would get tweets older than 1234
    maxstr = "&max_id="
    lastid = ""
    maxid = ""  

    while True:
        # Call the API and collect the response as a JSON response
        r = requests.get(
            url="https://api.twitter.com/1.1/statuses/user_timeline.json?screen_name=%s&include_rts=1&count=%s%s" 
            % (screenname, count, maxid, ), auth=AUTH, ).json()
        tweets += r
        lastid = r[len(r)-1]["id"]
        maxid = maxstr+str(lastid)      # update the Max_Id value

        if len(r) is 1:                 # You found all the tweets
            break

        print("Collecting next set of tweets starting at %s " % lastid)

    print("Collected %s tweets from %s" % (len(tweets), SCREENNAME))
    return tweets

def store_tweets(tweets):
    con = sqlite3.connect('tweet.db')
    cur = con.cursor()
    cur.execute('CREATE TABLE IF NOT EXISTS '+ SCREENNAME +'  (id text, tweet text, date text)')
    for tweet in tweets:
        cur.execute('INSERT OR REPLACE INTO '+ SCREENNAME +' VALUES(?,?,?)', (tweet['id'], tweet['text'], tweet['created_at']))
    cur.close()
    con.commit()

if __name__ == "__main__":
    #Setup OAuth if we haven't already
    if not OAUTH_TOKEN:
        token, secret = setup_oauth()
        print "OAUTH_TOKEN: " + token
        print "OAUTH_TOKEN_SECRET: " + secret
        print
    else:
        AUTH = get_oauth()
        tweets = get_all_tweets("antitree")
        store_tweets(tweets)</pre>

<https://gist.github.com/antitree/5835529>

The problem with the above is that the Twitter API doesn&#8217;t give you \_all\_ of the tweets. Just whatever they feel like. Usually that&#8217;s a large amount (over 1000) but for users with lots-o-tweets, you&#8217;ll just hit an (AFAIK) arbitrary brick wall.

# In Conclusion, screw you apis

IANADev, so to me, APIs are a polite way of accessing data but if we keep getting blocked, we can go back in time and collect the data in other ways. From an OSINT perspective, we would like to gather this content for whatever legitimate and illegitimate purposes that we want. I&#8217;ll be spending my time before Defcon updating whatever tools I have to make sure they&#8217;re not going to suddenly stop working.

Also to note, **[Recon-ng][4], a great tool for recon/OSINT/whatever, has had support for the 1.1 API** for a while now which leads me to continue to believe it&#8217;s worth porting my tools into rather than trying to roll my own.

&nbsp;

 [1]: https://foauth.org/
 [2]: python-requests.org
 [3]: https://dev.twitter.com/discussions/3809
 [4]: https://bitbucket.org/LaNMaSteR53/recon-ng/wiki/Home