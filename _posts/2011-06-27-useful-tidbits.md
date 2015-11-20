---
layout: post
title: "Useful tidbits"
permalink: 2011-06-27-useful-tidbits
---
During my day to day work I often stumble upon problems I’ve not encountered before. Usually I find solutions quickly, either by thrawling manual pages (oh ‘man’ how I love thee), [StackOverflow](http://stackoverflow.com) or [Google](http://google.com). When I discover good solutions I think to myself “must remember to share this with someone”, and then I forget all about it. This post is my attempt at rectifying that.

The things I will present are fairly trivial in themselves, and may very well be available through documentation, but have some aspect that I find worth noting. We’ll start off easy.

Today I needed to cancel one of the replications that were running between two [couchdb](http://couchdb.apache.org/)-instances. The manual tells you that you can restart the couchdb server, since that clears the list of replication jobs. I wanted to leave the other replications running and just kill the one I really needed to die.

If you read the [documentation on replication](http://wiki.apache.org/couchdb/Replication) you will learn how easy it is to do it using curl.

|1 2 3 4 5|curl \ -X POST \ -H 'Content-Type: application/json' \ -d ' { "source":"mydb","target":"http://mytargethost.com: 5984 /mydb/", "cancel": true } ' \ http://mysourcehost.com: 5984 /_replicate|

The only problem is that this will not do anything if it is set up to be a continuous replication. To cancel a running continuous replication you have to explicity tell CouchDB that “yeah, I know what I am doing, I really do want to cancel thins continuous replication”. Like so;

|1 2 3 4 5|curl \ -X POST \ -H 'Content-Type: application/json' \ -d ' { "source":"mydb","target":"http://mytargethost.com: 5984 /mydb/", "continuous":true, "cancel": true } ' \ http://mysourcehost.com: 5984 /_replicate|

Do you see the difference? I added “continuous”:true to the json structure. That’s really all there is to it!

Stay tuned for more of these. I’ll try to update on a day to day basis.
