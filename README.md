# Word Count Streaming using MapReduce + Python

- **`books-input`**: Contains multiple `.txt` files, each representing content of a book (Retrieved from https://www.gutenberg.org/).
- **mapper.py**: The Mapper function.
- **reducer.py**: The Reducer function.

## Prerequisites

- Create and start an AWS EMR Cluster.

## Instructions

1. Connect to the Hadoop master node using SSH.
    ```
    ssh -i ~/vockey.pem hadoop@ec*-*-***-***-***.compute-*.amazonaws.com
    ```

2. Copy the files from the local machine to Hadoop master node.
    ```
    scp -i aws/labsuser.pem mapper.py hadoop@ec*-*-***-***-***.compute-*.amazonaws.com:/home/hadoop
    ```

3. Create the input directory `books-input` in HDFS.
    ```
    hdfs dfs -mkdir books-input
    ```

4. Put the input text files to the created directory.
    ```
    hdfs dfs -put books-input/*.txt books-input
    ```

5. Locate the required `hadoop-streaming.jar` jar file.
    ```
    find /usr/lib/ -name *hadoop*streaming*jar
    ```
    
    A sample output:
    ```
    /usr/lib/hadoop/hadoop-streaming-3.2.1-amzn-5.jar
    /usr/lib/hadoop/hadoop-streaming.jar
    /usr/lib/hadoop-mapreduce/hadoop-streaming-3.2.1-amzn-5.jar
    /usr/lib/hadoop-mapreduce/hadoop-streaming.jar
    ```

6. Perform Hadoop Streaming. Output directory will be `books-output` when using the below command.
    ```
    hadoop jar /usr/lib/hadoop/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input books-input -output books-output
    ```

    Output files will be written to the `books-output` directory. It will contain files similar to the following:
    ```
    $ hdfs dfs -ls books-output
    Found 4 items
    -rw-r--r--   1 hadoop hdfsadmingroup          0 2022-01-29 12:28 books-output/_SUCCESS
    -rw-r--r--   1 hadoop hdfsadmingroup     128570 2022-01-29 12:28 books-output/part-00000
    -rw-r--r--   1 hadoop hdfsadmingroup     129049 2022-01-29 12:28 books-output/part-00001
    -rw-r--r--   1 hadoop hdfsadmingroup     125961 2022-01-29 12:28 books-output/part-00002
    ```

7. Get the files from `books-output` directory.
    ```
    hdfs dfs -get books-output/*
    ```

8. Examine the content of an output file.
    ```
    cat part-00000
    ```

    A part of sample output:
    ```
    Amaze 1
    Ambitious 1
    Amen. 1
    America. 7
    Americans, 2
    Americans; 2
    Ames 1
    Ampthill, 1
    Ampthill.'' 1
    Anderson, 1
    Andrews, 4
    Anglican 1
    Ann 8
    Annette 3
    Anointed 1
    ```

