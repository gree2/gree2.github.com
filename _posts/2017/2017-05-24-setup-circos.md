---
layout: post
title: "setup circos"
description: ""
category: [setup, viz]
tags: [circos, perl, cpan]
---
{% include JB/setup %}


### steps

1. from tgz

    1. download [circos-0.69-5.tgz](http://circos.ca/distribution/circos-0.69-5.tgz) and [circos-tutorials-0.67.tgz](http://circos.ca/distribution/circos-tutorials-0.67.tgz)

            $ curl -O http://circos.ca/distribution/circos-0.69-5.tgz
            $ curl -O http://circos.ca/distribution/circos-tutorials-0.67.tgz
            $ tar zxvf circos-0.69-5.tgz
            $ tar zxvf circos-tutorials-0.67.tgz

    1. check missing modules

            $ cd circos-0.69-5
            $ bin/circos -modules
            -bash: /bin/env: No such file or directory

            $ which env
            /usr/bin/env

            # change 1st line in bin/circos
            #!/usr/bin/env perl

    1. install missing modules

            $ which perl
            /usr/bin/perl

            $ perl -v
            this is perl 5, version 18, subversion 2...

            $ sudo perl -MCPAN -e shell
            cpan[1]> install Config::General
            cpan[1]> install Font::TTF::Font
            cpan[1]> install GD
            cpan[1]> install GD::Polyline
            cpan[1]> install Math::Bezier
            cpan[1]> install Math::VecStat
            cpan[1]> install Readonly
            cpan[1]> install SVG
            cpan[1]> install Set::IntSpan
            cpan[1]> install Statistics::Basic

1. using brew

    1. steps

            $ brew tap homebrew/science
            $ brew remove gd
            $ brew install gd --with-freetype
            $ brew install cpanminus
            $ sudo chown "$USER":admin /Library/Perl/5.16 # on macOS
            $ cpanm Config::General Font::TTF::Font Math::Bezier Math::VecStat Readonly Set::IntSpan Text::Format
            $ cpanm --force GD::Polyline # Building and testing GD-2.66 ... FAIL
            $ brew install circos
            $ brew test circos
