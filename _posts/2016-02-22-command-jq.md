---
layout: post
title: "command jq"
description: ""
category: [command]
tags: [jq, json, filter, map, transform, stream]
---
{% include JB/setup %}


### [info](https://stedolan.github.io/jq/)

1. is a lightweight and flexible command line json processor

    1. jq is like `sed` for json data

            use it to slice and filter and map and transform

    1. written in portable c

            has zero runtime dependencies

### [tutorial](https://stedolan.github.io/jq/tutorial/)

1. use github json api

    1. get last 5 commits from the jq repo

            $ curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5'

    1. the simplest jq program is the expression `.`

            $ curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.'

    1. extract just the first commit

            $ curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.[0]'

    1. leave out the `curl` command

            $ ... | jq '.[0] | {message: .commit.message, name: .commit.committer.name}'

            # the | operator in jq feeds output of one filter `.[0]`
            # into the input of another `{...}`
            # you can access nested attributes, such as `.commit.message`

    1. get the rest of the commits

            $ ... | jq '.[] | {message: .commit.message, name: .commit.committer.name}'

            # `.[]` returns each element of the array returned in the response, one at a time
            # which are all fed into `{message: .commit.message, name: .commit.committer.name}`

            # data in jq represented as streams of json values
            # streams are serialised by just separating json values with whitespace
            # this is a `cat` friendly format
            # join 2 json streams together and get a valid json stream

    1. get output as a single array

            $ ... | jq '[.[] | {message: .commit.message, name: .commit.committer.name}]'

    1. pull out all of the `html_url` fields inside that array of parent commits

            # and make a simple list of strings
            # to get along with the `message` and `author` fields

            $ ... | jq '[.[] | {message: .commit.message, name: .commit.committer.name, parents: [.parents[].html_url]}]'

### [manual](https://stedolan.github.io/jq/manual/)
