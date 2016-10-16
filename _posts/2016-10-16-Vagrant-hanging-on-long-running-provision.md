---
layout: post
title: Vagrant hanging on long running provision
permalink: vagrant-provision-hang
comments: True
---
As you will probably agree Vagrant is an absolutely fantastic tool for everything from bringing up a single simple dev-machine, to creating complex multi-node clusters for distributed load-testing. With the addition of plugins like [vagrant-digitalocean](https://github.com/devopsgroup-io/vagrant-digitalocean), [vagrant-aws](https://github.com/mitchellh/vagrant-aws) and [vagrant-hostmanager](https://github.com/devopsgroup-io/vagrant-hostmanager) you can do some pretty cool stuff.

However, recently I've been struggling _a lot_ with getting Vagrant to run reliably. The issue I ran into was that vagrant would spin up a node (using the digitalocean plugin) and start provisioning. Then after some time it would just hang there outputting nothing. I ssh'ed into the offending box and checked what processes were running. To my surprise none of the provisioning scripts were running, and everything looked to have been finished successfully. In my provisioning scripts I usually just redirect STDOUT and STDERR to a logfile, or if sufficiently interesting and not very verbose I pipe it through `tee` so I get both log to screen and log to file. When I ran `vagrant up mymachine --debug` it was apparent that Vagrant was chooching along just fine, totally oblivious to what was happening on the machine. The Vagrant log just outputted this indefenitly:

<pre>
DEBUG ssh: Sending SSH keep-alive...
DEBUG ssh: Sending SSH keep-alive...
DEBUG ssh: Sending SSH keep-alive...
DEBUG ssh: Sending SSH keep-alive...
DEBUG ssh: Sending SSH keep-alive...
DEBUG ssh: Sending SSH keep-alive...
</pre>

After I had done some googling I found a lot of issues that seemed to describe the same things as I had observed. However they all seemed to veer into one direction or other either blaming shell output redirects, ssh agent forwarding or pty-requests from spawned processes (like `apt-get` and such). I did a lot of experimentation and from what I can tell it has got nothing to do with either one of those.

To be precise, the command that made my provisioning-scripts halt was the one I used to install all Perl dependencies for a particular project. We use `cpanm` to install and install from a custom cpan-repository (somewhat irrelevant). This was executed like so:

<pre>
cpanm \
  --no-wget \
  --quiet \
  --mirror-only \
  --notest \
  --local-lib $LIBDIR \
  --mirror https://you:wish@pan.secret.sauce.com/org/project/ \
  --installdeps . | tee -a $LOGFILE
</pre>

Due to how `cpanm` is written the order that modules are installed in (this without any prior requirements) is somewhat random so the time until Vagrant seemed to hang varied. I did however narrow it down to being when either one of two modules were being installed. One of then was [Alien::ImageMagick](https://metacpan.org/pod/Alien::ImageMagick) which fetches the real [ImageMagick](http://www.imagemagick.org/script/index.php), does some build-patching so it can run under [Perlbrew](https://perlbrew.pl/) and [Plenv](https://github.com/tokuhirom/plenv), and then go ahead and compile it. While this compilation is running _absolutely no output is produced_ when running `cpanm` like in the above example. This made me think that Vagrant hanging might be the result of a lack of output.

I went ahead and changed the task to this:

<pre>
cpanm \
  --no-wget \
  --quiet \
  --mirror-only \
  --notest \
  --local-lib $LIBDIR \
  --mirror https://you:wish@pan.secret.sauce.com/org/project/ \
  --installdeps . | tee -a $LOGFILE

cpan_pid=$!

trap "kill $cpan_pid 2> /dev/null" EXIT

while kill -0 $cpan_pid 2> /dev/null; do
  echo -n `date`' ... still installing perl dependencies'
  sleep 10
done

trap - EXIT
</pre>

This made the execution succeed every single time. Hooray for fun issues eh?
