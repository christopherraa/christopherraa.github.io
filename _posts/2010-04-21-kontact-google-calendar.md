---
layout: post
title: "Kontact & Google Calendar"
permalink: 2010-04-21-kontact-google-calendar
---
Have you ever been annoyed at the fact that there has been no straightforward and easy way of having a two-way synchronization between [Kontact](http://www.kontact.org) and the perhaps biggest online calendaring-service [Google Calendar](http://google.com/calendar)? I certainly have. The good news is that the wait is now over. Kontact now talks to Google Calendar.

Historically you would have to go through the tedious steps of setting up [GCalDaemon](http://gcaldaemon.sourceforge.net) to read / write to a local iCal-file and have Kontact read / write to that again. I am not going to go into detail on the issues with the above approach. What I feel that I must say is even though it was cumbersome, error prone and generally not how I like things; it did work. Most of the time.

What we have now is a brand new KDE ( 4.3.2 here on my desktop ) which sport some pretty decent advances. One of these is [Akonadi](http://en.wikipedia.org/wiki/Akonadi). Akonadi seeks to facilitate easy administration and utilization of various data sources for personal information, such as mail, contacts, calendaring, etc.

To get Google Calendar working in KDE you now need to install the “googledata” Akonadi resource. In Debian this is done by issuing the following command:

sudo aptitude install akonadi-kde-resource-googledata

After that you open Kontact and do the following:

Click [Calendar] in the sidebar (usually in the left hand side of the window) In the resources view (field in bottom left corner) right-click and select “Add new resource” Choose ‘Akonadi resource’ in the dialog that pops up A new dialog will pop up. Here you should click “Edit calendar resources” (translations may vary) Click ‘Add’ in the new dialog. Choose ‘Akonadi Google Calendar Resource’ Enter username and password when prompted Press the ‘Close’-button A new resource with a name similar to ‘akonadi_gcal_resource_X’ should now be visible Select it and press ‘Synchronize folder’ to do an initial sync Press the ‘Ok’-button to return to your calendar view

The items you have in your Google Calendar should now be present in Kontact.

I hope someone will find this helpful.
