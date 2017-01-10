---
layout: post
title: "graphviz dot language"
description: ""
category: [viz]
tags: [dot, graphviz, install]
---
{% include JB/setup %}


### install

1. on mac

            brew install graphviz

### language

1. subgraphs and clusters

    1. roles in graphviz

            # first role

            A -> {B C}

            # is equivalent to
            A -> B
            A -> C

            # 2 second role

            subgraph {
                rank = same; A; B; C;
            }

    1. lexical and semantic notes

            # a graph must be `digraph` or `graph`
            strict graph {
                a -- b
                a -- b
                b -- a [color=blue]
            }

1. output formats

1. node edge and graph attributes

    1. all graphviz attrs are specified by `name-value` pairs

            abc [fillcolor=red]

            abc -> def [arrowhead=diamond]

            # `color`
            digraph G {
                a -> b [dir=both color="red:blue"]
                c -> d [dir=none color="green:red;0.25:blue"]
            }

            # `constraint`
            digraph G {
                a -> c;
                a -> b;
                b -> c [constraint=false]
            }

    1. attribute type description

            arrowType
            normal
            inv
            dot
            invdot
            odot
            invodot
            none
            tee
            empty
            invempty
            diamond
            odiamond
            ediamond
            crow
            box
            obox
            open
            halfopen
            vee
            circle

            # dirType
            forward
            back
            both
            none

            # basic style settings for nodes
            solid *
            dashed
            dotted
            bold
            rounded
            diagonals
            filled
            striped
            wedged

            # basic style settings for edges
            solid *
            dashed
            dotted
            bold

            # basic style settings for clusters
            solid
            dashed
            dotted
            bold
            rounded
            filled
            striped

1. node shapes

    1. polygon-based

            box
            polygon
            ellipse
            oval
            circle
            point
            egg
            triangle
            ...

            digraph G {
                {
                    node [margin=0 fontcolor=blue fontsize=32 width=0.5 shape=circle style=filled]
                    b [fillcolor=yellow fixedsize=true label="a very long label"]
                    d [fixedsize=shape label="an even longer label"]
                }
                a -> {c d}
                b -> {c d}
            }

    1. record-based

    1. user-defined

1. colors