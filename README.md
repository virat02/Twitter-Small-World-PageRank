Project Intermediate
CS6240
Spring 2019

Spark Program for Single Source Shortest Path and PageRank

Code author
-----------
- Javesh Monga
- Vivin Wilson
- Virat Goradia

## Proof

As the single source shortest path is based on Breadth First Search algorithm, each new Spark Job indicates that a new level/breadth in the user relations tree is being processed. Thus, according to the six degrees theory we must never get more than 7 Spark jobs in our executions where the 7th job marks the convergence of our execution due to no change in the results from the 6th job. We were able to observe exactly this and thus could prove the six-degree theory.

## Project Report 

[Click here to view project report](https://docs.google.com/document/d/15zDsXcDInnR6-PjJXmhbfgh2_UjVYzycJe6uVRTPz1w/edit?usp=sharing)

Installation
------------
These components are installed:
- JDK 1.8
- Scala 2.12.8
- Hadoop 2.8.5
- Spark 2.4.0 (without bundled Hadoop)
- Maven
- AWS CLI (for EMR execution)

Environment
-----------
1) Example ~/.bash_aliases (My Environment):
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/javeshmonga/workspace/mapreduce/hadoop-2.8.5
export SCALA_HOME=/home/javeshmonga/.sdkman/candidates/scala/2.12.8
export SPARK_HOME=/home/javeshmonga/workspace/mapreduce/spark-2.4.0-bin-without-hadoop
export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$SCALA_HOME/bin:$SPARK_HOME/bin
export SPARK_DIST_CLASSPATH=$(hadoop classpath)

2) Explicitly set JAVA_HOME in $HADOOP_HOME/etc/hadoop/hadoop-env.sh:
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

Execution
---------
All of the build & execution commands are organized in the Makefile.
1) Unzip project file.
2) Open command prompt.
3) Navigate to directory where project files unzipped.
4) Edit the Makefile to customize the environment at the top.
	Sufficient for standalone: hadoop.root, jar.name, local.input
	Other defaults acceptable for running standalone.
5) Standalone Hadoop:
	make switch-standalone		-- set standalone Hadoop environment (execute once)
	make local
6) Pseudo-Distributed Hadoop: (https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html#Pseudo-Distributed_Operation)
	make switch-pseudo			-- set pseudo-clustered Hadoop environment (execute once)
	make pseudo					-- first execution
	make pseudoq				-- later executions since namenode and datanode already running 
7) AWS EMR Hadoop: (you must configure the emr.* config parameters at top of Makefile)
	make upload-input-aws		-- only before first execution
	make aws					-- check for successful execution with web interface (aws.amazon.com)
	make download-output-aws	-- after successful execution & termination
	make download-log-aws       -- after successful execution & termination to get the log files in a local log directory
