---
layout: post
title: "Fedora 10 + KDE + missing screensavers"
permalink: 2008-12-08-fedora-10-kde-missing-screensavers
---
Today I made another interesting discovery in Fedora 10. My (somewhat) vanilla install did not present me with any other screensavers than the one called ‘Blank screen’ or whatever. A case of packages for screensavers not being installed? I thought so and went and made sure that the following packages got registered in the system:

|1 2 3 4 5 6 7 8|xscreensaver xscreensaver-base xscreensaver-extras xscreensaver-gl-base xscreensaver-gl-extras rss-glx rss-glx-kde rss-glx-xscreensaver|

Still no luck. Perhaps you have to log out and in again? Tried that, no luck. Some digging around the net revealed that you have to do a simple

|1|yum install kdeartwork-extras|

to get the screensavers. Go figure.

I hope this will help other people in need of something to enjoy while code is compiling (or the spouse is having another fit).
