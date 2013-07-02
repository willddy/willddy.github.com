---
title: Add Backup Master Node in HBase Cluster 
layout: post
guid: urn:uuid:04aadc1c-8153-42ec-8c45-201307020953
tags:
  - hbase
---
How to add a backup master node to the cluster? There are two ways of doing that.

#### In One Way

`hadoop@master2$ $HBASE_HOME/bin/hbase-daemon.sh start master`<br>
From the master log, you will find that the newly started master is waiting to become the next active master:<br>
`60000,1328878644330; waiting to become the next active master`<br>







`hadoop@master1$ echo "master2" >> $HBASE_HOME/conf/backup-masters`



`hadoop@master1$ $HBASE_HOME/bin/start-hbase.sh`<br>
`starting master, logging to /usr/local/hbase/logs/hbase-hadoop-master-master1.out`<br>


`hadoop@master2$ tail /usr/local/hbase/logs/hbase-hadoop-master-master2.log`<br>
`is the active master, ip-10-176-201-128.us-west-1.compute.internal,`<br>
`60000,1328883123244; waiting to become the next active master`


