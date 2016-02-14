---
layout: post
title: "grand central dispatch tutorial for swift part 1"
description: ""
category: [ios]
tags: [gcd, serial, concurrent, synchronous, asynchronous, critical, race, deadlock, thread safe, parallelism, queue]
---
{% include JB/setup %}

### grand central dispatch

    gcd for short
    c-based api

### getting started

1. gcd is the marketing name for libdispatch

        provides support for concurrent code execution on multicore hardware on iOS and OS X

    * gcd can improve app's responsiveness by helping you defer computationally expensive task and run then in the background

    * gcd provides an easier concurrency model than locks and threads and helps to avoid concurrency bugs

1. serial vs. concurrent

    * tasks executed serially are always executed one at a time

    * tasks executed concurrently might be executed at the same time

1. task

    * you can consider a task to be a closure

            closures are self-contained
            callable blocks of code
            can be stored and passed around

            when called, they behave like funcs
            and can have parameters and return values

            closure "captures" variables it uses form outside its own scope
            it sees the variables from the enclosing scope and remembers their value

            swift closures are similar to objective-c blocks
            they are almost entirely interchangeable
            only limitation is objective-c cannot interact with swift closures
            that expose swift-only language features

    * you can also use gcd with func pointers

1. synchronous vs. asynchronous

        these terms describe when a func will return control to the caller
        and how much work will have been done by that point

    * synchronous func returns only after the completion of a task

    * asynchronous func returns immediately

            ordering the task to be done but not waiting for it
            asynchronous func does not block the current thread of execution from proceeding on to the next func

1. critical section

        this is piece of code that must not be executed concurrently
        this is usually because the code manipulates a shared resource

1. race condition

        this is a situation where the behavior of a software system
        depends on a specific sequence or timing of events that execute in an uncontrolled manner

1. deadlock

        two (or sometimes more) items in most cases threads
        are said to be deadlocked if they all get stuck
        waiting for each other to complete or perform another action

        the first can't finish because it's waiting for the second to finish
        but the second can't finish because it's waiting fro the first to finish

1. thread safe

        thread safe code can be safely called
        from multiple thread or concurrent tasks
        without causing any problems
        (data corruption, crashing, etc.)

1. context switch

        a context switch is the process of storing and restoring execution state
        when you switch between executing different threads on a single process

1. concurrency vs. parallelism

        separate parts of concurrent code can be executed "simultaneously"
        it's up to the system to decide how this happens

        multi-core devices execute multiple threads at the same time via parallelism

        for single-cored devices to achieve this
        they must run a thread perform a context switch
        ten run another thread or process

                    |
                    |
                    |
        parallelism |---------------------------------------- thread 1
                    |    \
                    |     `---------------------------------- thread 2
                    |
                    |
        concurrency |------    --    -------     --  ----     thread 3
                    |    \
                    |     `----  ----       -----  --    ---- thread 4
                    |            ^^^^
                    |            ||||
                    |       context switch
        ------------+--------------------------------------------
                    |   time --->

    * it's up to gcd to decide how much parallelism is required

    * parallelism requires concurrency

    * but concurrency does not guarantee parallelism

### queues

        gcd provides dispatch queues to handle submitted tasks
        these queues manage the tasks you provides to gcd
        and execute those tasks in fifo order

        all dispatch queues are themselves thread-safe

1. serial queues

    * tasks in serial queues execute one at a time

    * each task starting only after the preceding task has finished

    * gcd executed only one task at a time

                            |
                            |
                            |
            serila queues   | [task 0.....][task 1..][task 2........][task 3..]
                            |
                            |
            ----------------+--------------------------------------------
                            |   time --->

1. concurrent queues

    * tasks in concurrent queues are guarantee to start in the order ther were added

                                |
                                | [task 0.....]
                                |    [task 1..]
            concurrent queues   |     [task 2........]
                                |           [task 3..]
                                |
            --------------------+--------------------------------------------
                                |   time --->

1. queue types

    * QOS_CLASS_USER_INTERACTIVE

            tasks that need to be done immediately
            in order to provode a nice ue

            use it for ui updates, event handling
            and small workload that require low latency

    * QOS_CLASS_USER_INITIATED

            tasks that are initiated form the ui
            and can be performed asynchronously

            it should be used when the user is waiting for
            immediate results
            and for tasks required to continue user interaction

    * QOS_CLASS_UTILITY

            long-running tasks, typically with a user-visible progress indicator

            use it for computations, i/o networking
            continous data feeds and similar tasks

            this class is designed to be energy efficient

    * QOS_CLASS_BACKGROUND

            tasks that user is not directly aware of

            use it for prefetching, maintenance
            and other tasks that don't require user
            interaction and aren't time-sensitive

    * finally you can create your own custom serial or concurrent queues

            that means you have at least five queues at your disposal
            the main queue
            four global dispatch queues
            plus any custom queues