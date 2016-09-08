---
layout: post
title: Final Thoughts On Gitlab
permalink: final-thoughs-on-gitlab
comments: True
---
Previously I wrote a post on our companys move from [Github to self hosted Gitlab](/migrating-github-gitlab). As mentioned in the post we were fairly happy with the move at the time, and this post is an attempt at summarize our experience with it now that we've had it running for well over 8 months.

# Using Gitlab
## Keeping track of issues
Over all keeping track of issues is not very difficult as the interface has some decent labeling and filter options. There are however some key things that I believe could make it even better.

The first of these is what I'd call "negative filter". Lets say I want to see all issues that are labeled with _backend_ and is explicitly not labeled as _discussion_. Getting the list of issues that match this would today be a tedious task of getting all isses labeled _backend_ and then doing manual filtering when looking at the results.

Being able to put multiple assignees on a task would be super useful. For the small team of developers we have this would make reporting and managing issues that should be touched by multiple people less of a chore. Let me give you a practical example. Let us say that someone files a bug reporting on some weirdness in our software (it does happen, and we can't _always_ label it as a _feature_), and upon closer inspection it proves to not be a bug but a quirk in some of our design choices. In this case I would have the designer or the UI-guy sit down with one of us doing backend development and figure out a solution, implement it, test it and deploy it. Ideally I would create one ticket / issue in Gitlab, label it with _backend_, _frontend_ and perhaps _enhancement_, assign it to the two correct people and make sure that all discussions on the matter went into the issue. And yes multiple people can be register as participants, but what I would like to see is multiple assignees.

## Mobile experience
One thing that still has me puzzled is the mobile experience when using Gitlab. The interface when used from a computer is fine, but the mobile experience leaves something to be desired. It might very well be that I have a weird work-pattern, but when I am on the move and not using a computer I frequently find myself wanting to comment on issues, check build statuses, read up on commits or perform other tasks within Gitlab.

It does _work_, but it is far from a comfortable experience. Some of this is due to how assets are handled, some is the markup and some of it I think is just a result of the desktop getting all the design-attention. That being said: it is slowly getting better, but I would love to see the mobile interface getting more love. I think Gitlab would benefit from that, and it would enable me to get more work done while traveling. Any tool that let me stay productive during "down time" get a lot of love from me.

## Filing issues for Gitlab-projects
My experience with filing issues for Gitlab CE and related projects (like Gitlab CI) has been very nice. I have many reasons for loving (F)OSS, and the attentiveness of the people running projects such as Gitlab is one of the top reasons. They take care really listening, and also to _understand_ what it is that the people reporting issues are trying to communicate. For me as a non-native English speaker this is really appreciated, as it is often difficult to convey exactly what it is I mean. You're doing a great job!

## Continuous Integration
As it currently stands Gitlab has a very well thought out CI system. Yes there are some quirks, _especially_ when it comes to asset/artifact/cache handling, but all in all it works great. It is pretty easy to structure the build pipeline so that it is expressive and efficient. My experience is that you can make it do pretty much whatever you need it to. If you have tried the CI-system a while back then I'd say that chances are that you found the options limited, the documentation poor and the performance awful. That was my first impression of it at least (_way_ back mind you). However the state now is completely different. The documentation has received a **huge** increase in completeness and quality, many more features are now elegantly implemented and the performance is pretty good. This is another testiment to Gitlab and their steady and continuous work on improving their product.

# Managing
## Stability
In terms of stability we have had absolutely no problems. Just for testing out we have been running Gitlab on [Digitalocean](https://digitalocean.com] (gotta love those guys) and it has worked out great. No hiccups, no crashed and very little in the form of maintenance has been needed. All in all it just works.

## Upgrading
As mentioned previously we run Debian (critical systems) and Debian-derivatives like Ubuntu (non-critical stuff) on all our servers. Since the Gitlab-instance we run on Digitalocean run on the same type of host then doing upgrades is a breeze. All we have to do is:

    sudo apt-get update
	sudo apt-get install gitlab-ce

Yup. That's it. What this means is that we actually do upgrades right when they are released, and we absolutely make sure to pull in the latest version when security patches are released. When you can have confidence in the platform and it's ability to safely upgrade itself then you'll have no problem doing the little work that it is keeping current with regards to new versions of the software. For a small shop like ours these little timesavers are very much appreciated.

# Listening to the community
One thing that Gitlab should be praised for is really listening to their community. Thet have a well-functioning issue-tracker where a lot of good (and actually civil) discussions take place. The Gitlab employees seem to really take to heart the feedback, requests and ideas that the community produce. Many of these result in improvement and extensions of Gitlab as a product, something that I am sure benefit not just us the users but also Gitlab as a company.

# In conclusion
Gitlab is a great product, and a product that serves us great instead of getting in our way. We very much look forward to using Gitlab in the time to come, and are confident that it will enable us as a team to spend our time focused on building our own products.

We ❤ Gitlab.
