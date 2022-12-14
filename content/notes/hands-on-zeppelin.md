---
title: "Hands on Zeppelin"
tags:
- data
- zeppelin
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



## Create New Note

![yshJJIkNnq](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/yshJJIkNnq.jpg)

Create a new note through these two entries by giving it a name and select a default interpreter.

<img src="https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214150836370.png" alt="image-20221214150836370" style="zoom:50%;" />

> You can use multiple interpreter in one zeppeline note to support different languages. You can get this via ChatGPT. See details in "Q&A (with ChatGPT on Zeppelin)" part.

![image-20221214151356720](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214151356720.png)

I write 3 different code snippet in 3 different languages of Markdown, Python and sql. Run them and we will get:

![image-20221214151758144](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214151758144.png)

You can see: 

* %md and %python work fine.
* %sql gives an error: Interpreter sql not found.

## Interpreters

We can see and manage Zeppelin's interpreters via the 'interpreter' menu below: 

<img src="https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214151959692.png" alt="image-20221214151959692" style="zoom:50%;" />



All interpreters are here:

![image-20221214154255076](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214154255076.png)

## Add a SQL interpreter

In order to access data in mysql, add a SQL interpreter:

Reference: 

* [SQL support in Zeppelin](https://zeppelin.apache.org/docs/0.8.0/quickstart/sql_with_zeppelin.html)
* [Generic JDBC Interpreter for Apache Zeppelin - mysql](https://zeppelin.apache.org/docs/0.8.0/interpreter/jdbc.html#mysql)

> If you want to connect other databases such as `Mysql`, `Redshift` and `Hive`, you need to edit the property values. You can also use [Credential](https://zeppelin.apache.org/docs/latest/setup/security/datasource_authorization.html) for JDBC authentication. If `default.user` and `default.password` properties are deleted(using X button) for database connection in the interpreter setting page, the JDBC interpreter will get the account information from [Credential](https://zeppelin.apache.org/docs/latest/setup/security/datasource_authorization.html).

* [Data Source Authorization in Apache Zeppelin](https://zeppelin.apache.org/docs/0.7.0/security/datasource_authorization.html)
* [Tutorial: Using Apache Zeppelin with MySQL](https://thedataist.com/tutorial-using-apache-zeppelin-with-mysql/)



First, add a credential info, which stores safely the username and password of the mysql connection:

![image-20221214164153294](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214164153294.png)



Second, add a new interpreter `adbsql` as 

![image-20221214164336089](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214164336089.png)

Remember to add dependencies to locate the jar used to run the mysql driver:

![image-20221214164508044](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214164508044.png)



## Access Data using SQL

Then I can access the data in mysql using sql like this:

![image-20221214171612554](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214171612554.png)

## Q&A (with ChatGPT on Zeppelin)

### Can I use python and sql in one zeppelin note?

ChatGPT:

> Yes, it is possible to use Python and SQL together in a Zeppelin notebook. In fact, many data analysis tasks can benefit from combining the capabilities of both languages.
>
> 
>
> To use Python and SQL together in a Zeppelin notebook, you will first need to make sure that you have the appropriate interpreters installed and configured. In Zeppelin, an interpreter is a plug-in that enables the notebook to execute code in a specific programming language. For Python and SQL, you will need to install and configure the Python and SQL interpreters, respectively.
>
> 
>
> Once the interpreters are installed and configured, you can use the `%python` and `%sql` magic commands in your Zeppelin notebook to switch between Python and SQL code blocks. For example, you could use Python to load and clean your data, and then use SQL to analyze and visualize the cleaned data.
>
> Here is an example of how you might use Python and SQL together in a Zeppelin notebook:

```python
%python
# load and clean data using Python

import pandas as pd

df = pd.read_csv("data.csv")
df = df.dropna()

%sql
-- analyze and visualize data using SQL

SELECT * FROM df LIMIT 10

```

> I hope this helps! Let me know if you have any other questions.



### SQL: how to add a sql interpreter to apache zeppelin

<img src="https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214155408956.png" alt="image-20221214155408956" style="zoom:50%;" />

### Java: hello world in apache zeppelin

[Java interpreter for Apache Zeppelin](https://zeppelin.apache.org/docs/0.9.0/interpreter/java.html)

ChatGPT:

![image-20221214152806322](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221214152806322.png)
