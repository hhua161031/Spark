Spark shell
============
### 所需文件
#### 1，应用软件版本：<br>
jdk-7u67-linux-x64.tar.gz <br>
scala 2.11.8.tgz <br>
spark-1.2.0-bin-hadoop2.4.tgz <br>
<br>
或其他版本（以tar.gz或者tgz尾缀）

#### 2，ubuntu版本:<br>
Ubuntu 15.10

### 步骤
#### 1，查看本机是否装有jdk，llinux可以自动安装openjdk版本：<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark1.png) 
 
#### 2，jdk安装包所在目录:
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark2.png) 

#### 3，解压tar包:
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark3.png)
<br>
**Tar命令解释**
![github](https://github.com/hhua161031/Spark/blob/master/image/tar命令解释.png) 

#### 4，文件配置，类似window下环境变量配置:
$sudo gedit ~/.bashrc<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark4.png)
<br><br>
export JAVA_HOME=/home/hua/softWare/jdk1.7.0_67<br>
export JRE_HOME=${JAVA_HOME}/jre <br>
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib <br>
export PATH=${JAVA_HOME}/bin:$PATH<br>
立即生效配置文件<br>
source ~/.bashrc
#### 5，查看:
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark5.png)<br>
### 安装scala和spark 
安装方法和jdk一样。<br>
Bashrc配置为<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark6.png)
<br>
查看是否成功安装scala<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark8.png)<br>

启动
<br>
Spark支持python,scala,java三种语言<br>
启动python spark shell
<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark9.png)<br>
启动scala spark shell<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark10.png)<br>
### 简单案例
使用语言：Scala<br>

目的：统计文件中字符出现个数<br>

环境：spark shell
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark11.png)<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark12.png)<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark13.png)<br>
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark14.png)<br>


`val dataSource = sc.textFile`<br>
`val dataSource = sc.textFile("file:///home/hua/doc/GISSERVER.sql")`<br>
`val rdd=dataSource.flatMap(_.split(" "))`<br>
`rdd.map(x=>(x,1)).reduceByKey(_+_).collect().foreach(println)`<br>

本机访问地址127.0.0.1:4040能看到资源管理页面
![github](https://github.com/hhua161031/Spark/blob/master/image/伪分布式Spark15.png)<br>


