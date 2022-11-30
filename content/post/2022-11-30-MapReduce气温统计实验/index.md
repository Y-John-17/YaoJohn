---
title: MapReduce气温统计实验
author: Yao John
date: '2022-11-30'
slug: MapReduce气温统计实验
categories:
  - Hadoop
tags:
  - Big Data
  - Distributed System Environment
  - Hadoop
  - Mapreduce
---

# MapReduce气温统计实验

## 一、数据集准备

数据集下载地址：ftp://ftp.ncdc.noaa.gov/pub/data/uscrn/products/daily01 

将下载下来的数据集合并，更名为  temperature.txt

## 二、数据集导入

**将预处理好的数据导入Linux中，并启动Hadoop集群守护进程**

$  ll  temperature.txt    查看文件是否导入成功

**在hdfs创建文件路径和上传数据集**

$  hdfs  dfs  -mkdir  temperature

$  hdfs  dfs  -mkdir  temperature/input

$  hdfs  dfs  -mkdir  temperature/output

$  hdfs  dfs  -put   temperature.txt  temperature/input

**在浏览器中核验文件是否上传成功**（注意关闭防火墙）

192.168.153.147:50070
192.168.153.147:8088

## 三、数据处理脚本编译

**编写  java  文件**

$  vim  MyMaxMin.java

**写入脚本代码**

```java
import java.io.IOException;
import java.util.Iterator;

import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;

import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.conf.Configuration;

public class MyMaxMin {


    //Mapper

    /**
     *MaxTemperatureMapper class is static and extends Mapper abstract class
     having four hadoop generics type LongWritable, Text, Text, Text.
     */


    public static class MaxTemperatureMapper extends
            Mapper<LongWritable, Text, Text, Text> {

        /**
         * @method map
         * This method takes the input as text data type.
         * Now leaving the first five tokens,it takes 6th token is taken as temp_max and
         * 7th token is taken as temp_min. Now temp_max > 35 and temp_min < 10 are passed to the reducer.
         */

        @Override
        public void map(LongWritable arg0, Text Value, Context context)
                throws IOException, InterruptedException {

            //Converting the record (single line) to String and storing it in a String variable line

            String line = Value.toString();

            //Checking if the line is not empty

            if (!(line.length() == 0)) {

                //date

                String date = line.substring(6, 14);

                //maximum temperature

                float temp_Max = Float
                        .parseFloat(line.substring(39, 45).trim());

                //minimum temperature

                float temp_Min = Float
                        .parseFloat(line.substring(47, 53).trim());

                //if maximum temperature is greater than 35 , its a hot day

                if (temp_Max > 35.0) {
                    // Hot day
                    context.write(new Text("Hot Day " + date),
                            new Text(String.valueOf(temp_Max)));
                }

                //if minimum temperature is less than 10 , its a cold day

                if (temp_Min < 10) {
                    // Cold day
                    context.write(new Text("Cold Day " + date),
                            new Text(String.valueOf(temp_Min)));
                }
            }
        }

    }

//Reducer

    /**
     *MaxTemperatureReducer class is static and extends Reducer abstract class
     having four hadoop generics type Text, Text, Text, Text.
     */

    public static class MaxTemperatureReducer extends
            Reducer<Text, Text, Text, Text> {

        /**
         * @method reduce
         * This method takes the input as key and list of values pair from mapper, it does aggregation
         * based on keys and produces the final context.
         */

        public void reduce(Text Key, Iterator<Text> Values, Context context)
                throws IOException, InterruptedException {


            //putting all the values in temperature variable of type String

            String temperature = Values.next().toString();
            context.write(Key, new Text(temperature));
        }

    }



    /**
     * @method main
     * This method is used for setting all the configuration properties.
     * It acts as a driver for map reduce code.
     */

    public static void main(String[] args) throws Exception {

        //reads the default configuration of cluster from the configuration xml files
        Configuration conf = new Configuration();

        //Initializing the job with the default configuration of the cluster
        Job job = new Job(conf, "weather example");

        //Assigning the driver class name
        job.setJarByClass(MyMaxMin.class);

        //Key type coming out of mapper
        job.setMapOutputKeyClass(Text.class);

        //value type coming out of mapper
        job.setMapOutputValueClass(Text.class);

        //Defining the mapper class name
        job.setMapperClass(MaxTemperatureMapper.class);

        //Defining the reducer class name
        job.setReducerClass(MaxTemperatureReducer.class);

        //Defining input Format class which is responsible to parse the dataset into a key value pair
        job.setInputFormatClass(TextInputFormat.class);

        //Defining output Format class which is responsible to parse the dataset into a key value pair
        job.setOutputFormatClass(TextOutputFormat.class);

        //setting the second argument as a path in a path variable
        Path OutputPath = new Path(args[1]);

        //Configuring the input path from the filesystem into the job
        FileInputFormat.addInputPath(job, new Path(args[0]));

        //Configuring the output path from the filesystem into the job
        FileOutputFormat.setOutputPath(job, new Path(args[1]));

        //deleting the context path automatically from hdfs so that we don't have delete it explicitly
        OutputPath.getFileSystem(conf).delete(OutputPath);

        //exiting the job only if the flag value becomes false
        System.exit(job.waitForCompletion(true) ? 0 : 1);

    }
}
```

保存退出

**定位到  Hadoop  CLASSPATH路径**

 #该操作需要在Hadoop环境搭建初期在环境变量中写入HADOOP_CLASSPATH路径

$  export  HADOOP_CLASSPATH=$(hadoop classpath) 

**创建类文件夹**、

$  mkdir  temperature_classes

**编译  java class  类文件**

$  javac  -classpath  ${HADOOP_CLASSPTH}  -d  temperature_classes  MyMaxMin.java

**将 java class 类文件打包成  jar 文件**

$  jar  -cvf  temperature.jar  -C  temperature_classes

## 四、处理数据集

$  hadoop  jar  temperature.jar  MyMaxMin  temperature/input   temperature/output

## 五、下载查看处理结果

$  hdfs  dfs   -get  temperature/output

$  ls

$  cd  output

$  cat  part-r-00000