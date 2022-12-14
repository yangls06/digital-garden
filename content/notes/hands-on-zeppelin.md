---
title: "Hands on Zeppelin"
tags:
- data, zeppelin
weight: -5

---



# Hands on Zeppelin: Step by Step

> In order to build a data playground enabling the engineers to explore data collected by autonomous cars, I am trying to deploy Apache Zeppelin as a component of data platform.

## Introduction

[Apache Zeppelin](https://zeppelin.apache.org/) is: "Web-based notebook that enables data-driven,
interactive data analytics and collaborative documents with SQL, Scala, Python, R and more."



## Install

According to [installation document](https://zeppelin.apache.org/docs/0.10.0/quickstart/install.html), I choose to install Zeppelin using the offical docker on a server (http://10.10.32.4):

```sh
$ mkdir Zeppelin & cd Zeppelin
$ docker run -u $(id -u) -p 8080:8080 --rm -v $PWD/logs:/logs -v $PWD/notebook:/notebook \
           -e ZEPPELIN_LOG_DIR='/logs' -e ZEPPELIN_NOTEBOOK_DIR='/notebook' \
           --name zeppelin apache/zeppelin:0.10.0

Unable to find image 'apache/zeppelin:0.10.0' locally
0.10.0: Pulling from apache/zeppelin
16ec32c2132b: Pull complete
aafd5bdc2bb7: Pull complete
0bb58b150809: Pull complete
68d71ea3a296: Pull complete
9c7277321f0c: Downloading [=============================================>     ]   2.59GB/2.816GB
6be3e4488900: Download complete
622d30c2f649: Download complete
d10a38bf471f: Download complete
4006c4346d45: Download complete
4f4fb700ef54: Download complete

...

ERROR [2022-12-13 08:13:15,873] ({main} ZeppelinServer.java[main]:262) - Error while running jettyServer
java.lang.Exception: A MultiException has 2 exceptions.  They are:
1. java.io.IOException: Creating directories for /notebook/.git failed
2. java.lang.IllegalStateException: Unable to perform operation: create on org.apache.zeppelin.notebook.repo.NotebookRepoSync

	at org.apache.zeppelin.server.ZeppelinServer.main(ZeppelinServer.java:256)

```



These two error was raised because of [Volume mapping issue with Zeppelin](https://forums.docker.com/t/volume-mapping-issue-with-zeppelin/121917), you can fix it to add

`-u 0` parameter to set the user to 0 (root) in docker run.

```sh
$ docker run -u $(id -u) -p 8080:8080 -u 0 --rm -v $PWD/logs:/logs -v $PWD/notebook:/notebook \
           -e ZEPPELIN_LOG_DIR='/logs' -e ZEPPELIN_NOTEBOOK_DIR='/notebook' \
           --name zeppelin apache/zeppelin:0.10.0
```



or you can change the owner of dir `logs` and `notebook` using:

```sh
$ sudo chown -R 1000:1000 notebook
$ sudo chown -R 1000:1000 logs
$ docker run -u $(id -u) -p 8080:8080 --rm -v $PWD/logs:/logs -v $PWD/notebook:/notebook \
           -e ZEPPELIN_LOG_DIR='/logs' -e ZEPPELIN_NOTEBOOK_DIR='/notebook' \
           --name zeppelin apache/zeppelin:0.10.0
```



Then visit http://10.10.32.4:8080/ to use Zeppelin.

![image-20221213165658796](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221213165658796.png)



