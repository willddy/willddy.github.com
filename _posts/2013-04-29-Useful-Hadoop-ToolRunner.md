---
layout: post
uri: /posts/119
permalink: /posts/119/index.html
title: Useful Hadoop ToolRunner
category: bigdata
tag: hadoop
description: 
disqus: true 
lang: en
---
Developers are pissed off with following things quite often:

* When you write job configuration in the code of map and reduce, you need to repack everything if there changes on paramenters
* You need to write java code to add files in DistributedCache
* When you depend on 3rd party jar or files, you hold code them in java code

All of these can be resolved by using ToolRunner (it runs tools) so that you code could take input of configuration from args. Actually ToolRunner hide GenericOptionsParser, which parce the command line input parameters, in its run (this is not static method) method.

    bin/hadoop jar MyJob.jar com.xxx.MyJobDriver -Dmapred.reduce.tasks=5 \
    -files ./dict.conf  \
    -libjars lib/commons-beanutils-1.8.3.jar,lib/commons-digester-2.1.jar

The ToolRunner class will automatically handle the following generic Hadoop arguments:

* -conf: Takes a path to a parameter configuration file.
* -D: Used to specify Hadoop key/value properties which will be added to the job configuration. There is no space after -D to make difference from hadoop streaming -D option
* -fs: Used to specify the host port of the NameNode
* -jt: Used to specify the host port of the JobTracker

Most of time we also extends Configed class for the purpose of accessing `getConf()`, `getClass()` local method so that you do not need to create configuration objects by hard coding class name.

    public class WordCount extends Configured implements Tool {

        @Override
         public int run(String[] arg0) throws Exception {
            Job job = new Job();
            job.setJarByClass(getClass());
            Configuration conf = getConf();
            FileSystem fs = FileSystem.get(conf);
        }

        public static void main(String[] args) throws Exception {
            int res = ToolRunner.run(new Configuration(),new WordCount(),args);
            System.exit(res);
        }
    }


