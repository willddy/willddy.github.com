---
layout: post
uri: /posts/118
permalink: /posts/118/index.html
title: Hadoop Split and Block
category: bigdata
tag: hadoop
description: 
disqus: true 
lang: en
---
<p>HDFS was designed to hold and manage large amounts of data; a default block size is 64 MB. That means if a 128-MB text file was put in to HDFS, HDFS would split the file into two blocks (128 MB/64 MB) and distribute the two chunks to the data nodes in the cluster. The main reason to setting proper block size is to balance the load of each map and reduce throughput so that the overall job takes better time to finish. Because the block size is the boundary of split, so it is a very important setting. The best way to decide the proper size is to  understand your data profile (average size, etc) of your business.</p>
<p>To set up the block size, you need to modify/override setting in hdfs-size.xml:</p>
<pre>&lt;property&gt;
&lt;name&gt;dfs.block.size&lt;name&gt;
&lt;value&gt;134217728&lt;value&gt;
&lt;description&gt;Block size&lt;description&gt;
&lt;property&gt;</pre>
<p>Note, changing this setting will not affect the block size of any files currently in HDFS. It will only affect the block size of files placed into HDFS
after this setting has taken effect. That means you need to reload the data or put new data using "hadoop fs -put" to make setting effective on the data.</p>
<p>Below is summary between split and block</p>
<ul>
<li>One split must contain n times of blocks where n is integer and big than 0</li>
<li>A split will not contain blocks from two files or split will not cross file boundaries</li>
<li>One split is mapping to multiple blocks</li>
<li>The number of working map tasks depends on the number of splits</li>
</ul>
<p>By the way, if something is not split-able, we can consider warrap them into Avro or Sequencefile for instead</p>

<a href="http://i.imgur.com/OuU4CYW.png" target="_blank"><img src="http://i.imgur.com/OuU4CYW.png" width="700" height="330" /></a>
