---
layout: post
title: "FreeBSD 6.2 + Apache 2.2 + PHP5 + php5-recode = crash"
permalink: 2008-02-29-freebsd-62-apache-22-php5-convertersphp5-recode-silent-crash
---
Today I set out to configure another simple jail, and ran into some problems when doing the usual ‘cd /usr/local/ports/lang/php5-extensions && make install’.

I had chosen quite a few extensions to install, and they were installed without any drama. The problem presented itself when starting the webserver. Apache would load the php5-module, then silently die, leaving no clues that I could find as to what had caused it. Since the only change I had made since the working install was the install of php5-extensions, I started bugtracking there.

First I deinstalled php5-extensions and all ports installed by that meta-port. Then I ran these commands in sequence over and over again until I found the guilty party:

|1 2 3 4 5 6|make config -- Added one more option each time make install -- Installed /usr/local/bin/etc/rc.d/apache22 start && \ -- Started apache top && \ -- Observed if httpd stayed up /usr/local/bin/etc/rc.d/apache22 stop -- Stopped the httpd-daemon make deinstall -- Deinstalled|

All went well until RECODE-support was enabled. Then apache failed silently. Ok, so far so good. Now what next?

Some googling later and I had learned that the order in which the php5-extensions are loaded actually matter. I located the extensions.ini-file ( /usr/local/etc/php/extensions.ini ), and changed it so that recode preceeded ‘mysql’, ‘pspell’, ’sockets’ and ‘imap’ in the list. The extensions.ini-file that worked for me is available here: [extensions.ini](http://christopher.rasch-olsen.no/wp-content/uploads/2008/02/extensions.ini)

Please keep in mind that extensions.ini is recreated when you do a portupgrade of php5-extensions. A solution in that case can be to run a shellscript after portupgrade that reorders the file according to your needs. You can find information about one such script here: [pingle.org](http://www.pingle.org/2007/09/22/php-crashes-extensions-workaround/)
