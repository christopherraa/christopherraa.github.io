---
layout: post
title: "Mantis and Subversion-integration"
permalink: 2010-03-17-mantis-and-subversion-integration
---
The last couple of years I’ve been using [Mantis](http://www.mantisbt.org) for issue-tracking and have been really pleased with it. It’s simple, does what it’s supposed to do and is simple enough that my customers can be given access and gain some feeling with what’s happening in the projects I’m running for them.

I have also been using [Subversion](http://subversion.apache.org/) for version control. I’m happy with that as well (was a nice step up from CVS). A very non-steep learning curve makes it a very nice tool even for people who have never user VC systems before. And yes, some of you will probably comment about how I should be using Git instead. To you I have this to say; git have it’s uses and is well suited for some things. For me and the way my business is structured though, Subversion is a better tool. But, that was not what this post was supposed to be about.

Ever since I started using Mantis I wanted to integrate it with Subversion so that when I write “Fixed issue #1234. A good description of whatever bug I fixed / feature I implemented.” then the corresponding issue in Mantis will be marked as ‘fixed’ automatically. I did that first in the 1.1.x-series of Mantis-releases and it worked well. By that I mean that once you had hacked on Mantis enough it would actually do what you wanted and all was good.

With the release of 1.2 the team behind Mantis gave us a plugin-system and we saw the emergence of a multitude of plugins for email-handling, extending various forms in Mantis, providing links to wiki-engines and integration with VC systems. In the latter category you find the [Mantis Source Integration Plugin](http://git.mantisforge.org/w/source-integration.git) by John Reese. It’s a nice piece of work that makes it a LOT easier to get a link between Mantis and various VC systems.

If you want to get this set up then you should have a look at [this post](http://www.unitz.com/u-notez/2009/10/subversion-svn-integration-mantisbt/) by Chris Dornfeld in addition to [this post](http://leetcode.net/blog/2009/01/integrating-git-svn-with-mantisbt/) by John Reese. Just be mindful that the screenshots there are somewhat outdate. The plugin-page and configration looks a little different today, but it’s nothing you won’t figure out if you just use your head a little.

So, thanks to Chris and John for this.
