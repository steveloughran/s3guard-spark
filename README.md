# s3guard-spark
Integrate the Hadoop s3guard committer with Apache Spark

This module integrates the [S3guard committer](https://issues.apache.org/jira/browse/HADOOP-13786) with Apache Spark.

It does this by defining a new Spark committer, (which must go under the `org.apache.spark` package to get access to `private[spark]` types).
This committer
1. Delegates the core commit operation to the Hadoop S3Guard committer.
1. Does not attempt to do this for files directly created with create file requests. Why not? The mechanism used for pending commits places data into the object store; scattering it round the file system complicates commitment and recovery. Instead those are copied over in the task commit (whose time becomes O(data)). That part of the operation may delay the entire job.
