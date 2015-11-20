---
layout: post
title: "Building Qt without examples, demos and all that other stuff"
permalink: 2009-04-11-building-qt-without-examples-demos-and-all-that-other-stuff
---
This is just a tip for all of you who, like myself, are tired of waiting through a full Qt build (sources, examples, demos, +all the bells and whistles).

After having run ./configure (Linux / *NIX) or configure.exe (Windows) if you just want to build the qt-libraries, plugins etc then do a make with ’sub-src’ as an argument to the mak-command. Examples:

|1 2 3| mingw32-make sub-src nmake sub-src make sub-src|

This is not explained in the qt-docs as far as I’ve seen, but that does not mean it’s not there. If any of you guys / gals find it; let me know, so I can link to it.

Hope this is useful for someone.
