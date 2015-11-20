---
layout: post
title: "Taking Fedora 10 for a spin"
permalink: 2008-11-28-taking-fedora-10-for-a-spin
---
On the Fedora project [website](http://fedoraproject.org) you can find a list of new [features](http://fedoraproject.org/wiki/Releases/10/FeatureList) in Fedora 10. As a long time Fedora (Core) user, reading these made me pretty excited about this tenth release.

They touted faster and more fancy booting, better printer and webcam support, improved network handling (ability to share network connections effortlessly), and many other things.

In this article I will tell you about my experience with Fedora 10.



The hardware Fedora 10 will be installed on is a Dell Precision M70 with 2 GB RAM, 30GB 5400 RPM harddrive, 2.0 GHz Pentium M processor and a display that can do 1920 x 1200 resolution. The laptop resides in a dockingstation. In adition to the above I have two external monitors (Dell 24″ that each can do 1920 x 1200) that I have connected to the DVI and VGA ports on the dockingstation.

[IMHO](http://www.google.com/search?q=define:IMHO) this hardware should be sufficient to run the newest installment of Fedora.

After burning the DVD-image that I found [here](http://fedoraproject.org/en/get-fedora-all) I popped the disc in the DVD-drive and booted the computer. I booted the machine with the laptops lid closed. This makes it default to one of the external monitors, and prevents some hiccups the X-server tends to get when I don’t do it this way.

When the booting started the first thing that filled the screen was text about images being uncompressed and things that was being loaded. As much as I’d like to _not_ see this I have to accept that it’s not _that_ much of a need to get rid of it, even though it will bring the use backto the good old days of MS DOS.

After waiting for the images to uncompress and waiting for the “new and improved graphical user experience” Fedora presented me with something that frightened me a bit. The screen was filled with blue! MS DOS style!

When I finally recovered after the initial shock, I learned that here I could test the installation-media befor I attempt installing from it. Ok, now that’s useful. I do not know how many times I’ve been installing Windows XP and gotten a message saying something along the lines of “Cannot read Somedll.DL_. Please insert installation disc.”.

After testing the disc I proceeded with the install. [Anaconda](http://fedoraproject.org/wiki/Anaconda) was fired up and on we went. On thing to note here; the installer used my external display, but for some truly bizzare reason it choosed to only use a small part of the display. Anaconda occupied perhaps 1/6 of the total screen area on ONE of the monitors. That sucked, but ok.

From here everything was more or less similar to the previous couple of releases. Only thing that comes to mind was that I saw an option to have the filesystem encrypted. Everything else was pretty standard; choosing password for root, formating the disk, choosing packages to install, etc.

On we went, and before the end of my dinner the installer informed me that it was now completed and all I had to do was reboot. Hitting “Finish” closed the installer-window but left the mouse cursor on the screen. After waiting for a good 10 minutes for the machine to reboot on it’s own I finally gave in an manually restarted using the power-button on the computer.

So keeping the laptops lid closed I rebooted and waited for that special [Plymouth](http://fedoraproject.org/wiki/Releases/FeatureBetterStartup) experience. What I got was flicker, some more flicker, oh and some more flicker. After that I got the extremely fancy “text plugin” for Plymouth. What that looks like can be seen [here](http://ajax.fedorapeople.org/text-boot-hotness.ogg) (Ogg-support for playback needed).

Needless to say I was let down. It was like going back in time a whole bunch of years. After investigating this on the Interwebs (you know, it’s a series of tubes, just mislabled as a “web”) I stubled upon the [“User Documentation”](http://fedoraproject.org/wiki/Features/BetterStartup#User_Documentation) on the Fedora Project website, subsection Features / Better Startup. There I learned that “Plymouth’s graphical boot plugins require kernel modesetting (KMS) support. KMS is currently supported on most ATI Radeon chips”.

Some more digging provided a [comment](http://www.nvnews.net/vbulletin/showpost.php?p=1841465&postcount=4) from one AaronP stating that “that the hooks necessary to implement kernel modesetting were exported to GPL modules only, and therefore are not usable by the NVIDIA driver”. Seems like the good ol’ (and annoying as a nail to the eye) GPL comes back to bite us in the ass. Oh well, as long as 3D-acceleration works I’ll be happy.

Note: I had as of yet not installed the proprietary nvidia-drivers. We’ll get to that.

After waiting for a while I was presented with the standard “First Boot” welcome screen. Everything here was pretty similar to what I’ve seen previously, with one glaring exception. It did not present me with an option to disable SELinux. Why would I want to do that you say? We’ll get to that one too.

Some flicker later a new, and I must say rather dashing, login-screen presented itself. At this point I just had to take a couple of minutes to admire the artwork. Fedora chose well in using the [Solar-theme](http://fedoraproject.org/wiki/Artwork/F10Themes/Solar). It is incredibly beautiful and pleasing to look at. The whole color-scheme is well composed, and the artwork has dynamics that serve more to please than to bug your eyes. Good job Mr. Mysterious Designer Man!

However, lets not forget that the login form (yeah, I’m old fashioned and call it a form) has seen some improvements. Better layout, less of that horrible grey surface. KDM that I use (since I use KDE) has come a long way. It seems like I no longer have to start the login-process in shame because it looks like crap. Chin up, be proud. KDM is now centerfold-sexy.

I chose my user and enterd the password I had set during “First Boot”. Only a minimal amount of flicker occured, and I was logged in.

Boot time is not great. With the defauls services turned on it took close to three minutes to boot. Windows XP on the same computer takes roughly 30 seconds from right after BIOS until I can start working on the desktop. Fedoras performance in this case is not good, but as expected really (though I had hoped for something better). Linux does not really have a reputation for fast boot / load times.

The first thing I do is usually a ‘yum update’ of the whole system. A lot of updates were installed and I noticed some changes in Yum. First of all it was faster. Dependency resolution, which had become one of those go-build-yourself-a-greenhouse-while-you-wait deals on Fedora 9 (not as slow as Yast though), was suddenly lightning fast. The other improvement was that at the end of execution you will see a column-formatted view of packages installed, removed, and updated. Very nice. Makes it all a _lot_ better. Good work guys.

Next up was installing the proprietary Nvidia drivers. Not a big deal. Do the following as root:

|1 2 3 4|rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm \ http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-rpmfusion-* yum install kmod-nvidia|

It finished quickly and I rebooted and logged in again.

GAH! Big-ass fonts at random places! The [DPI](http://www.google.com/search?q=define:DPI) was probably set all wrong. After doing some digging I found that this is according to some a “Fedora feature”. However, if you are like me and prefer sane font-sizes, this is what worked for me: Opened /etc/X11/Xresources and commented out the line that read

|1|Xft.dpi: 96|

In addition to that I opened /etc/X11/xorg.conf and put

|1|Option "DPI" " 96 x 96 "|

in the “Device” section.

A manual restart (ctrl-alt-backspace) of the X-server reloaded the settings and the fonts looked like they should have done from the beginning.

Next up was configuring the system so both my 24″ monitors were used. I opened “nVidia Display Settings” found under [Programs]->[System] and selected a TwinView setup for the two external monitors, and disabled the laptop-display.

This has worked without a hitch since Fedora 8 (I think..) and no surprise here; it still worked. Some interesting changes did however present themselves in KDE at this point.

In KDE 4.1 on Fedora 9, when I chose a dual-screen layout using the nVidia-program the panel at the bottom of the screen would span the two monitors if it was set to span. I could center it across the two and choose whichever size I wante to. Worked pretty well.

Now on the other hand, the panel insist on living on the rightmost monitor. I would like to have it on the leftmost one, or be able to center it, but that does not seem like something that can be done easily. So I’m stuck with the panel on my right.

Next up I wanted to use a wallpaper that stretches across my monitors. Wanting to keep with the Solar theme I found [this](http://fedoraproject.org/wiki/Image:Solar_3840x1200_DayTime.png) on this [page](http://fedoraproject.org/wiki/Artwork/F10Themes/Solar#Wallpapers). I save it locally, right-click the desktop to get to the desktop settings and choose the downloaded picture as background. Lo and behold! Only one monitor got it. I actually had to open the image manually, cut it in half (Gimp 2.6 is sweet btw), and set the two images on the two different desktops individually.

I guess this can be described both as a bug and a feature. To you who would place this in the feature-category; I agree, it’s nice to be able to set backgrounds individually, but _let the user choose_. If I want a background stretched, then let me.

On the other hand, maximizing a window on one monitor now works. Good.

Next up; installing [Opera](http://www.opera.com). Why this browser you say? Well, it’s simple. I’ve been using it since it’s first public beta and really come to treasure it’s speed, features and clean UI. The latest version (9.62) for Linux has got some neat freatures and has proved to be stable and comfortable to work with.

To install you just have to go to the Opera website, click “Get Opera” og “Download” or something like that. When it comes to distribution / vendor just choose the one for Fedora 8. You can choose to open the file with the package-management utility (it will suggest you do so) and you will have a brand spanking new browser installed.

To get Flash visit this [page](http://www.adobe.com/products/flashplayer/) at Adobes website and select “YUM for linux”. You’ll then get an RPM that will put a .repo-file in your /etc/yum.repos.d/ directory. Next up just do the following as root:

|1|yum install flash-plugin|

That’s what worked for me anyway.

After a snack I sat down again to get my data back. It’s a collection of pricate photos, work-related stuff, private documents and some multimedia. The usual mix. I had dumped this to an ftp-server at home (just ‘cos it’s easy) and expected it the be a breeze to get it back.

I opened my home directory in Dolphin and chose to split the view. This is where the trouble started. The filemanager came to a halt. It would take several seconds after clicking on a folder until it became selected, and upwards to a minute before that folder would open and the content would be shown. This is unacceptable.

I noticed a tiny star in the basket in the panel and went to investigate. Apparently SELinux had intercepted something and was blocking things in the system. Since this started happening when I started using Dolphin I figured that I’d disable it yet again (like I have done every time I have installed Fedora). The way to do this is fairly straightforward and I’ll leave it as an exercise for the reader to figure it out (Hint: Use the K-menu Luke!).

After disabling SELinux it worked like a charm.

You may argue agains doing this, but what it boils down to is this; I need a working desktop. If SELinux is a hindrance it has to go. Simple.

I did also get a whole lot of popups from various services and such. Package management, NetworkManager etc. I can accept these since they were not very obtrusive, and did eventually do as they promised. There were just so many of them… But ok.

You might be left with the impression that I don’t like Linux, and could do without this latest installment of Fedora. It’s actually quite the opposite.

Linux is to me like that big toolbox with all those sharp edges that you simply cannot carry without beating your legs until they bleed. Why do I stick to it? It has got tools in it. Powerful tools. Well-made tools. Tools that I don’t want to live without. As a developer there are certain things I need, and Linux provides them in a very neat package.

Would I recommend this distro to a computer novice? No. There are too many things that needs fixing through the console, too much bad behaviour that will annoy a lot of people. If you are a developer and want quality tools; go for it. Also if you are somewhat familiar with the commandline or don’t mind learning; go for it.

That said, I’m pleased with the progress and improvements in this release. We finally have OpenOffice 3 which no longer mess up your desktop if you use nVidia-drivers. A lot of small quirks in Fedora 9 has also been rectified. See Fedora 10’s changelog / releasenotes to get more info on this. KDEPim has also included Kontact for KDE 4, so you no longer need to use the old KDE 3 version like you did on Fedora 9. It’s a step up, that’s for sure.

Fedora team; you have done a good job, though there are still ways you can improve your distro. I will continue using it, and I’m eager to see what you have in store for us next.
