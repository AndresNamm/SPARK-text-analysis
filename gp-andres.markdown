---
layout: "post"
title: "GP_Andres"
date: "2017-04-19 12:59"
---

# Progress of Text Analysis Project

##  Description of conspect

Because I had a lot of trouble with the topics mentioned below, I decided to put together somewhat a conspect of references to make your life a bit more easy.
* setting up spark, understanding how spark with yarn works
* understanding the weird errors I sometimes got
* understanding logging both from yarn and spark
* Getting into understanding Spark Development, tutorial sources and so on

Use hduser whent executing commadn

## Setting up Spark in Yarn

### Settings

#### Distributed or Local ?

I'm not entirely sure, but like in hadoop I think the settings should be shared to each machine, so I wrote  to hduser folder a script shareSpark, which shares all the spark conf/ files to all the other machines. However, I guess its always easier to start Spark Context with speficic settings on the command line like in this example:

>  pyspark --master yarn-cluster --driver-memory 4g \

Please note I have also written a script called shareSettings, which send settings to computers currently included in the cluster. 

### PROBLEMS

As mentioned earlier I had tons of problems running spark. To solve these problems, I had to modify 2 files
spark-env.sh and spark-defaults.conf. I'm actually not sure, which of the settings files has a bigger impact. These settings are also shared to every computer in the cluster with script sharesSpark.

In spark-env.sh I modified


>HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop  
LD_LIBRARY_PATH=/opt/hadoop-2.6.0/lib/native:$LD_LIBRARY_PATH  
SPARK_MASTER_IP=student13-x1  
SPARK_EXECUTOR_INSTANCES=3  
SPARK_EXECUTOR_MEMORY=4G  
SPARK_DRIVER_MEMORY=2G  

In spark-defaults I modified

>spark.master                     spark://student13-x1:7077  
spark.eventLog.enabled           true  
spark.eventLog.dir               hdfs://student13-x1:9000/tmp/sparkLog  
spark.history.fs.logDirectory    hdfs://student13-x1:9000/tmp/sparkLog  
spark.serializer                 org.apache.spark.serializer.KryoSerializer  
spark.executor.instances         3  
spark.yarn.archive               hdfs://student13-x1:9000/addedlibs/spark-archive.zip  
spark.executor.memory            1g  
spark.driver.memory              4g   
spark.yarn.executor.memoryOverhead 700    
spark.yarn.driver.memoryOverhead   1000  
spark.yarn.am.memoryOverhead       700  


In yarn-site.xml I modified


><property>  
        <name>yarn.nodemanager.resource.memory-mb</name>  
        <value>4092</value>  
</property>  
<property>  
        <name>yarn.scheduler.minimum-allocation-mb</name>  
        <value>1024</value>  
</property>  
<property>  
        <name>yarn.scheduler.maximum-allocation-mb</name>  
        <value>3072</value>  
</property>  
<property>  
  <name>yarn.resourcemanager.nodes.exclude-path</name>  
  <value>/opt/hadoop-2.6.0/etc/hadoop/yarn.exclude</value>  
  <final>true</final>  
</property>  




For problem solving I used these links.

* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d  
* http://stackoverflow.com/questions/27792839/spark-fail-when-running-pi-py-example-with-yarn-client-mode
* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d
* http://stackoverflow.com/questions/33141129/why-spark-executor-receives-sigterm


### Logging

You can find container logs under  the nodes , which ran the container
Fore example container logs for student13-x1  
http://202.45.128.135:15913/logs/userlogs/application_1490663309971_0002/container_1490663309971_0002_01_000001/


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


## FOLDERS of THE LOADED DATA in hdfs

i loaded 1-grams from fiction [link](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html)

in hdfs they are list as
>/ngrams/test # 1 file  
/ngrams/complete # all file

## Spark Tutorials Python

* I think this is the most comprehensive tutorial, but its long. https://docs.databricks.com/spark/latest/training/index.html
* http://spark.apache.org/docs/latest/running-on-yarn.html
* https://www.supergloo.com/spark-tutorial/spark-tutorials-python/
* https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python
* https://www.dezyre.com/apache-spark-tutorial/pyspark-tutorial
* https://www.datacamp.com/community/tutorials/apache-spark-python#gs.9_=gwlg
* http://spark.apache.org/docs/latest/quick-start.html
* https://mapr.com/ebooks/spark/05-processing-tabular-data-with-spark-sql.html
