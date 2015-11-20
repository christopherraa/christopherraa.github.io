---
layout: post
title: "Qt 4.5 and MySQL-plugin with Mingw on Windows XP"
permalink: 2009-04-14-qt-45-and-mysql-plugin-with-mingw-on-windows-xp
---
This is meant to be a quick howto / tutorial on how to setup a build-enviroment for C++ / Qt with Qt Creator ( Trolltech / Nokias new Qt IDE) where you use Mingw to compile your applications and have a MySQL-plugin available.



You will be needing three files to get started.

First get the latest GA-release of MySQL. Go to their [download-section](http://dev.mysql.com/downloads/mysql/) and find the “Windows MSI Installer” for your architecture (x86 or x64). Save the file to your desktop.

Then go to Trolltech / Nokias [download-section](http://www.qtsoftware.com/downloads/sdk-windows-cpp) to get the LGPL’ed Qt SDK. Save the file to your desktop.

Finally go to the Mingw [download-section](https://sourceforge.net/project/showfiles.php?group_id=2435) to get the Mingw Utilities. I downloaded version 0.3 (latest version as of this writing). Save the file mingw-utils-0.3.tar.gz to your desktop.

In addition to the three files mentioned above you might need [WinRAR](http://www.rarlab.com/rar/wrar380.exe) or a similar tool to unpack tar.gz-files.

I always start by installing MySQL. This is not a requirement and the order in which you install MySQL and Qt does not matter.

Start the MySQL-installer you have on your desktop. Choose the advanced install options. I usually remove the MySQL server instance from the install as I have several external database-servers I do tests against. No need to clutter the dev-box if you have it available other places.

What’s important here is that you choose to install “Development Components” and all sub-features. These are the ones that’ll enable Qt to know how to reach a given MySQL database.

Another important step here is to install directly to c:\mysql, not c:\Program Files\Mysql\MySQL Server\whatnot. Spaces in pathnames confuse certain of the items in the toolchain. If you do install somewhere with spaces in the path then you have to remember to always use the old style-pathnames: c:\Progra~1\etc .

Proceed with the install, and after completion verify that you have libmysql.dll under c:\mysql\bin, and that there is a directory c:\mysql\include with header-files (.h) in it.

Start the Qt SDK installer and choose to install it in c:\Qt\2009.01 or whatever path it propose, as long as it is a path without spaces in it. Choose to install all the components, including Mingw.

The Mingw Utilities is simply a collection of executables, plus a couple of dlls. Open the file you downloaded earlier with a suitable program like WinRAR and copy the _contents_ of the bin-directory into C:\Qt\2009.01\mingw\bin.

It might just be me having this problem, but I had to rebuild QMake even before configure would run and not crash spewing something about not finding stuff in ‘c:\Progra~1\Trolltech..’. Anyway, it’s not exactly rocketscience. Locate the ‘Qt Command Prompt’ and start that. It will upon execution add a couple of locations to PATH so that you’ll have the tools you need easily available. From there you do the following. C:\Qt\2009.01> cd qt\qmake C:\Qt\2009.01\qt\qmake> mingw32-make clean C:\Qt\2009.01\qt\qmake> mingw32-make After that QMake should start looking for mkspecs and other candy in the appropriate locations.

The next thing you want to do is run configure with a handful of arguments. For the sake of simplicity I’ll just include the arguments you need to have Qt build with the MySQL-plugin. Again, in the Qt Command Prompt: C:\Qt\2009.01\qt> configure.exe -debug-and-release -plugin-sql-mysql The above will create Makefiles and things for all the sub-projects in the Qt directory-tree. This might take a little while so you might want to have some knitting-needles or a book at hand.

See ‘configure.exe -help’ for more options.

The [Qt Reference Documentation](http://doc.trolltech.com/latest) is one of the best pieces of documentation I’ve ever seen. It’s full of working examples of how to use the API Trolltech / Nokia provide and generally have a very high level of quality.

However when it comes to how to build Qt with certain plugins it can be a bit lacking. The documentation [here](http://doc.trolltech.com/latest/sql-driver.html#qmysql) states that you do the following to build the MySQL-plugin on Windows: cd %QTDIR%\src\plugins\sqldrivers\mysql qmake "INCLUDEPATH+=C:\MySQL\include" "LIBS+=C:\MYSQL\MySQL Server\lib\opt\libmysql.lib" mysql.pro nmake That’s great, _if you use nmake_. Alot of us rely on Mingw to build our applications on Windows, and for us the above simply will not work. That is due to differences in object and lib-format between MSVC (which the mysql-dll’s are built with) and the GNU-toolchain. To solve this you have to do some magic on the dll and produce something that the GNU-toolchain can work with.

Proceed by doing this in the “Qt Command Prompt”: C:\Qt\2009.01\qt> cd src\plugins\sqldrivers\mysql C:\Qt\2009.01\qt\src\plugins\sqldrivers\mysql> set MYSQL_PATH=c:mysql C:\Qt\2009.01\qt\src\plugins\sqldrivers\mysql> reimp -d %MYSQL_PATH%\lib\opt\libmysql.lib C:\Qt\2009.01\qt\src\plugins\sqldrivers\mysql> dlltool -k --input-def LIBMYSQL.def --dllname libmysql.dll --output-lib libmysql.a C:\Qt\2009.01\qt\src\plugins\sqldrivers\mysql> qmake "INCLUDEPATH+=%MYSQL_PATH%include" "LIBS+=-L. -lmysql" mysql.pro

When all of the above has been done all you have to do is start the Qt build process. Again in the “Qt Command Prompt”: C:\Qt\2009.01\qt> mingw32-make sub-src That’s it! You should now have qmysql*.dll in pluginssqldrivers. To use it just make sure that you have the path where you find libmysql.dll (usually C:\mysql\bin) as a part of the environment variable PATH.

Questions and comments are as always welcome.
