---
title: CMU 95702 - Project 5
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-11-15 09:49:45
---
CMU 95702 - Distributed System

Project 5

<!-- more -->

***

# Commands

## HDFS
// 查看文件夹
`hadoop dfs -ls /user/student107/input/`

// 从本地文件夹复制到hdfs上
// 复制的时候回创建文件夹和文件
`hadoop dfs -copyFromLocal /home/student107/input/1902.txt /user/student107/input/1902.txt`

// 删除文件夹
`$hadoop dfs -rmr /user/student107/output`

// Merge and copy files from the Hadoop Distributed File system to the client.
// hdfs 到本地
`hadoop dfs -getmerge /user/student107/output aCoolLocalFile`
`$hadoop dfs -getmerge /user/student107/output ~/coolProjectOutput/`

// You may view what jobs are running on the cluster with the command:
`hadoop job -list`

// Kill a job that is not making progress:
`hadoop job -kill job_201310251241_0754`

// 跑mapreduce jar包+input+未创建的output文件夹
$ hadoop jar /home/student107/temperature.jar edu.cmu.andrew.mm6.MaxTemperature
/user/student107/input/combinedYears.txt /user/student107/output

## Local
// 删除本地文件夹
`rm -r wenjianjia`
`rm -rf dirname`

// Remove an existing jar file.
`$rm temperature.jar`

// 新建文件夹
`mkdir wenjianjia`

// 查看，编译
``` bash
ls
temperature_classes MaxTemperatureMapper.java MaxTemperatureReducer.java
MaxTemperature.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperatureMapper.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperatureReducer.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperature.java

```

// 打jar包
`$jar -cvf temperature.jar -C  temperature_classes/  .`

## FTP
``` bash
sftp student107@heinz-jumbo.heinz.cmu.local
put MaxTemperature.java
get MaxTemperature.java
```


# Tasks
## Task0
``` bash
// 登陆
// Login ID : student107
// password : JNiaming_01
ssh -l student107 heinz-jumbo.heinz.cmu.local

// Linux 复制文件
mkdir wordcount
cp /home/public/WordCount.java /home/student107/wordcount

// 建个文件夹，放编译以后的类
mkdir wordcount_classes
// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./wordcount_classes -d wordcount_classes WordCount.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./test -d test *.java

// 打jar包
cd /home/student107/wordcount
jar -cvf WordCount.jar -C  wordcount_classes/  .

jar -cvf OaklandCrimeStats.jar -C  test/  .


// 用来wordcount的文件在/home/public/words.txt
// 把它拷贝到hdfs上
hadoop dfs -copyFromLocal /home/public/words.txt /user/student107/wordcountinput/words.txt

// 瞅一眼有没有复制成功
hadoop dfs -ls /user/student107/wordcountinput/
hadoop dfs -cat /user/student107/wordcountinput/words.txt
ls

// MapReduce 
// jar + jar包+带package的类名+输入+输出位置
// 注意这个类名，要带package！！
hadoop jar /home/student107/wordcount/WordCount.jar org.myorg.WordCount /user/student107/wordcountinput/words.txt /user/student107/wordcountoutput

// 瞅一眼输出
hadoop dfs -ls /user/student107/wordcountoutput/

// merge到本地
hadoop dfs -getmerge /user/student107/wordcountoutput /home/student107/Project5/Task0/Task0Output

// 瞅一眼输出
cat /home/student107/Project5/Task0/Task0Output

// 完美

```

## Task1

``` bash
// 要修改wordcount这个文件
// 先ftp下载下来

// 连接服务器
// password : JNiaming_01
sftp student107@heinz-jumbo.heinz.cmu.local
// 下载文件
get WordCount.java

// 要计算总的单词数量
// 把java文件改成TotalWords.java

// 上传文件
put TotalWords.java

// 在task1目录下
mkdir totallwords_classes
// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./totallwords_classes -d totallwords_classes TotalWords.java
// 打jar包
jar -cvf TotalWords.jar -C  totallwords_classes/  .

// MapReduce 
// jar + jar包+带package的类名+输入+输出位置
// 输出在task1output 文件夹中
hadoop jar /home/student107/Project5/Task1/TotalWords.jar org.myorg.TotalWords /user/student107/wordcountinput/words.txt /user/student107/task1output

// fail 了，先删除输出文件夹
hadoop dfs -rmr /user/student107/task1output
// 重来

// 瞅一眼输出
hadoop dfs -ls /user/student107/task1output/

// merge到本地
hadoop dfs -getmerge /user/student107/task1output /home/student107/Project5/Task1/Task1Output

// 瞅一眼输出
// totalwords	235923
cat /home/student107/Project5/Task1/Task1Output


// Linux里就有wordcount功能
// 输出依然是 235923 
wc -w /home/public/words.txt

// 完美
```

## Task2
计算有且仅有两个元音的单词的数量

``` bash
// 上传文件
put TotalTwoVowels.java

// 在task2 目录下
mkdir totaltwovowels_classes
// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./totaltwovowels_classes -d totaltwovowels_classes TotalTwoVowels.java
// 打jar包
jar -cvf TotalTwoVowels.jar -C  totaltwovowels_classes/  .


// MapReduce 
// jar + jar包+带package的类名+输入+输出位置
// 输出在task1output 文件夹中
hadoop jar /home/student107/Project5/Task2/TotalTwoVowels.jar org.myorg.TotalTwoVowels /user/student107/wordcountinput/words.txt /user/student107/task2output

// fail 了，先删除输出文件夹
hadoop dfs -rmr /user/student107/task2output
// 删jar 和 totaltwovowels_classes
rm -rf totaltwovowels_classes
rm TotalTwoVowels.jar

// 瞅一眼输出
hadoop dfs -ls /user/student107/task2output/
hadoop dfs -cat /user/student107/task2output/part-r-00000

// merge到本地
hadoop dfs -getmerge /user/student107/task2output /home/student107/Project5/Task2/Task2Output

// 瞅一眼
twoVowels	30792
cat /home/student107/Project5/Task2/Task2Output

// 应该差不多吧 完美
```

## Task3
Task3太傻逼了。输出两个元音的，另外输出不含两个元音的……呵呵

``` bash
// 上传文件
put TotalTwoVowels2.java

// 在Task3 目录下
mkdir totaltwovowels2_classes
// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./totaltwovowels2_classes -d totaltwovowels2_classes TotalTwoVowels2.java
// 打jar包
jar -cvf TotalTwoVowels2.jar -C  totaltwovowels2_classes/  .


// MapReduce 
// jar + jar包+带package的类名+输入+输出位置
// 输出在task1output 文件夹中
hadoop jar /home/student107/Project5/Task3/TotalTwoVowels2.jar org.myorg.TotalTwoVowels2 /user/student107/wordcountinput/words.txt /user/student107/task3output

// 瞅一眼输出
hadoop dfs -ls /user/student107/task3output/
hadoop dfs -cat /user/student107/task3output/part-r-00000

// merge到本地
hadoop dfs -getmerge /user/student107/task3output /home/student107/Project5/Task3/Task3Output

// 瞅一眼
// notTwoVowels	205131
// twoVowels	30792
// 205131 + 30792 = 235923
cat /home/student107/Project5/Task3/Task3Output

// 把java文件拷到Project5的文件夹里
cp /home/student107/task3/TotalTwoVowels.java /home/student107/Project5/Task3

// 完美
```

## Task4
``` bash
// 拷贝文件
cp /home/public/MaxTemperature* /home/student107/Project5/Task4

mkdir temperature_classes

// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperatureMapper.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperatureReducer.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MaxTemperature.java

// MaxTemperature.java 编译不过，要加一个import

jar -cvf maxtemperature.jar -C  temperature_classes/  .

// 复制文件到hdfs上 
// /home/public/combinedYears.txt
hadoop dfs -copyFromLocal /home/public/combinedYears.txt /user/student107/temperatureinput/combinedYears.txt

// 瞅一眼
hadoop dfs -ls /user/student107/temperatureinput/

hadoop jar /home/student107/Project5/Task4/maxtemperature.jar edu.cmu.andrew.mm6.MaxTemperature /user/student107/temperatureinput/combinedYears.txt /user/student107/task4output

hadoop dfs -ls /user/student107/task4output

hadoop dfs -getmerge /user/student107/task4output /home/student107/Project5/Task4/Task4Output

// 输出长这样
// 1901	317
// 1902	244

```

## Task5
Note that the temperatures in this file are degrees Celsius * 10. 
没搞明白这句话有作用吗
输出的时候除以10好了

``` bash 
mkdir temperature_classes

// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MinTemperatureMapper.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MinTemperatureReducer.java

javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./temperature_classes -d temperature_classes MinTemperature.java


jar -cvf mintemperature.jar -C  temperature_classes/  .

hadoop jar /home/student107/Project5/Task5/mintemperature.jar MinTemperature /user/student107/temperatureinput/combinedYears.txt /user/student107/task5output

hadoop dfs -ls /user/student107/task5output

hadoop dfs -getmerge /user/student107/task5output /home/student107/Project5/Task5/Task5Output

// 输出长这样
// 1901	-33.3
// 1902	-32.8
cat /home/student107/Project5/Task5/Task5Output
```


## Task6
又是数数

以wordcount为基础改一下

用到split
String[] strings = line.split("\\t");

``` bash
mkdir assaultsplusrobberies_classes

// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./assaultsplusrobberies_classes -d assaultsplusrobberies_classes Crime.java

jar -cvf assaultsplusrobberies.jar -C  assaultsplusrobberies_classes/  .

// 拷贝文件到hdfs上
hadoop dfs -copyFromLocal /home/public/P1V.txt /user/student107/P1Vinput/P1V.txt

hadoop jar /home/student107/Project5/Task6/assaultsplusrobberies.jar Crime /user/student107/P1Vinput/P1V.txt /user/student107/task6output

hadoop dfs -ls /user/student107/task6output

hadoop dfs -getmerge /user/student107/task6output /home/student107/Project5/Task6/Task6Output

// 输出长这样
// AGGRAVATED ASSAULT or ROBBERY	24879
cat /home/student107/Project5/Task6/Task6Output

// 用 wc 数一下行数 
wc -l /home/public/P1V.txt
// 一共是 27219 那大部分都是这两种类型，也还正常的吧

```

## Task7

``` bash
mkdir oaklandcrimestats_classes

// 编译
javac -classpath  /usr/local/hadoop/hadoop-core-1.2.1.jar:./oaklandcrimestats_classes -d oaklandcrimestats_classes OaklandCrime.java

jar -cvf oaklandcrimestats.jar -C  oaklandcrimestats_classes/  .

    
hadoop jar /home/student107/Project5/Task7/oaklandcrimestats.jar org.myorg.OaklandCrime /user/student107/P1Vinput/P1V.txt /user/student107/task7output

hadoop dfs -ls /user/student107/task7output

hadoop dfs -rmr /user/student107/task7output

hadoop dfs -getmerge /user/student107/task7output /home/student107/Project5/Task7/Task7Output

// 输出长这样
// oaklandcrimestats	275
cat /home/student107/Project5/Task7/Task7Output

```











***

Over！