---
layout: post
title: "Tip Of The Day: Rescan faces in Picasa 3"
permalink: 2010-12-13-tip-of-the-day-rescan-faces-in-picasa-3
---
Some have written complaining that the steps outlined below double-tags the people in their photos. I did not have that problem, but do be careful before you attempt these steps, as Picasa might behave differently between versions.



When you start the initial scan the application will hog whatever resources your machine has available (at least on single/low-core machines). This is not a bad thing, and not the problem which I encountered. The problem is actually related to the developers solution to the resource-hogging. If you start messing around in the program while the scan is running, then the program will in fact queue / pause or simply cancel the running job (so you will have a snappy, not laggy interface), never to resume it again. At least this is what I assume from observing the behavior of the application.

The result of this is that everything is seemingly fine, and you have gotten a lot of faces to tag with names. What you do not get any indication of is how many of the pictures were scanned. When you discover that not all of the pictures were scanned it’s natural to ask the question “Where is the ‘rescan for new faces’-button?”. The short answer is; “There is none!”. At least not as of this writing.

There is however a solution to the problem. Go to “Tools”->”Folder Manager”, highlight the topmost directory that contains pictures / folders of pictures and click the option “Face detection” from “On” to “Off” then back to “On” again. When you then click “Ok” the application should start scanning again. At this point you should go grab a cold one, write a note to your spouse so he / she does not disturb it and go do something else. It’ll take it’s time to complete. Do NOT click “Ok” before you have switched the “Face detection”-option back to “On”. If you press “Ok” while it is “Off” then Picasa will forget everyone you have already tagged and you’ll have to tag everyone again.

Oh, and dear Picasa developers: please make it so that Picasa can use more resources while performing these tasks. Here it’s using only one core, and very little available disc-bandwidth. It’s got 7 idle cores, 6 GB ram not being used for anything and a disc that’s screaming to be utilized. You could provide a configuration-parameter / slider that indicates how much of the available ‘juice’ it can use: “Please be gentle…” -> “You think you can take me?” -> “USE ME BABY! USE ME!”

I’l let you decide on the labels. ![;)](/public/uploads/smilies/icon_wink.gif)
