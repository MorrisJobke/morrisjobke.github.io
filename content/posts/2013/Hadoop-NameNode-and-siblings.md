---
categories:
- hadoop
date: "2013-12-11T19:20:00Z"
title: Hadoop - NameNode, Checkpoint Node and Backup Node
---

### NameNode

The NameNode stores the metadata of the HDFS. The state of HDFS is stored in a
file called `fsimage` and is the base of the metadata. During the runtime
modifications are just written to a log file called `edits`. On the next
start-up of the NameNode the state is read from `fsimage`, the changes from
`edits` are applied to that and the new state is written back to `fsimage`.
After this `edits` is cleared and contains is now ready for new log entries.

### Checkpoint Node

A Checkpoint Node was introduced to solve the drawbacks of the NameNode. The
changes are just written to `edits` and not merged to `fsimage` during the
runtime. If the NameNode runs for a while `edits` gets huge and the next
startup will take even longer because more changes have to be applied to the
state to determine the last state of the metadata.

The Checkpoint Node fetches periodically `fsimage` and `edits` from the
NameNode and merges them. The resulting state is called checkpoint. After this
is uploads the result to the NameNode.

There was also a similiar type of node called "Secondary Node" but it doesn't
have the "upload to NameNode" feature. So the NameNode need to fetch the state
from the Secondary NameNode. It also was confussing because the name suggests
that the Secondary NameNode takes the request if the NameNode fails which
isn't the case.

### Backup Node

The Backup Node provides the same functionality as the Checkpoint Node, but is
synchronized with the NameNode. It doesn't need to fetch the changes
periodically because it receives a strem of file system edits. from the
NameNode. It holds the current state in-memory and just need to save this to
an image file to create a new checkpoint.

#### Sources

 * [NameNode, Secondary NameNode, CheckpointNode, Backup Node @ Hadoop Wiki](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html#Secondary_NameNode)
 * [NameNode vs Secondary NameNode @ stackoverflow](http://stackoverflow.com/a/19975012)
 * [Checkpoint Node, Backup Node @ stackoverflow](http://stackoverflow.com/a/10424902)
 * [Hadoop Bugtracker - Comment about introduction of Checkpoint and Backup Node](https://issues.apache.org/jira/browse/HADOOP-4539?focusedCommentId=12674954&page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel#comment-12674954)


### Updates:

**Comment by Moritz Becker (March 2017):**

 > "There was also a similiar type of node called “Secondary Node” but it doesn’t
have the “upload to NameNode” feature. So the NameNode need to fetch the state
from the Secondary NameNode."
>
> This appears to be wrong. The Secondary NameNode (SNN) does in fact (indirectly) upload the checkpoint to the NameNode (NN). Indirectly means that, once checkpointing is completed, the SNN issues a GET request to the NN to trigger a fetch of the checkpoint by the NN. However, a CheckpointNode (CN) does exactly the same. So there is no difference between the operations of SNN and CN. However, SNN was deprecated in favor of CN.
>
> * https://issues.apache.org/jira/browse/HADOOP-4539
> * https://github.com/apache/hadoop-hdfs/blob/HDFS-265/src/java/org/apache/hadoop/hdfs/server/namenode/SecondaryNameNode.java
> * https://github.com/apache/hadoop-hdfs/blob/trunk/src/java/org/apache/hadoop/hdfs/server/namenode/Checkpointer.java

**Comment by myself (March 2017):** 

> Thanks for the reply!
>
> This post is over 3 years old - I would rate it quite outdated anyway ;) Nevertheless: Good to have your comment here for clarification. 👍
