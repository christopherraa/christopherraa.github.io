---
layout: post
title: "Perlbrew and Perl 5.14.4 failing tests"
permalink: 2015-02-18-perlbrew-and-perl-5-14-4-failing-tests
---
So today I had to compile Perl 5.14.4 on a new box running Ubuntu 14.10. Followed the normal procedures for getting [Perlbrew](http://perlbrew.pl) installed, updated patchperl and installed the Perlbrew cpanm. Then went ahead to install perl 5.14.4.

|1|perlbrew install perl-5.14.4 -Duseshrplib|

However, this process fails due to tests of perl failing. As far as I’ve been able to understand this is becase Ubuntu 14.10 is shipping with GCC 4.9 as default, and there are some issues with compiler-flags changing between versions. See [this bug](https://rt.perl.org/Public/Bug/Display.html?id=121505) for more technical details. After fiddling around a little bit I decided to just make Perlbrew skip the tests. How smart this is can be debated, but I have yet to experience any unpleasant effects from this.

|1|perlbrew --notest install perl-5.14.4 -Duseshrplib|

If the above feels unpleasant you can always install gcc-4.6 through apt, set it as default using /etc/alternatives and then having a go at compiling Perl. That works as well. The third option is fiddling with the CPAN-config, setting the CC make_arg so that it points to your gcc-4.6 binary.

Oh, and by the way; the reason I have -Duseshrplib in there is because I need Imagemagick, and the only viable option for getting that with Perlbrew is AFAIK using [Alien::ImageMagick](https://metacpan.org/pod/Alien::ImageMagick), which requires perl to be built with that flag.
