---
layout: post
title: "storm stream grouping"
description: ""
category: [realtime]
tags: [storm, stream, grouping]
---
{% include JB/setup %}


### stream grouping

1. all grouping

            a.1 -> b.1 100%
            a.1 -> b.2 100%
            a.2 -> b.1 100%
            a.2 -> b.2 100%

            +--------------------+      +--------------------+
            |       bolt a       |      |       bolt b       |
            |   +------------+   |      |   +------------+   |
            |   | instance 1 |   |      |   | instance 1 |   |
            |   +------------+   |      |   +------------+   |
            |   +------------+   |      |   +------------+   |
            |   | instance 2 |   |      |   | instance 2 |   |
            |   +------------+   |      |   +------------+   |
            +--------------------+      +--------------------+

    1. sends tuple to all instances of the receiving bolt

    1. used to send signals to bolts

            // bolt execute
            public void execute(Tuple input){
                String str = null;
                try{
                    if (input.getSourceStreamId().equals("signals")){
                        str = input.getStringByField("action");
                        if("refreshCache".equals(str)){
                            counters.clear();
                        }
                    }
                }catch (IllegalArgumentException e){
                    // do nothing
                }
            }

            // topology builder
            builder.setBolt("word-counter", new WordCounter(), 2)
                .fieldGrouping("word-normalizer", new Fields("word"))
                .allGrouping("signals-spout", "signals");

1. custom grouping

    1. impl `backtype.storm.grouping.CustomStreamGrouping` interface

            public class ModuleGrouping implements CustomStreamGrouping, Serializable
            {
                int numTasks = 0;

                @Override
                public List<Integer> chooseTasks(List<Object> values) {
                    List<Integer> boltIds = new ArrayList();
                    if(values.size()>0){
                        String str = values.get(0).toString();
                        if(str.isEmpty()){
                            boltIds.add(0);
                        }
                        else{
                            boltIds.add(str.charAt(0) % numTasks);
                            //          ************************
                        }
                    }
                    return boltIds;
                }

                @Override
                public void prepare(TopologyContext context, Fields outFields, List<Integer> targetTasks) {
                    numTasks = targetTasks.size();
                }
            }

1. direct grouping

    1. source decides which component will receive the tuple

            // 1. execute
            public void execute(Tuple input){
                // ...
                for (String word: words){
                    if(!word.isEmpty()){
                        // ...
                        collector.emitDirect(getWordCountIndex(word), new Value(word));
                        //        **********************************
                    }
                }
                // acknowledge the tuple
                collector.ack(input);
            }

            // 2. getWordCountIndex
            public Integer getWordCountIndex(String word){
                word = word.trim().toUpperCase();
                if(word.isEmpty())
                {
                    return 0;
                }
                else{
                    return word.charAt(0) % numCounterTasks;
                    //     ********************************
                }
            }

            // 3. work out the number of target tasks in the `prepare` method
            public void prepare(Map stormConf, TopologyContext context, OutputCollector collector){
                this.collector = collector;
                this.numCounterTasks = context.getComponentTasks("word-counter");
            }

            // 4. topology definition
            builder.setBolt("word-counter", new WordCounter(), 2)
                .directGrouping("word-normalizer");

1. field grouping

            a.1 -> b.1 field p
            a.2 -> b.1 field p
            a.2 -> b.2 field q

            +--------------------+      +--------------------+
            |       bolt a       |      |       bolt b       |
            |   +------------+   |      |   +------------+   |
            |   | instance 1 |   |      |   | instance 1 |   |
            |   +------------+   |      |   +------------+   |
            |   +------------+   |      |   +------------+   |
            |   | instance 2 |   |      |   | instance 2 |   |
            |   +------------+   |      |   +------------+   |
            +--------------------+      +--------------------+


    1. based on one or more fields of the tuple

            builder.setBolt("word-counter", new WordCounter(), 2)
                .fieldGrouping("word-normalizer", new Fields("word"));

1. global grouping

            a.1 -> b.1 100%
            a.2 -> b.1 100%

            +--------------------+      +--------------------+
            |       bolt a       |      |       bolt b       |
            |   +------------+   |      |   +------------+   |
            |   | instance 1 |   |      |   | instance 1 |   |
            |   +------------+   |      |   +------------+   |
            |   +------------+   |      |   +------------+   |
            |   | instance 2 |   |      |   | instance 2 |   |
            |   +------------+   |      |   +------------+   |
            +--------------------+      +--------------------+

    1. `all instances` of the source to a `single target instance`

    1. specifically the task with `lowest id`

1. local grouping

    1. same as `shuffle grouping`

1. none grouping

    1. same as `shuffle grouping`

1. shuffle grouping

            a.1 -> b.1 50%
            a.1 -> b.2 50%
            a.2 -> b.1 50%
            a.2 -> b.2 50%

            +--------------------+      +--------------------+
            |       bolt a       |      |       bolt b       |
            |   +------------+   |      |   +------------+   |
            |   | instance 1 |   |      |   | instance 1 |   |
            |   +------------+   |      |   +------------+   |
            |   +------------+   |      |   +------------+   |
            |   | instance 2 |   |      |   | instance 2 |   |
            |   +------------+   |      |   +------------+   |
            +--------------------+      +--------------------+

    1. takes a single parameter (the source component)

    1. sends each tuple emitted by the source to a randomly chosen bolt

    1. `load balancing` is automatically taken care

    1. useful only for `atomic operations` by specifying a single parameter
