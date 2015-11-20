---
layout: post
title: "FreeBSD + [FireBird / PostgreSQL] in a jailed enviroment"
permalink: 2008-09-14-freebsd-firebird-postgresql-in-a-jailed-enviroment
---
Today I ran into an interesting problem. While building firebird2-client from ports I got this little message:

|1 2|Please do not build firebird as 'root' because this may cause conflicts with SysV semaphores of running services|

This was easily overcome with with this short flag (and yes, I got that from peeking in the makefile):

|1|make -DPACKAGE_BUILDING|

After the build had run for a little while I got this message:

|1 2 3 4 5 6 7|gmake [ 4 ]: Leaving directory `/var/ports/usr/ports/databases/ firebird2-client/work/Firebird-2.0.2.12964- 0 /gen' rm -f empty.fdb ../gen/firebird/bin/create_db empty.fdb operating system directive semget failed -Function not implemented gmake [ 3 ]: *** [ empty.fdb ] Error 254|

Ok? You kidding me ports-system? But I thought we were _friends_! After turning my back and preparing the .45 for some summary execution of a certain port, I thought to myself “Hey! Could it be semaphore-access or something?”. I fired up an ssh-connection to the host-system and did this:

|1 2|# sysctl security.jail.sysvipc_allowed security.jail.sysvipc_allowed: 0|

AHA! Gotcha!

|1 2|# sysctl security.jail.sysvipc_allowed= 1 security.jail.sysvipc_allowed: 0 -> 1|

Back in the jail I reran the make, and that completed roughly a minute later, without any errors. Sure, you might say that allowing these things is a bad idea, but it sure beats running PostgreSQL/FireBird in a host system. *shudder*
