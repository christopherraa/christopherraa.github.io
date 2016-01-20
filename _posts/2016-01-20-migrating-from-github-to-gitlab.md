---
layout: post
title: Migrating from Github to Gitlab
permalink: migrating-github-gitlab
comments: True
---
For the past few years the company I run have been a paying customer at [Github](https://github.com). Generally we've been very satisfied with the service, but we have come to realize that it might be time to move on. If our decision has moved us to greener pastures remains to be seen.

The pros for using Github were:

- no infrastructure to manage
- easy access
- integrate well with tooling
- packed with awesome features
- resonably low cost
- plenty of integrations / external services

The cons we saw were:

- no control over who have access to our code
- too frequently low push / pull speed
- cost/repo dropping too slowly as repo count increase
- high prices for the collection of extra services we need

We could naturally go with Github Enterprise to address some of these things but from our perspective the price to get started is too steep. For our team size it would mean a x5 price increase. After looking at what we need in terms of functionality, plus external demands (tooling, customers etc), the list was condensed to these requirements:

- public and private repositories
- user management
- issue-tracking
- organizing milestones
- sensible continuous integration
- hooks to call external services on events
- self-hosted with little maintenance

## Enter [Gitlab](https://gitlab.com).

Gitlabs webpage is in my opinion mostly marketing fluff made to make middle-managers responsible for purchase feel all warm and fuzzy inside. This is mostly what I see on all other pages where an enterprise offering is present. If it helps drive adoption and funds for the company behind it I'm all for it. Instead of reading fluff I prefer to test the product, and that is what I did. This is not in any way meant to be a detailed review of Gitlab, but some of the things we discovered were:

### Easy to get instance going

Getting up and running with Gitlab was _extremely_ easy. Several providers, [DigitalOcean](https://digitalocean.com) among others, offer you ready to run virtual machines with Gitlab preinstalled. This is one option if you want to get going quickly. When we evaluated Gitlab we opted for firing up an Ubuntu instance in Virtualbox and following the install-guide in the Gitlab documentation. Keeping the thing updated inside the virtual machine was very easy, and we felt comfortable moving on to a more "proper" setup.

As we already have some droplets running at DigitalOcean we chose to use them for the more permanent setup. For this we used the ready-made Gitlab stack that they have, and chose a droplet at US$20 pr month. Cost-wise this left us at under half of what we currently pay to Github.

### Easy to import repositories

So, this is a pretty big thing actually. Any time you deal with migrations you want the process to be quick and you really don't want your users to experience any pain, as that will reflect right back on you. Needless to say I was very happy to see that the process of getting all our repositories into Gitlab was as easy as:

- setting up Github OmniAuth authentication
- logging in with Github credential with organization access
- choosing to create new repository with Github as source
- marking all existing repos, and watch the import happen

A couple of things to note here:

- the import does *NOT* import your wiki pages
- projects get marked "private", you have to click each one to mark it "internal"
- you might have to set the default branch if you don't want it to be `master`
- you'll want to set up SSL. Just. Do. It.

### Easy for developers to switch

For the developers and designer here the switch was almost too easy. It meant

- logging in to Gitlab
- adding the ssh public key
- `git remote set-url origin git@<GITLABHOST>:<ORGNAME>/<REPO>.git`

That was it. This meant that even though the project hosting moved, there was practically no developer downtime.

### Some work importing "cards" from Trello

So, we had this naíve idea that it would be neat to move all Trello cards into Gitlab as issues, as we wanted to use Gitlab for issues as well. Even though Trello has an "Export JSON"-feature it was not an entirely painless process parsing it. After doing some scripting we concluded that it was absolutely feasable, but not worth the effort at that point in time. We instead moved all pending issues manually from Trello to Gitlab. Copy, paste, rinse, repeat. All closed issues were left in Trello. If we need them later we'll probably finish the script and publish it so other people can try it out.

For the curious; as of this writing Gitlab does not have support in their issue-API for backdating issues, which is essential if you want to keep history. Thankfully the database layout is fairly simple and it is trivial to just inject data directly there, which is what our script did.

### Decent support for integrations

Gitlab has out of the box support for several nice services like [Hipchat](https://hipchat.com/), [JIRA](https://www.atlassian.com/software/jira), [Slack](https://slack.com) etc. Here we use Hipchat and wanted to set up notification for issues, builds and commits.

The default setup for the Hipchat module is pretty easy, and the configuration is fairly basic. It does not take you long to see notifications start appearing in Hipchat. One thing I have to note though, is that it is written to be so spammy that it is almost not worth turning it on. We want the messages to be short and concise, but what happens is that it is so incredibly verbose that all other interaction just drown.

It would help immensely if

- we had a way to choose terse (one-line) notifications
- comment text etc was not wrappen in <pre>

For our own sake we will have a crack at submitting a patch that fixes this. If it is approved thats great, if not we'll just monkey-patch on our end. Leave a comment if you're interested in the patch as well. If so we'll leave it out there for people to grab.

### Basic but very nice CI setup

I won't comment much on this here, as it is a large enough topic to warrant it's own post. However I do want to say that getting up and running with Gitlab CI was very comfortable and did not take long at all. It is pretty basic (especially compared to the build-your-own-buildserver-swiss-chain-machete-of-wonder [Buildbot](http://buildbot.net/)) but it gets the job done. Configure one or several runners, drop a `.gitlab-ci.yml` file in your project root and you're done. [The documentation](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/) is a little bit lacking, and I found it easier to just use Google for whatever I was curious about, rather than looking at the documentation.

All in all *very* happy with the CI-stuff.

## In conclusion

After a couple of weeks running everything on Gitlab we are very satisfied. We will keep and pay for the Github account for a while longer, but I see no reason at the present time to move back. We might post some more updates on how we have set things up to fit with the rest of our tooling, but that is a task for future-us.
