---
layout: "post"
title: "SparkStuff"
date: "2017-04-10 18:29"
---


# List of useful Commands for Spark

Run Spark in client mode

>/opt/spark-2.1.0-bin-hadoop2.6/bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode client /opt/spark-2.1.0-bin-hadoop2.6/examples/jars/spark-examples_2.11-2.1.0.jar 3  

Run Spark in cluster mode


>/opt/spark-2.1.0-bin-hadoop2.6/bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode cluster /opt/spark-2.1.0-bin-hadoop2.6/examples/jars/spark-examples_2.11-2.1.0.jar 3   
