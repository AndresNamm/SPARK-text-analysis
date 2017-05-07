---
layout: "post"
title: "InstallationReadme"
date: "2017-05-07 16:12"
---

# Installation


## YARN-HADOOP

In installation, followed the Default Course tutorial.

**Project specific configuration**  

Installed in all the 8 vm-s. The cluster is configured currently to use Yarn and Hadoop only on these computers to acoid student1 small memory bottlenecks.

* student13-x1 Hadoop master  
* student85-x1 slave  
* student85-x2 slave  
* student13-x2 slave  
* student14-x1 slave  
* student14-x2 slave  

**Most Important Settings**

_yarn-site.xml_


>....  
<property>  
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
...

_yarn.exclude_

>student1-x2   
>student1-x1  

_hdfs-site.xml_


>...
<configuration>  
	<property>  
		<name>dfs.replication</name>  
		<value>3</value>  
	</property>  
	<property>  
		<name>dfs.blocksize</name>  
		<value>64m</value>  
	</property>  
	<property>  
		<name>dfs.client.block.write.replace-datanode-on-failure.policy</name>  
		<value>ALWAYS</value>  
	</property>  
	<property>  
	  <name>dfs.hosts.exclude</name>  
	  <value>/opt/hadoop-2.6.0/etc/hadoop/dfs.exclude</value>  
	  <final>true</final>  
	</property>   
</configuration>
...

_hdfs.exclude_

>student1-x2   
>student1-x1  


## SPARK

In installation, followed the Default Course tutorial.

**Project specific configuration**  

_spark.defaults_ - spark settings when you don't specify anything else in the commandline or in the program-

>spark.executor.instances         4  
spark.yarn.archive		 hdfs://student13-x1:9000/addedlibs/spark-archive.zip   
spark.executor.memory		 4g  
spark.driver.memory		 4g  
spark.yarn.executor.memoryOverhead 700  
spark.yarn.driver.memoryOverhead   1000  
spark.yarn.am.memoryOverhead       700   


# XEN

All cluster node settings

>vcpus       = '1'
memory      = '6096'  
maxmem      = '8098'

Student1 vm Settings

>vcpus       = '1'
memory      = '1024'  


### PySpark on Student13-x1


* Step1: Installed Jupyter and Ipython notebook and configured it to use together with Spark. Main [tutorial1](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jupyter-notebook-to-run-ipython-on-ubuntu-16-04)  I followed to install this module .
* Step2: Configured ipython to be accessible outside localhost and added tunneling through cocserver port 18813.   
* Step3  Configured Spark and Ipython so that sparkContext could be executed inside a Ipython kernel. Basic idea how to do this I got from this [tutorial2](http://coolinux.com/blog/2015/07/24/install-pyspark-and-integrate-ipython/), however had to do some specifications to be adapted to this enviroment.
  * Added this to end of _.profile_ of hduser in student13-x1 so SparkContext could be used by hduser
    >export PYSPARK_DRIVER_PYTHON=ipython  
    export PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip=\*' pyspark

  * Added this to the end of _.bashrcs_ in student13-x1 so pyspark command would be executable everywhere.  
    >export SPARK_HOME="/opt/spark-2.1.0-bin-hadoop2.6"


# Usage

## Starting Pyspark

To start ipython together with spark just execute at hduser@student13-x1
>pyspark --master yarn [other options ]  

At you home directory or wherever you find this most reasonable. This starts a Ipython notebook in address http://202.45.128.135:18813/?token=dsmaofnfds (token is different every time) _The sparContext_ has not yet started.

To initialize sparkContext you have to initialize an ipython kernel. After you have initialized the kernel, sparkContext initialization has started, but this takes some time. You can follow this process under Yarn applications interface.


## Executing Our Project Source

1. Start pyspark like specified under the previous header.
2. Execute the code snippets commented as Task1,2,4,3. In the window you started the pyspark you can follow the job and application output that spark writes. You can also follow it under yarn application interface.

## Monitoring the environment

Fom the ListOflinks web-page you can systematically access


*Cluster References*

* Yarn and Hadoop Monitoring interfaces
* Spark Monitoring interfaces
* Project web-page
* Ganglia


*External project parts*

* Github repository for project related info
  * [CONF and TUTORIALS](https://github.com/AndresNamm/SPARK-text-analysis)
  * [SOURCE CODE](https://github.com/AndresNamm/NgramAnalysis )
  * [Project Specific Tutorial with some Concept explanations](https://github.com/AndresNamm/SPARK-text-analysis/blob/master/SparkYarnTutorial.markdown)

## Using management scripts

in hduser@student13-x1 home execute

* startHadoop - restart the hadoop enviroment, yarn enviroment and history server
* shareSettings - shares Yarn settings accrosse the cluster.



# Files submitted in this report.  

YARN HADOOP SETTINGS
* yarn-site.xml
* dfs-site.xml
* yarn.exclude
* dfs.exclude

SPARK SETTINGS

* spark-defaults.conf
* spark-env .sh      

SOURCE CODE FOR DATA ANALYSIS

* max_count.ipynb

WEB PAGE SOURCE CODE

/ngram folder

XEN

* student13-x1.cfg- shares settings with student13-x2.cfg, student14-x1.cfg, student14-x2.cfg, student85-x1.cfg, student85-x2.cfg
* student1-x1.cfg- shares settings with student1-x2.cfg

MANAGEMENT INTERFACE

* ListOflinks.html
* YarnSparkTutorial.h

SCRIPTS  

* startHadoop
* shareSettings
