---
layout: post
title: "alias for git"
description: ""
category: [command, git]
tags: [command, git, alias, add, status, push, pull, origin, master]
---
{% include JB/setup %}


### .bash_profile

# git
alias gst="git status"
alias gad="git add ."
alias gaa="git add -A"
function __gcm() {
    git commit -m "$*"
}
alias gcm=__gcm
alias gps="git push origin master"
alias gpl="git pull origin master"