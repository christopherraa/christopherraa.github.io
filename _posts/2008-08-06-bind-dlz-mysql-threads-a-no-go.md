---
layout: post
title: "Bind-DLZ + MySQL + threads a no-go"
permalink: 2008-08-06-bind-dlz-mysql-threads-a-no-go
---
I recently got a question about why Bind-DLZ start behaving oddly together with the MySQL-driver from time to time (or rather from installation to installation). The problem is this: Bind starts just fine, and returns data as it should. However, after a given amount of time it just dies (yup, like your grandma, bless her soul) giving no errors to syslog (which surely grandma would!).

According to [this](http://bind-dlz.sourceforge.net/mysql_driver.html) page Bind-DLZ has an issue with threads when using the MySQL. The solutions is disabling threads (–disable-threads) when building, and starting Bind-DLZ with “-n 1″ just to be sure.

Oh, and make sure you are starting the right bind-executable. I had an issue with that when I installed it on one of the FreeBSD-systems here. Bind-DLZ was compiled and installed, but still dlz did not work. It turned out that I was trying to run the non-dlz over and over and over again. It was one of those “bash-your-head-repeatedly-into-the-rack-and-pray-that-your-skull-bursts-before-the-rack-does”-moments.

So, be a good little sysadmin and disable threads, or just switch to using another driver.
