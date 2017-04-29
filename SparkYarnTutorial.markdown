---
layout: "post"
title: "GP_Andres"
date: "2017-04-19 12:59"
---

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Text Analysis Project](#)
	- [Intro](#)
	- [Setting up Spark in Yarn](#)
		- [Recap of Definitions in Hadoop](#)
		- [Definitions for Hadoop and Yarn](#)
		- [Starting spark](#)
		- [Cluster division studentX - dom0, studentX-xX - vm](#)
		- [Important settings](#)
		- [Problems](#)
		- [Logging](#)
		- [Managing the CLUSTER](#)
	- [Ipython Notebook](#)
		- [Installation](#)
		- [Starting Pyspark](#)
	- [Spark Tutorials Python](#)
	- [Spark on Yarn Architecture](#)

# Text Analysis Project


**All info related to this project is available at Github**: [Project setup](https://github.com/AndresNamm/SPARK-text-analysis.git) ; [Project Source](https://github.com/AndresNamm/NgramAnalysis.git)

## Intro


We Implemented google [N-GRAM](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html) 
Analysis. This is a short summary of the problem we solved and issues that occured and how we solved them. 
I have also included some references to good tutorials and I hope some of my insights can help people who are solving different problems than this. 
The analysis we did is rather simple, but it is a good example, because it involves a large dataset, which takes a lot of time to analyse. This allows 
us to take advantage of a lot of features of spark withou focusing too much on the complexities of underlying algorithms.


Dataset: google 1gram;science-fiction books;5.2 GB of data  
Enviroment: Cluster with 5 nodes <- 1 master and 4 slaves. 



Use hduser whent executing commadn

## Setting up Spark in Yarn

### Recap of Definitions in Hadoop

* RESOURCE MANAGER (YARN) - manages all resources :O
* NODE MANAGER (HADOOP) - manages file system
* APPLICATION MASTER - For every application, there exists 1 master. concept in yarn. Allows to run multiple applications at the same time
* CONTAINER - resizabel. useful for spark.

### Definitions for Hadoop and Yarn

* CLIENT VS CLUSTER modified
 * CLIENT MODE is when the driver is running inside the client process. (The one, who initiated application)
 * CLUSTER MODE is when the driver is running inside Application master in yarn
* APPLICATION
 * JOB - like in yarn , runs inside application
* DRIVER PROGRAM - Schdules tasks on Executors, Manages job flow
* CLUSTER MANAGER - Starts executor processes
* PARTITION - like in HDFS
* SPARK-SUBMIT vs SPARK-SHELL/PYSPART
  * SUBMIT - For scripts
  * SHELL - for shell :O
* TRANSFORMATION, ACTION
  * TRANSFORMATION
    * map
  * ACTION
  *
* RDD
  * PARTITIONS


### Starting spark 

For debugging I guess its always easier to start Spark Context with speficic settings on the command line like in this example:

>  pyspark --master yarn-cluster [--driver-memory 4g]   -- for python with settings


### Cluster division studentX - dom0, studentX-xX - vm

* student1 – Ganglia    
* student1-x1 project webpage    
* student1-x2 project administration page    
* student13-x1 Hadoop master  
* student85-x1 slave  
* student85-x2 slave  
* student13-x2 slave  
* student14-x1 slave  
* student14-x2 slave  


### Important settings

Because spark uses the principle, that all executors have to share same settings, I configured yarn to be the same across all nodes and decommissioned nodes, 
that would have become bottlenecks with decommissioning procedure. This also allows easier cluster management.

**spark.defaults** - spark setting when you dont specify anything else8 in the commandline or in the program

>spark.executor.instances         4  
spark.yarn.archive		 hdfs://student13-x1:9000/addedlibs/spark-archive.zip   
spark.executor.memory		 4g  
spark.driver.memory		 4g  
spark.yarn.executor.memoryOverhead 700  
spark.yarn.driver.memoryOverhead   1000  
spark.yarn.am.memoryOverhead       700   


**yarn-site.xml** 

><property>  
        <name>yarn.nodemanager.resource.memory-mb</name>  
        <value>5020</value>  
</property>  
<property>  
        <name>yarn.scheduler.minimum-allocation-mb</name>  
        <value>1024</value>  
</property>  
<property>  
        <name>yarn.scheduler.maximum-allocation-mb</name>  
        <value>5020</value>  
</property>  
<property>  
  <name>yarn.resourcemanager.nodes.exclude-path</name>  
  <value>/opt/hadoop-2.6.0/etc/hadoop/yarn.exclude</value>  
  <final>true</final>  
</property>  




### Problems

Error TimeLine       
1. The Spark Shell starts
2. Error – Slave Lost – Happened with diferent slaves, with Scala and Pyspark. Yarn container logs dont show any memory issues. 

Turns out the error comes from java 8 using too much VM which does not fit into the bounds of Spark boundaries. These settings made all the errors
disappear.

>spark.yarn.executor.memoryOverhead 700    
spark.yarn.driver.memoryOverhead   1000   
spark.yarn.am.memoryOverhead       700    

* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d  
* http://stackoverflow.com/questions/27792839/spark-fail-when-running-pi-py-example-with-yarn-client-mode
* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d
* http://stackoverflow.com/questions/33141129/why-spark-executor-receives-sigterm


### Logging

Overally it seems like Hadoop logs are more specific regarding resources like physical and virtual memory whereas spark logs the events
and their execution sequences, e.g. it is more abstract.

You can find container logs under  the nodes , which ran the container
For example in this project container logs for student13-x1. The ip naturally 
http://[IP]/logs/userlogs/application_1490663309971_0002/container_1490663309971_0002_01_000001/


### Managing the CLUSTER

* SCRIPTS - To ease the management of services I wrote 3 scripts: startHadoop: Restarts the hadoop environment; shareSettings: shares hadoop settings across specified nodes;
shareSpark: shares spark settings across the cluster.  
* GITHUB - allows easily to get access to important settings to analize them. Can store the development history. Good intermediate between the cluster and other computers   
* [GANGLIA](http://ganglia.sourceforge.net/) - Provides simple interface to have an overall view of resource usages.
* ListOflinks - It's good to keep a simple website which references all the web interfaces you have in the project. Otherwise it's easy to loose track 


## Ipython Notebook

### Installation

+ I had to install pip and ipython jupyter according to this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jupyter-notebook-to-run-ipython-on-ubuntu-16-04) and then perform tunneling, which was specific to my envrionment.
Notebook is accessible, when its running in port 18813
+ Now I have to try to configure spark and both iphyhton so spark context could be commanded in Ipython Environment
I did the installation exactly like in this [tutorial](http://coolinux.com/blog/2015/07/24/install-pyspark-and-integrate-ipython/
) except for the
> PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip=* ' pyspark'

 I also had to specify pyspark in .profile file in my home directory to be a path variable

### Starting Pyspark

+ Now, to start ipython together with spark just execute at hduser@student13-x1
>pyspark --master yarn [other options ]          

 at you home directory or wherever you find this most reasonable. This starts a Ipython notebook in address http://202.45.128.135:18813/?token=dsmaofnfds
(token is different every time naturally)


## Spark Tutorials Python

* [I think this is the most comprehensive tutorial, but its long.](https://docs.databricks.com/spark/latest/training/index.html)
* [Spark job Visualisation](https://databricks.com/blog/2015/06/22/understanding-your-spark-application-through-visualization.html)
* http://spark.apache.org/docs/latest/running-on-yarn.html
* https://www.supergloo.com/spark-tutorial/spark-tutorials-python/
* https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python
* https://www.dezyre.com/apache-spark-tutorial/pyspark-tutorial
* https://www.datacamp.com/community/tutorials/apache-spark-python#gs.9_=gwlg
* http://spark.apache.org/docs/latest/quick-start.html
* https://mapr.com/ebooks/spark/05-processing-tabular-data-with-spark-sql.html

## Spark on Yarn Architecture


* http://badrit.com/blog/2015/2/29/running-spark-on-yarn#.WNSeW6IlGUl
* http://blog.cloudera.com/blog/2014/05/apache-spark-resource-management-and-yarn-app-models/  
* https://jaceklaskowski.gitbooks.io/mastering-apache-spark
* https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cdh_ig_running_spark_on_yarn.html
* http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-1/
* http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-2/



