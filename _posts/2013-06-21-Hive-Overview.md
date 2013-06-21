---
title: Hive Overview
layout: post
guid: urn:uuid:04aadc1c-8153-42ec-8c45-201306211158
tags:
  - hive
---
#Hive Overview
______________
<ul>
<li class="toclevel-1 tocsection-1"><a href="#About_Hive"><span class="tocnumber">1</span> <span class="toctext">About Hive</span></a>
<ul>
<li class="toclevel-2 tocsection-2"><a href="#Hive_Architecture"><span class="tocnumber">1.1</span> <span class="toctext">Hive Architecture</span></a></li>
<li class="toclevel-2 tocsection-3"><a href="#Compareing_Hive_and_RDBMS"><span class="tocnumber">1.2</span> <span class="toctext">Compareing Hive and RDBMS</span></a></li>
<li class="toclevel-2 tocsection-4"><a href="#Hive_Meta_Data_Management"><span class="tocnumber">1.3</span> <span class="toctext">Hive Meta Data Management</span></a></li>
<li class="toclevel-2 tocsection-5"><a href="#Hive_Data_Structure"><span class="tocnumber">1.4</span> <span class="toctext">Hive Data Structure</span></a></li>
</ul>
</li>
<li class="toclevel-1 tocsection-6"><a href="#HQL_Guideline"><span class="tocnumber">2</span> <span class="toctext">HQL Guideline</span></a></li>
<li class="toclevel-1 tocsection-7"><a href="#Hive_Configuration_Variables"><span class="tocnumber">3</span> <span class="toctext">Hive Configuration Variables</span></a></li>
<li class="toclevel-1 tocsection-8"><a href="#Hive_Extensions"><span class="tocnumber">4</span> <span class="toctext">Hive Extensions</span></a></li>
<li class="toclevel-1 tocsection-9"><a href="#Performance_Tuning"><span class="tocnumber">5</span> <span class="toctext">Performance Tuning</span></a>
<ul>
<li class="toclevel-2 tocsection-10"><a href="#General_Practice"><span class="tocnumber">5.1</span> <span class="toctext">General Practice</span></a></li>
<li class="toclevel-2 tocsection-11"><a href="#Optimizer"><span class="tocnumber">5.2</span> <span class="toctext">Optimizer</span></a></li>
<li class="toclevel-2 tocsection-12"><a href="#Better_Execution_Plan"><span class="tocnumber">5.3</span> <span class="toctext">Better Execution Plan</span></a></li>
<li class="toclevel-2 tocsection-13"><a href="#Execution_Tuning"><span class="tocnumber">5.4</span> <span class="toctext">Execution Tuning</span></a></li>
</ul>
</li>
<li class="toclevel-1 tocsection-14"><a href="#Reference"><span class="tocnumber">6</span> <span class="toctext">Reference</span></a></li>
</ul>
</td></tr></table>
<h2> <span class="mw-headline" id="About_Hive">About Hive</span></h2>
<ul><li>Hive 是建立在 Hadoop 上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载(ETL),这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制. Hive 定义了简单的类 SQL 查询语言，称为 QL，它允许熟悉 SQL 的用户查询数据. 同时,这个语言也允许熟悉 MapReduce 开发者的开发自定义的 mapper 和 reducer 来处理内建的mapper 和 reducer 无法完成的复杂的分析工作.
</li></ul>
<ul><li>90% percentage of MR jobs are done by Hive in facebook. Usually, it takes 10 minutes to write.
</li></ul>
<h3> <span class="mw-headline" id="Hive_Architecture">Hive Architecture</span></h3>
Hive 的结构可以分为以下几部分： <div class="thumb tright"><div class="thumbinner" style="width:182px;"><a href="/mediawiki/index.php/File:Hive_architecture.jpg" class="image"><img alt="" src="/mediawiki/images/thumb/9/90/Hive_architecture.jpg/180px-Hive_architecture.jpg" width="180" height="235" class="thumbimage" /></a>  <div class="thumbcaption"><div class="magnify"><a href="/mediawiki/index.php/File:Hive_architecture.jpg" class="internal" title="Enlarge"><img src="/mediawiki/skins/common/images/magnify-clip.png" width="15" height="11" alt="" /></a></div>Hive Architecture</div></div></div>
<ul><li>用户接口
<dl><dd>用户接口主要有三个：CLI,Client 和 WUI. 其中最常用的是 CLI. CLI 启动的时候，会同时启动一个 Hive 副本。Client 是 Hive 的客户端，用户连接至 Hive Server。在启动 Client 模式的时候，需要指出 Hive Server 所在节点，并且在该节点启动 Hive Server. WUI 是通过浏览器访问 Hive。 
</dd></dl>
</li><li>元数据存储
<dl><dd>Hive 将元数据存储在数据库中，如 mysql(what we use), derby. Hive 中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等。
</dd></dl>
</li><li>解释器,编译器,优化器,执行器
<dl><dd>解释器、编译器、优化器完成 HQL 查询语句从词法分析、语法分析、编译、优化以及查询计划的生成。生成的查询计划存储在 HDFS 中，并在随后有 MapReduce 调用执行。 
</dd></dl>
</li><li>Hadoop：Hive 构建在 Hadoop 之上用 HDFS 进行存储，利用 MapReduce 进行计算 
<dl><dd>HQL 中对查询语句的解释、优化、生成查询计划是由 Hive 完成的. 所有的数据都是存储在 Hadoop 中.查询计划被转化为 MapReduce 任务,在 Hadoop中执行. Hadoop和Hive都是用UTF-8编码的. <b>Note</b>: 有些查询没有 MR 任务，如：select * from table
</dd></dl>
</li></ul>
<table>

<tr>
<td> <a href="/mediawiki/index.php/File:Hive_Architecture_2.png" class="image" title="Hive Architecture"><img alt="Hive Architecture" src="/mediawiki/images/thumb/2/25/Hive_Architecture_2.png/500px-Hive_Architecture_2.png" width="500" height="277" /></a></td>
<td><a href="/mediawiki/index.php/File:Hiave_system_architecture.png" class="image"><img alt="Hiave system architecture.png" src="/mediawiki/images/thumb/f/fb/Hiave_system_architecture.png/600px-Hiave_system_architecture.png" width="600" height="310" /></a>
</td></tr></table>
<p><br />
</p>
<h3> <span class="mw-headline" id="Compareing_Hive_and_RDBMS"> Compareing Hive and RDBMS</span></h3>
<table class="wikitable">

<tr>
<th> Header text </th>
<th> Hive</th>
<th> RDBMS
</th></tr>
<tr>
<td> 查询语言</td>
<td> HQL</td>
<td> SQL
</td></tr>
<tr>
<td> 数据存储</td>
<td> HDFS </td>
<td> Raw Device or Local FS
</td></tr>
<tr>
<td> 数据更新</td>
<td> 不支持</td>
<td> 支持
</td></tr>
<tr>
<td> 数据格式</td>
<td> 用户定义</td>
<td> 系统决定
</td></tr>
<tr>
<td> Index</td>
<td> Yes after v0.7</td>
<td> Yes
</td></tr>
<tr>
<td> 执行</td>
<td> MapReduce </td>
<td> Excutor
</td></tr>
<tr>
<td> 执行延迟 </td>
<td> 高</td>
<td> 低
</td></tr>
<tr>
<td> 处理数据规模</td>
<td> 大</td>
<td> 小
</td></tr></table>
<ul><li>查询语言
<dl><dd>由于 SQL 被广泛的应用在数据仓库中，因此，专门针对 Hive 的特性设计了类 SQL 的查询语言 HQL。熟悉 SQL 开发的开发者可以很方便的使用 Hive 进行开发。 
</dd></dl>
</li><li>数据存储位置
<dl><dd>Hive 是建立在 Hadoop 之上的，所有 Hive 的数据都是存储在 HDFS 中的。而数据库则可以将数据保存在块设备或者本地文件系统中。 
</dd></dl>
</li><li>数据格式
<dl><dd>Hive 中没有定义专门的数据格式，数据格式可以由用户指定，用户定义数据格式需要指定三个属性：列分隔符（通常为空格、”\t”、”\x001″）、行分隔符（”\n”）以及读取文件数据的方法(Hive 中默认有三个文件格式 TextFile, SequenceFile 以及 RCFile. For details, refer <a rel="nofollow" target="_blank" class="external autonumber" href="http://www.infoq.com/cn/articles/hadoop-file-format%7C1">[1]</a> and <a rel="nofollow" target="_blank" class="external autonumber" href="http://samuschen.iteye.com/blog/834402%7C2">[2]</a>).由于在加载数据的过程中，不需要从用户数据格式到 Hive 定义的数据格式的转换，因此，Hive 在加载的过程中不会对数据本身进行任何修改，而只是将数据内容复制或者移动到相应的 HDFS 目录中。而在数据库中，不同的数据库有不同的存储引擎，定义了自己的数据格式。所有数据都会按照一定的组织存储，因此，数据库加载数据的过程会比较耗时。 
</dd></dl>
</li><li>数据更新
<dl><dd>由于 Hive 是针对数据仓库应用设计的，而数据仓库的内容是读多写少的。因此，Hive 中不支持对数据的改写和添加，所有的数据都是在加载的时候中确定好的。而数据库中的数据通常是需要经常进行修改的，因此可以使用 INSERT INTO ...  VALUES 添加数据，使用 UPDATE ... SET 修改数据。 
</dd></dl>
</li><li>执行
<dl><dd>Hive 中大多数查询的执行是通过 Hadoop 提供的 MapReduce 来实现的（类似 select * from tbl 的查询不需要 MapReduce）。而数据库通常有自己的执行引擎。 
</dd></dl>
</li><li>执行延迟
<dl><dd>之前提到 Hive 在查询数据的时候，由于没有索引，需要扫描整个表，因此延迟较高。另外一个导致 Hive 执行延迟高的因素是 MapReduce 框架。由于 MapReduce 本身具有较高的延迟，因此在利用 MapReduce 执行 Hive 查询时，也会有较高的延迟。相对的，数据库的执行延迟较低。当然，这个低是有条件的，即数据规模较小，当数据规模大到超过数据库的处理能力的时候，Hive 的并行计算显然能体现出优势。 
</dd></dl>
</li><li>可扩展性
<dl><dd>由于 Hive 是建立在 Hadoop 之上的，因此 Hive 的可扩展性是和 Hadoop 的可扩展性是一致的（世界上最大的 Hadoop 集群在 Yahoo!，2009年的规模在 4000 台节点左右)而数据库由于 ACID 语义的严格限制，扩展行非常有限。目前最先进的并行数据库 Oracle 在理论上的扩展能力也只有 100 台左右。 
</dd></dl>
</li><li>数据规模
<dl><dd>由于 Hive 建立在集群上并可以利用 MapReduce 进行并行计算，因此可以支持很大规模的数据；对应的，数据库可以支持的数据规模较小。
</dd></dl>
</li></ul>
<h3> <span class="mw-headline" id="Hive_Meta_Data_Management">Hive Meta Data Management</span></h3>
<p>一般不会在开发中用到。但如果要导出建表语句等，一般可以借助它. <a rel="nofollow" target="_blank" class="external text" href="http://www.imphrack.com/?p=21">Hive导出建表语句</a>
</p>
<h3> <span class="mw-headline" id="Hive_Data_Structure">Hive Data Structure</span></h3>
<p>Hive 没有专门的数据存储格式，用户可以非常自由的组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符和行分隔符，Hive 就可以解析数据. Hive 中所有的数据都存储在 HDFS 中，Hive 中包含以下数据模型：Table, External Table, Partition, Bucket.
</p>
<ul><li>Hive 中的 Table 和数据库中的 Table 在概念上是类似的，每一个 Table 在 Hive 中都有一个相应的目录存储数据。例如，一个表 xiaojun，它在 HDFS 中的路径为：/ warehouse /xiaojun，其中 wh 是在 hive-site.xml 中由 ${hive.metastore.warehouse.dir} 指定的数据仓库的目录，所有的 Table 数据(不包括 External Table)都保存在这个目录中。 
</li><li>Partition 对应于数据库中的 Partition 列的密集索引，但是 Hive 中 Partition 的组织方式和数据库中的很不相同。在 Hive 中，表中的一个 Partition 对应于表下的一个目录，所有的 Partition 的数据都存储在对应的目录中。例如：xiaojun 表中包含 dt 和 city 两个 Partition，则对应于 dt = 20100801, ctry = US 的 HDFS 子目录为：/ warehouse /xiaojun/dt=20100801/ctry=US；对应于 dt = 20100801, ctry = CA 的 HDFS 子目录为；/ warehouse /xiaojun/dt=20100801/ctry=CA 
</li><li>Buckets 对指定列计算 hash，根据 hash 值切分数据，目的是为了并行，每一个 Bucket 对应一个<b>文件</b>。将 user 列分散至 32 个 bucket，首先对 user 列的值计算 hash，对应 hash 值为 0 的 HDFS 目录为：/ warehouse /xiaojun/dt =20100801/ctry=US/part-00000. hash 值为 20 的 HDFS 目录为:/warehouse/xiaojun/dt =20100801/ctry=US/part-00020 
</li><li>External Table 指向已经在 HDFS 中存在的数据，可以创建 Partition. 它和 Table 在元数据的组织上是相同的，而实际数据的存储则有较大的差异。 
<ul><li>Table 的创建过程和数据加载过程(这两个过程可以在同一个语句中完成 using create-table-as-select),在加载数据的过程中，实际数据会被移动到数据仓库目录中；之后对数据对访问将会直接在数据仓库目录中完成。删除表时，表中的数据和元数据将会被同时删除。 
</li><li>External Table 只有一个过程，加载数据和创建表同时完成(CREATE EXTERNAL TABLE ……LOCATION),实际数据是存储在 LOCATION 后面指定的 HDFS 路径中，并不会移动到数据仓库目录中。当删除一个 External Table 时，仅删除
</li></ul>
</li></ul>
<h2> <span class="mw-headline" id="HQL_Guideline">HQL Guideline</span></h2>
<p><a rel="nofollow" target="_blank" class="external text" href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual">HQL Language Manual</a> is very improtant for every hive developer and tester.
<br />
<b>Below are notes of learning:</b>
</p>
<ul><li>Use ";" (semicolon) to terminate commands. Comments in scripts can be specified using the "--" prefix.
</li><li>推荐用八进制的ASCII码来转义 /0xx in SELECT or CREATE
</li><li>Load data while creating tables
<ul><li>EXTERNAL table: 使用关键字EXTERNAL 可以让用户创建一个外部表. 在建表的同时指定一个指向实际数据的路径 LOCATION '&lt;hdfs_location&gt;';
</li><li>table: Using create-table-as-select (CTAS) statement.
</li></ul>
</li><li>对列的改变(Alter)只会修改Hive的元数据，而不会改变实际数据。用户应该确定保证元数据定义和实际数据结构的一致性。但数据follow Implicit conversion after that.
</li><li>join 时，每次 map/reduce 任务的逻辑是这样的：reducer 会缓存 join 序列中除了最后一个表的所有表的记录，再通过最后一个表将结果序列化到文件系统。这一实现有助于在 reduce 端减少内存的使用量。实践中，应该把最大的那个表写在最后,否则会因为缓存浪费大量内存.
</li><li>Join 发生在 WHERE 子句之前。如果你想限制 join 的输出，应该在 WHERE 子句中写过滤条件——或是在 join 子句中写。
</li></ul>
<dl><dd><code>SELECT a.val, b.val FROM a LEFT OUTER JOIN b ON (a.key=b.key) WHERE a.ds='2009-07-07' AND b.ds='2009-07-07'</code>
</dd><dd>会 join a 表到 b 表列出 a.val 和 b.val 的记录(WHERE 从句中可以使用其他列作为过滤条件). 但是如前所述,如果b表中找不到对应a表的记录,b表的所有列都会列出 NULL,包括 ds 列.也就是说join ... where 会过滤b表中不能找到匹配a表join key的所有记录. 这样的话LEFT OUTER 就使得查询结果与WHERE子句无关了(不会出现b.key=null的行).解决的办法是在 OUTER JOIN 时使用以下语法：
</dd><dd><code>SELECT a.val, b.val FROM a LEFT OUTER JOIN b ON (a.key=b.key AND b.ds='2009-07-07' AND a.ds='2009-07-07')</code>
</dd><dd>这一查询的结果是预先在 join 阶段过滤过的,所以不会存在上述问题(就会出现b.key=null的行).这一逻辑也可以应用于 RIGHT 和 FULL 类型的 join 中。
</dd></dl>
<ul><li>Multiple aggregations can be done at the same time, however, no two aggregations can have different DISTINCT columns
</li><li>Hive supports SORT BY which sorts the data per reducer. The difference between "order by" and "sort by" is that the former guarantees total order in the output while the latter only guarantees ordering of the rows within a reducer. If there are more than one reducer, "sort by" may give partially ordered final results.
</li><li>It may be confusing as to the difference between SORT BY alone of a single column and CLUSTER BY. The difference is that CLUSTER BY partitions by the field and SORT BY if there are multiple reducers partitions randomly in order to distribute data (and load) uniformly across the reducers.
</li><li>Hive调用Map/Reduce脚本时通过stdin/stdout进行数据的读写,调试信息输出到stderr
</li><li>MapJoin 会在Map端完成Join操作而不用reducer. 这需要将Join操作的一个或多个表完全读入内存.MapJoin的用法是在查询/子查询的SELECT关键字后面添加/*+ MAPJOIN(tablelist) */提示优化器转化为MapJoin(目前Hive的优化器不能自动优化MapJoin).其中tablelist可以是一个表，或以逗号连接的表的列表. tablelist中的表将会读入内存，应该将小表写在这里.
</li><li>Hive does not directly support UPDATE, DELETE, MERGE, HAVING
</li></ul>
<dl><dd>However <code> "DELETE * FROM tb WHERE id=k" </code> can implement using <code> "INSERT OVERWRITE TABLE tb SELECT id, value FROM tb WHERE id&lt;&gt;k"</code>
</dd></dl>
<ul><li>Fech top k rows uses
</li></ul>
<dl><dd><code>SET mapred.reduce.tasks = 1; SELECT * FROM sales SORT BY amount DESC LIMIT k</code>
</dd></dl>
<ul><li>In every MapReduce stage of the join, the table to be streamed can be specified via a hint. e.g. in
</li></ul>
<dl><dd><code>SELECT /*+ STREAMTABLE(a) */ a.val, b.val, c.val FROM a JOIN b ON (a.KEY = b.key1) JOIN c ON (c.KEY = b.key1)</code>
</dd><dd>all the three tables are joined in a single MapReduce job and the values for a particular value of the key for tables b and c are buffered in the memory in the reducers. Then for each row retrieved from a, the join is computed with the buffered rows.
</dd></dl>
<ul><li>Example of using MAPJOIN来解决实际的问题。应用共同点如下：
<ol><li>有一个极小的表&lt;1000行
</li><li>需要做不等值join操作（a.x &lt; b.y 或者 a.x like b.y等）
</li></ol>
</li></ul>
<dl><dd>这种操作如果直接使用join的话语法不支持不等于操作，hive语法解析会直接抛出错误.如果把不等于写到where里会造成笛卡尔积，数据异常增大，速度会很慢。甚至会任务无法跑成功~ 据mapjoin的计算原理，MAPJION会把小表全部读入内存中，在map阶段直接拿另外一个表的数据和内存中表数据做匹配。这种情况下即使笛卡尔积也不会对任务运行速度造成太大的效率影响。而且hive的where条件本身就是在map阶段进行的操作，所以在where里写入不等值比对的话，也不会造成额外负担。如此看来，使用MAPJOIN开发的程序仅仅使用map一个过程就可以完成不等值join操作，效率还会有很大的提升。问题解决~~示例代码如下：
</dd><dd><code>select /*+ MAPJOIN(a) */ a.start_level, b.* from dim_level a join (select * from test) b where b.xx&gt;=a.start_level and b.xx&lt;end_level;</code>
</dd></dl>
<ul><li>在JOIN操作中,后面被连续JOIN且同一字段,只会执行一个mapreduce操作
</li></ul>
<dl><dd><code>SELECT * FROM a LEFT OUTER JOIN b ON a.t=b.t LEFT OUTER JOIN c ON a.t=c.t; </code>推荐的
</dd><dd><code>SELECT * FROM a LEFT OUTER JOIN b ON a.t=b.t LEFT OUTER JOIN c ON b.t=c.t; </code>效率低下的
</dd></dl>
<ul><li><code>ALTER TABLE a SET SERDEPROPERTIES('serialization.null.format'=''); </code>可以使结果表不出现\N字符串,而用空串代替
</li><li>原来hive不支持顶层union只能将union封装在子查询中,以下错误.
</li></ul>
<dl><dd><code>select count(*) from user_active_vv_20110801_31 where active_type_3&gt;0</code>  
</dd><dd><code>UNION ALL</code>  
</dd><dd><code>select count(*) from user_active_vv_20110801_31  where active_type_7&gt;0</code>
</dd><dd>且必须为union的查询输出定义别名，正确的hql如下：
</dd><dd><code>select * from (</code>
</dd><dd><code>   select count(*) as type3 from user_active_vv_20110801_31 where user_active_vv_20110801_31.active_type_3&gt;0</code>  
</dd><dd><code>   UNION ALL</code>  
</dd><dd><code>   select count(*) as type3 from user_active_vv_20110801_31  where user_active_vv_20110801_31.active_type_7&gt;0</code>
</dd><dd><code>  )</code>
</dd><dd>不过查询出来的结果和hql语句中union的顺序不一致.可以加union order 字段进行orderby调整,'7' as union_order.
</dd></dl>
<h2> <span class="mw-headline" id="Hive_Configuration_Variables">Hive Configuration Variables</span></h2>
<p>Proper setting of variables can improve the hive coding experience at <a rel="nofollow" target="_blank" class="external text" href="https://cwiki.apache.org/confluence/display/Hive/AdminManual+Configuration">Hive Configuration Variables</a>
Below is additional notes:
</p>
<ul><li>hive.optimize.cp
</li></ul>
<dl><dd> Default is true (this is default for column pruning - only fetch columns in the select column list)
</dd></dl>
<ul><li>hive.optimize.pruner
</li></ul>
<dl><dd>Default is true (this is default for partition pruning - partition condition is considered)
</dd><dd> eg. SELECT * FROM T1 JOIN (SELECT * FROM T2) subq ON (T1.c1=subq.c2) WHERE subq.prtn = 100; --会在子查询中就考虑 subq.prtn = 100 条件，从而减少读入的分区数目。
</dd></dl>
<ul><li>hive.groupby.skewindata
</li></ul>
<dl><dd>"set hive.groupby.skewindata=true" is general way to reslove data skew probolem 这是通用的算法优化
</dd></dl>
<ul><li>Hadoop MapReduce程序中,reducer个数的设定极大影响执行效率,这使得Hive怎样决定reducer个数成为一个关键问题.遗憾的是Hive的估计机制很弱,不指定reducer个数的情况下,Hive会猜测确定一个reducer个数,基于以下两个参数设定：
<dl><dd>(1) hive.exec.reducers.bytes.per.reducer（默认为1000^3）
</dd><dd>(2) hive.exec.reducers.max（默认为999）
</dd></dl>
</li></ul>
<dl><dd>计算reducer数的公式很简单：N= MIN(参数2,总输入数据量/参数1)
</dd><dd>通常情况下，有必要手动指定reducer个数。考虑到map阶段的输出数据量通常会比输入有大幅减少，因此即使不设定reducer个数，重设参数2还是必要的。依据Hadoop的经验，可以将参数2设定为0.95*(集群中TaskTracker个数).
</dd></dl>
<ul><li>hive.exec.compress.output
</li></ul>
<dl><dd>默认是 false 但是很多时候貌似要单独显式设置一遍否则会对结果做压缩的,如果你的这个文件后面还要在 hadoop下直接操作那么就不能压缩了. 如果将该参数设置为true的时候,文件类型一般会选择SequenceFile.因为SequenceFile的压缩可以按block的级别,压缩后也可以启用多个map来执行任务. 而当TextFile的时候则仅仅能按file来压缩,这样无论多大的文件也仅仅能采用一个map,效率差得不是一点半点.
</dd></dl>
<ul><li>hive.cli.errors.ignore
</li></ul>
<dl><dd>表示在cli的情况是否忽略错误，默认值是false，表示不忽略。有的时候通过hive -f xx.sql执行的时候，希望即使前面的sql有问题，后面的也要继续执行，那么当遇到错误的时候就需要忽略掉。此时只需要修改hive.cli.errors.ignore为true即可。
</dd></dl>
<ul><li>hive.merge.mapfiles
</li></ul>
<dl><dd>其中hive.merge.mapfiles控制是否对map的结果进行合并，默认值true，启动合并。
</dd></dl>
<ul><li>hive.merge.mapredfiles 
</li></ul>
<dl><dd>而参数hive.merge.mapredfiles控制是否对reduce的结果进行合并，默认值false，不启动合并。是否启动小文件合并要根据实际情况来看，如果开启合并后，会启动一个新的mapreduce过程进行合并，这样对于该任务来说就会增加执行时间。
</dd></dl>
<ul><li>hive.merge.smallfiles.avgsize
</li></ul>
<dl><dd>对小文件进行合并，减少map进程，并且降低对namenode的压力。参数hive.merge.smallfiles.avgsize就是告诉hadoop到底什么样的文件算是小文件，只有先对小小文件进行识别，接下来才能启动新的mapreduce进程进行小文件合并。该参数的默认值为16000000，大概16M，也就是小于16M的文件被认为是小文件，在启动了小文件合并的时候会对这些文件进行合并。
</dd></dl>
<ul><li>hive.merge.size.per.task
</li></ul>
<dl><dd>参数控制每个任务合并小文件后的文件大小，根据这个参数的设置来决定启动多少个reduce来进行小文件合并。默认值为256000000，大概256M，说明合并后的文件大小不能超过256M。我们目前的数据处理方式是将其他库抽取的数据文件先放在hdfs上，然后会对这些文件进行压缩并转化为sequence文件。在数据抽取的过程中，为了加快速度一般都会采用大量的并行，所以这些原始的数据文件一般说来都不是太大，所以在对这些原始数据进行压缩后会产生大量的小文件，在对这些小文件进行操作的时候默认情况下会启动大量的map进程，占用大量资源，并且过多的小文件对namenode也是个负担。所以需要对这些小文件进行合并。
</dd></dl>
<h2> <span class="mw-headline" id="Hive_Extensions">Hive Extensions</span></h2>
<p>File format, UDF, Script Transform, and SerDe are avaliable to extend Hive's functions. Usually, we can use <big><b><a href="/mediawiki/index.php/Hive_Scripting" title="Hive Scripting">Hive Scripting</a></b></big>
such as, shell script, python, perl, java code to implement them.<br />
</p>
<h2> <span class="mw-headline" id="Performance_Tuning">Performance Tuning</span></h2>
<h3> <span class="mw-headline" id="General_Practice">General Practice</span></h3>
<ul><li>好的模型设计事半功倍
</li><li>减少job数
</li><li>设置合理的map reduce的task数(hive.exec.reducers.max)能有效提升性能(eg.10w+级别的计算，用160个reduce,那是相当的浪费,1个足够)
</li><li>了解数据分布，自己动手解决数据倾斜问题是个不错的选择。通用算法(set hive.groupby.skewindata=true)优化有时不能适应特定业务背景，开发人员了解业务，了解数据，可以通过业务逻辑精确有效的解决数据倾斜问题
</li><li>数据量较大的情况下，慎用count(distinct),count(distinct)容易产生倾斜问题
</li><li>对小文件进行合并(see,hive.merge.mapfiles, mapredfiles, size.per.task)是行至有效的提高调度效率的方法，对所有的作业设置合理的文件数 
</li><li>优化时把握整体,单个作业最优不如整体最优
</li><li>尽可能使用partition过滤和mapjoin
</li><li>合并MapReduce操作 using "multi-group by" and/or "multi-distinct" (this is to be checked in doubt <a rel="nofollow" target="_blank" class="external text" href="https://issues.apache.org/jira/browse/HIVE-474">HIVE-474</a> and <a rel="nofollow" target="_blank" class="external text" href="https://issues.apache.org/jira/browse/HIVE-2416">HIVE-2416</a>)
</li></ul>
<h3> <span class="mw-headline" id="Optimizer">Optimizer</span></h3>
<ul><li>Rule based optimizer (Semantic query optimize)
<ul><li>Partition Pruning (hive.optimize.pruner=true [default])
</li><li>Predicate Push down (ppd)
</li><li>Column Pruning (hive.optimize.cp = true [default])
</li><li>Union Transformer
</li></ul>
</li><li>Physical optimizer
<ul><li>Map join Transformer
</li><li>Skew join
</li></ul>
</li></ul>
<h3> <span class="mw-headline" id="Better_Execution_Plan">Better Execution Plan</span></h3>
<ul><li>refer to [<a rel="nofollow" target="_blank" class="external text" href="http://www.tbdata.org/archives/2109">数据倾斜总结</a>] 
</li><li>Join数据倾斜
<dl><dd>Map Join
</dd><dd>限制:•内存•语义
</dd><dd>代价
</dd><dd>用法: <code> select /*+ MAPJOIN(tb_alias)*/ </code>
</dd><dd>Bucketed map join
</dd><dd>Sort
</dd></dl>
</li><li>GroupBy/distinct数据倾斜
<dl><dd>skewindata优化
</dd><dd>用法: <code> set hive.groupby.skewindata=true </code>
</dd></dl>
</li></ul>
<h3> <span class="mw-headline" id="Execution_Tuning">Execution Tuning</span></h3>
<ul><li>内存优化
<ul><li>驱动表
<dl><dd>使用大表做驱动表，避免内存溢出
</dd><dd>默认Join中最右边的表是驱动表
</dd><dd>MapJoin无视Join顺序，使用大表做驱动表
</dd><dd>STREAMTABLE hint
</dd></dl>
</li></ul>
</li><li>I/O优化
<ul><li>Map aggregation
</li><li>相关参数
<dl><dd>hive.map.aggr
</dd><dd>hive.groupby.mapaggr.checkinterval(100000)
</dd><dd>hive.map.aggr.hash.percentmemory(0.5)
</dd><dd>hive.map.aggr.hash.min.reduction(0.5)
</dd></dl>
</li><li>注意控制内存
</li><li>合并小文件
<dl><dd>减少后续任务的map数
</dd><dd>代价：额外的MR过程
</dd></dl>
</li><li>参数：
<dl><dd>hive.merge.mapfiles
</dd><dd>hive.merge.mapredfiles
</dd><dd>hive.merge.size.per.task
</dd><dd>hive.merge.size.smallfiles.avgsize
</dd></dl>
</li></ul>
</li><li>MR任务合并
<ul><li>multi-insert
<dl><dd><code>FROM from_statement
</dd><dd>INSERT OVERWRITE TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)] select_statement1
</dd><dd>[INSERT OVERWRITE TABLE tablename2 [PARTITION ...] select_statement2] ...</code>
</dd><dd>select statement中的过滤条件不能做分区裁剪
</dd></dl>
</li><li>union
<dl><dd><code>select col_listfrom tblwhere …
</dd><dd>union all
</dd><dd>select col_listfrom tblwhere …
</dd><dd>union all
</dd><dd>select col_listfrom tblwhere …</code>
</dd></dl>
</li><li>Joins
<dl><dd>相同Join key的Join可被优化为1次MR过程
</dd></dl>
</li><li>Map Joins
<dl><dd>多次查表操作可以用一个Map Join做完
</dd><dd>不需要是相同Join key
</dd></dl>
</li></ul>
</li></ul>
<h2> <span class="mw-headline" id="Reference">Reference</span></h2>
<ol><li><a rel="nofollow" target="_blank" class="external text" href="http://www.infoq.com/cn/articles/hadoop-file-format">File Format - Hadoop</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="http://samuschen.iteye.com/blog/834402">File Format - Hive</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="https://cwiki.apache.org/confluence/display/Hive/SerDe">Hive SerDe at official Wiki</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="http://dajuezhao.iteye.com/blog/795190">Hive SerDe 1</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="http://www.oratea.net/?p=656">Hive SerDe 2</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="http://wenku.baidu.com/view/4a3593492e3f5727a5e9620c.html">ASCII Code Reference</a>
</li><li><a rel="nofollow" target="_blank" class="external text" href="http://www.tbdata.org/archives/2109">数据倾斜总结</a>
</li></ol>

<!-- 
NewPP limit report
Preprocessor node count: 67/1000000
Post-expand include size: 0/2097152 bytes
Template argument size: 0/2097152 bytes
Expensive parser function count: 0/100
-->

<!-- Saved in parser cache with key my_wiki:pcache:idhash:53-0!*!0!!en!2!* and timestamp 20130621092436 -->
 

 
 
 
 

 
 

 



