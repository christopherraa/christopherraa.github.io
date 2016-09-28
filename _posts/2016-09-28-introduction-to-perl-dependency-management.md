---
layout: post
title: A gentle introduction to Perl-dependency management
permalink: perl-dependency-management
comments: True
---
If you have been programming Perl for a while you might be familiar with [CPAN](https://metacpan.org/), the large repository of every module conceivable. It is a great source for ready made Perl module that can solve almost every problem you are working on, but it is also always changing due to the dilligence of the maintainers of various modules. This post is meant to be a gentle introduction to _one way_ you can do some simple dependency management and add module stability to your Perl projects. As always in Perl: [TIMTOWTDI](https://en.wikipedia.org/wiki/There%27s_more_than_one_way_to_do_it).

# The problems we seek to solve

## Stop the use of system perl

I do not pretend to know the ratio between Linux, Mac and Windows users who do Perl programming, but this item is primarily for Linux and Mac users.

Chances are your OS/distribution of choice comes with a version of Perl preinstalled, and it is also very likely that it is obsolete or patched in some peculiar way that will bite you when you least expect it. That is why I recommend compiling and running your own version of Perl. This is not one of my bright ideas though, I am merely echoing the good advice other kind people gave me back in the day. Compiling something as large as Perl used to be a daunting task, but this is no longer the case. The state of Perl today is really great and the tooling around Perl has seen a huge boost the last couple of years.

*Goal 1:* **find some way of using your chosen version(s) of Perl**

## Isolate project dependencies

When working on multiple projects like most of us do it is hugely beneficial to be able to rest assured that the dependencies you install for one of your projects do not interfere with one or more of your other projects. We achieve this by assigning specific directories in which modules are installed. For this there are also multiple options you can consider.

*Goal 2:* **isolate dependencies for project A from project B, C and D**

## Lock versions of modules you depend on

Have you ever installed all the dependencies a project needed when you worked on it, forgot the project for a while, then set the project up again only to find that the dependencies you were using had changed their API? I sure have. For small pet projects this is usually not a problem, but when you do things for `$work` then it might become a bigger issue. Nobody want surprise breakage in production.

We solve this by specifying exactly which versions of each module that we want to install for a project. For this there are multiple options, all of which solves the core issue of locking versions, and some who solve multiple related issues as well. I'll go through some of the options further down.

*Goal 3:* **lock versions of modules you depend on**

## Summary of goals

So summarize we now have three goals:

 - find some way of using your chosen version(s) of Perl
 - isolate dependencies for project A from project B, C and D
 - lock versions of modules you depend on

# Goal 1: find some way of using your chosen version(s) of Perl

By far the most talked about and perhaps also most widely used ways of handling multiple versions of Perl are [Perlbrew](https://perlbrew.pl) and [Plenv](https://github.com/tokuhirom/plenv).

Perlbrew is a tool that helps you install Perl, cpanm and do a whole lot when it comes to managing modules and libraries. Once Perlbrew is installed getting up and running with a new version of Perl is as simple as:

    perlbrew install perl-5.24.0
    perlbrew use perl-5.24.0
    perl my-app.pl

When you run that last statement `my-app.pl` is executed using Perl 5.24.0. You may run as many `perlbrew install ...` as you need, and you can rest assured that the different versions will not interfere with each other. Perlbrew lets you switch between perl versions globally (think of it as your "default" perl version) with `perlbrew switch ...` or use a specific version in the current shell only with `perlbrew use ...`.

Plenv is a tool that promises some of the same features as Perlbrew, but does some things differently. Once installed you may run the following to install a Perl version, start using that version and run an application on that version:

    plenv install 5.24.0
    plenv shell 5.24.0
    perl my-app.pl

This is pretty similar to Perlbrew, but here the similarities end, as Plenv has got another pretty clever way of figuring out which Perl version it is you are really interested in running.

If you run `plenv shell ...` you set the Perl version you want to use for that shell session. This is roughly the same as `perlbrew use ...`. If no specific version has been set for the shell then plenv looks in the current directory to see if there is a `.perl-version` file there. The command `plenv local ...` will write that file for you. Such a file may contain the text `5.24.0` to specify that every perl invocation under this directory should use that version of Perl. If no such file is found in the current directory it traverses upwards looking to see if it has been set further up in the file hierarchy. If unsuccessfil it will look for a `.perl-version` in your home directory. The command `plenv global ...` will write the global file for you.

# Goal 2: isolate dependencies for project A from project B, C and D

Perlbrew has a pretty good system for isolating dependencies and let you create separate "library collections" named somewhat intuitively. Here is an example that creates a library for the FooBar-project and makes everything ready for you to install modules into:

    perlbrew lib create perl-5.24.0@FooBar
    perlbrew lib use perl-5.24.0@FooBar

Anything installed by eg. `cpanm` after that would go in a separate directory and not interfere with the libraries for your other important project BarFooBaz. Perlbrew has a lot of nice features build around these library-commands and I recommend looking at [the documentation](https://metacpan.org/pod/distribution/App-perlbrew/bin/perlbrew#COMMAND:-LIB) to get a full understaning of what you can do with the `lib` series of subcommands. All in all Perlbrew is a great tool and the lib-handling is an area where it really shines.

Plenv does not seem to have been written to that end, but perhaps have been made to make installing and managing Perl simpler and more streamlined. For Plenv you have [plenv-contrib](https://github.com/miyagawa/plenv-contrib) that seek to add such things as Perlbrew-like lib-support. I did try plenv-contrib for a while but did not find it as natural to use as the Perlbrew alternative.

If you simplyfi things a bit then you might say that all both Perlbrew and Plenv does is set a couple of environment variables, namely so that cpan(m), Module::Builder, Module::MakeMaker and Perl itself know where to place and look for things. The variables are

 - `PERL5LIB` : tells perl that additional directories should be looked in for libraries
 - `PERL_MM_OPT` : options for Module::MakeMaker, relevant for installation of modules
 - `PERL_MB_OPT` : options for Module::Builder, relevant for installation of modules
 - `PERL_LOCAL_LIB_ROOT` : options for `local::lib`
 - `PATH` : directory of your perl / libdir is prepended, lets the shell know where to find binaries

The third option for isolation of dependencies is [local::lib](https://metacpan.org/pod/local::lib), and the tasks that the module perform is pretty much set up relevant environment variables and make sure a directory for modules exists. One way to invoke `local::lib` is the following:

    eval "$( perl -Mlocal::lib=local/ - )"

What this does is set the abovementioned environment variables and make sure that the `local/` directory exists and have the required subdirectories. You may also pass options to `local::lib`, even when invoking it this way. One such example is telling `local::lib` to skip making sure the directories exists:

    eval "$( perl -Mlocal::lib=local/,--no-create - )"

I have not looked at all the code for Plenv and Perlbrew, but I assume that they either do the same thing as `local::lib`, and for me it was just as easy to use `local::lib` directly.

# Goal 3: lock versions of modules you depend on

This is big topic and one that could be covered in multiple posts, as there are a several ways of locking versions. I will however not go into great detail here, but rather give some information on how I do it for the majority of the projects I work on. Some of the ways to lock down dependencies are:

 - Download required version from CPAN, put somewhere safe and copy from there when you need to use / install. It is primitive, but it works.
 - Specify exact version of dependencies in a `cpanfile`, and gamble that they will be available through CPAN when you want to install.
 - Roll your own PAN (there are multiple projects that let you do this).
 - Use a service like [Stratopan](http://stratopan.com) (in combination with a `cpanfile`) that seeks to sort of be a hosted PAN.
 - Store distributions youself using eg. [Pinto](https://metacpan.org/release/Pinto), which Stratopan is based upon.

I think it is worth noting that many people advocate for the use of [Carton](https://metacpan.org/release/Carton), though I have yet to find it terribly convenient for my own use. It might also be abandoned by it's creator in favor of work on [Carmel](https://metacpan.org/release/Carmel), a projects that seems to be aimed at solving some of Cartons shortcomings and design-issues. If you're just starting out then looking at Carton is probably a good idea and you might even find that Carton is the tool best suited for your particular use.

Since 2014 we have been using Stratopan at `$work`. This has let us upload our own distributions, pull distributions from CPAN and be sure (for the most part) that we can install the exact versions we want every time we deploy. Stratopan is as mentioned based on Pinto and does offer an easy to use web interface. We have however missed a push to make the service pay-to-use. Using something that is in beta is fine for a period, but as a business we want certain assurances of service levels, support etc. As far as I know Stratopan has been at a stand-still since 2014. It is worth noting that Stratopan is the baby of the very talented [Jeffrey Thalhammer](https://metacpan.org/author/THALJEF), and my **guess** is that he is not exactly lacking paid work. Given it is so then I can understand him not pushing Stratopan into a more commercial venture.

We have also used Pinto for a while, mostly to reduce the time being spent downloading distributions from CPAN/Stratopan. Pinto is a great piece of software, easy to set up and maintain, and fairly easy to use. We have opted to have versions of modules locked in Stratopan and Pinto, and let our [cpanfile](https://metacpan.org/pod/distribution/Module-CPANfile/lib/cpanfile.pod) contain no information about versions. Removing versioning-information from the `cpanfile` makes it real easy to test your projects codebase with the same modules as your main deploy stack, though with different versions, for example during a process of figuring out which modules are safe to upgrade. In any case, one of our `cpanfile` could look something like this:

    requires 'Mojolicious';
    requires 'Mojo::Pg';
    requires 'Text::CSV';

This lets us install all the required modules for a project with the following command:

    $ cpanm --mirror https://your-server.com/your-project/the-stack --mirror-only --local-lib local/ --self-contained --installdeps .

The magic is in `--installdeps .` which instructs `cpanm` to read the `cpanfile` and install all dependencies listed therein. We also specify `--mirror https://your-server.com/your-project/the-stack --mirror-only` to let `cpanm` know we want to install our packages from our chosen mirror (our own Pinto, or Stratopan) plus `--local-lib local/ --self-contained` to make sure that `cpanm` does not try to install the modules and make them globally available.

My recommendation is to set up Pinto yourself or use a service like Stratopan, as this gives you near total control of your dependencies in addition to giving you the flexibility of having separate "stacks" under a particular project. This is useful for testing upgrades and maintaining several branches of a project that have dependencies that are incompatible. One such example is our companys main product, which has a "legacy" branch that is maintained alongside the current version of the software. The legacy branch is on Perl 5.14.4 with [Mojolicious](http://mojolicious.org/) 3.97, whilst the current version is on Perl 5.24.0 and Mojolicious 6.x .

# The conclusion

There are many ways of doing thing in and with Perl, and this write-up is mostly a collection of my thoughts on _some_ of the options that exists. My recommendation is to familiarize yourself with all of the above, so you can make a properly informed decision when choosing how to move forward. My preference is simply this:

 - use `plenv` to manage the perls you use
 - use `local::lib` to control where Perl gets to look for modules
 - use a cpanfile to track which modules your project depend on
 - lock versions of modules somewhere (Pinto, Stratopan, others)
 - install dependencies using `cpanm --installdeps`

If there is anything you feel relevant that has been left out then please let me know. Please also comment if this write-up is too high level and not practical / technical enough. I'd be happy to follow up with some more technical posts.
