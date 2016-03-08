---
layout: post
title: "pygraphviz in depth"
description: ""
category: [viz]
tags: [pygraphviz, graphviz, node, attrs]
---
{% include JB/setup %}


### graph

1. draw direction `rankdir`

    1. `LR` left   -> right

    1. `RL` right  -> left

    1. `TB` top    -> bottom

    1. `BT` bottom -> top

### node

1. attrs

    1. `shape` ![shape](https://raw.githubusercontent.com/gree2/hobby/master/python/p.pygraphviz/node.shape.png)

            assembly
            box
            box3d
            cds
            circle
            component
            cylinder
            diamond
            doublecircle
            doubleoctagon
            egg
            ellipse
            fivepoverhang
            folder
            hexagon
            house
            insulator
            invhouse
            invtrapezium
            invtriangle
            larrow
            lpromoter
            Mcircle
            Mdiamond
            Msquare
            none
            note
            noverhang
            octagon
            oval
            parallelogram
            pentagon
            plain
            plaintext
            point
            polygon
            primersite
            promoter
            proteasesite
            proteinstab
            rarrow
            rect
            rectangle
            restrictionsite
            ribosite
            rnastab
            rpromoter
            septagon
            signature
            square
            star
            tab
            terminator
            threepoverhang
            trapezium
            triangle
            tripleoctagon
            underline
            utr

### edge

1. multinode to one

            node1 -> node4;
            node2 -> node4;
            node3 -> node4;

            /* equal to */
            
            {node1, node2, node3} -> node4;
