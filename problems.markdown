---
layout: "post"
title: "Problems"
date: "2017-05-07 18:29"
---


# Problems

###PROBLEM 1


_Description_


Error TimeLine       
1. The Spark Shell starts
2. Error – Slave Lost – Happened with diferent slaves, with Scala and Pyspark. Yarn container logs dont show any memory issues.

* According to Container Logs Memory Usage was okay
* Spark Logging at Level INFO did not tell anything
*  Very hard to actually understand how logging really happened in Cluster Enviroment, which used Yarn and Spark together.  Logs are usually very large
* Don’t know whether there are some problems related to settings, virtual machines, yarn. Search space large.

_Solution_

Turns out the error comes from java 8 using too much VM which does not fit into the bounds of Spark boundaries. These settings made all the errors
disappear.

>spark.yarn.executor.memoryOverhead 700    
spark.yarn.driver.memoryOverhead   1000   
spark.yarn.am.memoryOverhead       700    

* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d  
* http://stackoverflow.com/questions/27792839/spark-fail-when-running-pi-py-example-with-yarn-client-mode
* http://stackoverflow.com/questions/28967500/why-spark-application-fail-with-executor-coarsegrainedexecutorbackend-driver-d
* http://stackoverflow.com/questions/33141129/why-spark-executor-receives-sigterm


###PROBLEM 2

_Description_

Cant perform tunneling to student1-x2 from cocserver. Received a random SSL error.

_Solution_

Tunnelled
cocserver -> student1 -> student1-x2


###PROBLEMS WITH IPYTHON PYSPARK

* Had problems with installation. Initially the web inteface did not show for ipython. Had to specify ip=* in command line
* Spark Context did not start. Had to specify environment variables.
