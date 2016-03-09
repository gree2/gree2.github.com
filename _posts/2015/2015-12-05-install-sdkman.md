---
layout: post
title: "install sdkman"
description: ""
category: [setup]
tags: [curl, source, sdkman]
---
{% include JB/setup %}


### [installation](http://sdkman.io/install.html)

1. description

    1. a tool

    1. managing parallel versions of multiple `software development kits`

    1. on most unix based systems

    1. provides `cli` and `api`

    1. installing, switching, removing, listing candidates

    1. inspired by [rvm](https://rvm.io/) and [rbenv](https://github.com/sstephenson/rbenv)

1. install to default location

    1. open a terminal and run

            $ curl -s get.sdkman.io | bash

            Thanks for using...                                                          
                                                                                         
                                                                                         
                 SSSSSSSSSSSSSSS DDDDDDDDDDDDD       KKKKKKKKK    KKKKKKK                  
               SS:::::::::::::::SD::::::::::::DDD    K:::::::K    K:::::K                  
              S:::::SSSSSS::::::SD:::::::::::::::DD  K:::::::K    K:::::K                  
              S:::::S     SSSSSSSDDD:::::DDDDD:::::D K:::::::K   K::::::K                  
              S:::::S              D:::::D    D:::::DKK::::::K  K:::::KKK                  
              S:::::S              D:::::D     D:::::D K:::::K K:::::K                     
               S::::SSSS           D:::::D     D:::::D K::::::K:::::K                      
                SS::::::SSSSS      D:::::D     D:::::D K:::::::::::K                       
                  SSS::::::::SS    D:::::D     D:::::D K:::::::::::K                       
                     SSSSSS::::S   D:::::D     D:::::D K::::::K:::::K                      
                          S:::::S  D:::::D     D:::::D K:::::K K:::::K                     
                          S:::::S  D:::::D    D:::::DKK::::::K  K:::::KKK                  
              SSSSSSS     S:::::SDDD:::::DDDDD:::::D K:::::::K   K::::::K                  
              S::::::SSSSSS:::::SD:::::::::::::::DD  K:::::::K    K:::::K                  
              S:::::::::::::::SS D::::::::::::DDD    K:::::::K    K:::::K                  
               SSSSSSSSSSSSSSS   DDDDDDDDDDDDD       KKKKKKKKK    KKKKKKK                  
                                                                                           
                                                                                           
                                  mmmmmmm    mmmmmmm     aaaaaaaaaaaaa  nnnn  nnnnnnnn     
                                mm:::::::m  m:::::::mm   a::::::::::::a n:::nn::::::::nn   
                               m::::::::::mm::::::::::m  aaaaaaaaa:::::an::::::::::::::nn  
                               m::::::::::::::::::::::m           a::::ann:::::::::::::::n 
                               m:::::mmm::::::mmm:::::m    aaaaaaa:::::a  n:::::nnnn:::::n 
                               m::::m   m::::m   m::::m  aa::::::::::::a  n::::n    n::::n 
                               m::::m   m::::m   m::::m a::::aaaa::::::a  n::::n    n::::n 
                               m::::m   m::::m   m::::ma::::a    a:::::a  n::::n    n::::n 
                               m::::m   m::::m   m::::ma::::a    a:::::a  n::::n    n::::n 
                               m::::m   m::::m   m::::ma:::::aaaa::::::a  n::::n    n::::n 
                               m::::m   m::::m   m::::m a::::::::::aa:::a n::::n    n::::n 
                               mmmmmm   mmmmmm   mmmmmm  aaaaaaaaaa  aaaa nnnnnn    nnnnnn 
                                                                                             
                                                                                             
                                                             Now attempting installation...
                                                                                           
            Looking for a previous installation of SDKMAN...
            Looking for unzip...
            Looking for curl...
            Looking for sed...
            Installing SDKMAN scripts...
            Create distribution directories...
            Getting available candidates...
            Prime the config file...
            Download script archive...
            Extract script archive...
            Install scripts...
            Set version to 3.2.4 ...
            Attempt update of bash profiles...
            Updated existing /Users/hqlgree2/.bash_profile
            Created and initialised /Users/hqlgree2/.bashrc
            Attempt update of zsh profiles...
            Created and initialised /Users/hqlgree2/.zshrc



            All done!


            Please open a new terminal, or run the following in the existing one:

                source "/Users/hqlgree2/.sdkman/bin/sdkman-init.sh"

            Then issue the following command:

                sdk help

            Enjoy!!!

    1. open a new terminal

            $ source "$HOME/.sdkman/bin/sdkman-init.sh"

    1. check to ensure

            $ sdk version

            ==== BROADCAST =================================================================
            * 03/12/15: Grails 3.0.10 released on SDKMAN! #grailsfw
            * 03/12/15: Grails 3.1.0.M3 released on SDKMAN! #grailsfw
            * 26/11/15: Maven 3.3.9 released on SDKMAN! #maven
            ================================================================================
            SDKMAN 3.2.4

1. install to a custom location

            $ export SDKMAN_DIR="/usr/local/sdkman" && curl -s get.sdkman.io | bash

### usage

1. install

    1. install the `latest stable`

            $ sdk install gradle

    1. install specific version

            $ sdk install scala 2.11.7

    1. install  local version

            $ sdk install grails 3.1.0.SNAPSHOT /path/to/grails-3.1.0-SNAPSHOT

1. remove

            $ skd remove scala 2.11.6

1. list candidates

            $ sdk list

1. version

    1. list versions

            $ sdk list groovy

    1. use version

            $ sdk use scala 2.11.6

    1. default version

            $ sdk default scala 2.11.6

    1. current version(s)

            $ sdk current grails

            # all candidates
            $ sdk current

    1. outdated version(s)

            $ sdk outdated

1. broadcast messages

            $ sdk broadcast

1. offline mode

            $ sdk offline enable

            $ sdk offline disable

1. self-update

            $ sdk selfupdate
            $ sdk selfupdate force

1. flush

    1. candidates

            $ sdk flush candidates

    1. broadcast

            $ sdk flush broadcast

    1. archives

            $ sdk flush archives

    1. temporary folder

            $ sdk flush temp