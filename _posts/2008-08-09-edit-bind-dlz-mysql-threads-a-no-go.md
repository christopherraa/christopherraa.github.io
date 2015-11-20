---
layout: post
title: "Edit: [Bind-DLZ + MySQL + threads a no-go}"
permalink: 2008-08-09-edit-bind-dlz-mysql-threads-a-no-go
---
Recently I was contacted by Marc from [oriented.net web hosting](http://oriented.net) due to him having a problem with Bind-DLZ and MySQL similar to one I had a while back. He had found a post I submittet to a Bind-DLZ mailinglist and wondered if I had managed to resolve the problems. After some correspondence back and forth he was able to solve it on Debian Etch. I’m posting (with permission) the solution that worked for him. This is his work, I’m merely reposting.

Default MySQL client Debian packages (libmysqlclient15*) are compiled with threads enabled (–enable-thread-safe-client configure parameter). Unfortunately BIND DLZ with MySQL driver requires a non-threaded MySQL client.

There are two solutions to this: 1) change the compilation parameters of the Debian package and recompile this one 2) compile MySQL from the original source of mysql.com. I will go for option 2) as I don’t like the idea of messing up with official OS packages (what will happen during a package upgrade?).

Here are the commands I used to compile a non-threaded MySQL form the source:

|1 2 3 4|./configure --prefix=/opt/mysql-client --without-server --disable-threads \ --without-pthread make make install|

Note that I do not compile the server part here, as I am just interested in the client library. Once MySQL compiled and installed in /opt/mysql- client, I can start compiling BIND DLZ specifying the right path for the freshly compiled non-threaded MySQL client libraries in –with-dlz- mysql like this:

|1 2 3 4 5 6|./configure --prefix=/opt/bind-dlz --sysconfdir=/etc/bind-dlz \ --localstatedir=/var/run/bind --disable-threads --with-libtool \ --enable-shared --enable-static --with-openssl=/usr --with-gnu-ld \ --enable-ipv6 --with-dlz-mysql=/opt/mysql-client make make install|

This configuration was tested on Debian GNU/Linux 4.0r4 amd64. Having the non-threaded MySQL client library installed in /opt/mysql-client allows you to still have the threaded MySQL client packages installed on your OS. So both versions of the libraries are available on your system.
