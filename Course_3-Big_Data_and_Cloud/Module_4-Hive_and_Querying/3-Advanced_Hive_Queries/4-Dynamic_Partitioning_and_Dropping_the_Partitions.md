# Dynamic Partitioning and Dropping the Partitions

In the previous segment, you learnt how to create static partitioning. In this segment, you will be introduced to dynamic partitioning.

So, let’s watch the next video for a demonstration of dynamic partitioning where data gets allocated automatically in the partitions.

**VIDEO**

Now, let’s summarise all that you saw in the video above:

- By default, dynamic partitioning is not allowed in Hive. You need to set dynamic partitioning to true:

![23](https://i.ibb.co/gPtDgmq/23.png)

- In the next snapshot, you can see how a dynamic partitioned table named “dyn_part_user_info” has been created and how data has been loaded into it using the “insert” clause:

![24](https://i.ibb.co/r4Fvm5w/24.png)

- Once you have loaded the data into the dynamic partitioned table named “dyn_part_user_info”, the values under the profession column are automatically allocated into multiple partitions, which as you can see within  “**user/hive/warehouse/dyn_part_user_info**” location in Hadoop.

In the next video, you will learn where to use static and dynamic partitioning.

**VIDEO**

So, you can see that it takes less time if you place the partitions manually, as in the case of static partitioning, as Hive gets an explicit command to allocate the values under the profession column into partitions. On the other hand, in the case of dynamic partitioning, Hive allocates the profession automatically and, hence, it takes some time to process the data into partitions.

In the next video, you will learn how to drop partitions.

**VIDEO**

- So, you have already created static partitions on ‘doctor’ and ‘engineer’ in the previous segment. Now, let us first see what are the different partitions of the “part_user_info” table in the HDFS.

![25](https://i.ibb.co/V2X7w6F/25.png)

So, there are two partitions in the “part_user_info” table, namely, ‘doctor’ and ‘engineer’.

- Let us write a command to drop the profession ‘doctor’. Here, you have to explicitly define the partition name that you want to drop, e.g., ‘doctor’.  
  ![26](https://i.ibb.co/rv40GC3/26.png)  
  Now, there remains only one partition, that is, ‘engineer’.

So, you have learned how to create the two types of partitions and how to drop them. Now, let us compare the query time when you are using a non-partitioned table with the query time when you are using a partitioned one.

**VIDEO**

Here, Vishwa has taken an example whose problem statement is as follows:

“What are the total ratings given by the male and female users for the ‘engineer’ profession”? You have two tables, of which one is non-partitioned, whereas the another one is a partitioned table:

- **user_info**: This is a **non-partitioned** table. If you want to find out the total rating of both the genders separately for the 'engineering' profession, then you can run the following query:
  
  `select gender, sum(ratings) from user_info where profession= 'engineer' group by gender;`
  
  The time for the query works out to be 7.157 seconds.

- **dyn_part_user_info**: This table is partitioned dynamically on the ‘profession’ column.  If you want to find out the total rating of both the genders separately for the 'engineering' profession, then you can run the following query:
  
  `select gender, sum(ratings) from dyn_part_user_info where profession= 'engineer' group by gender;`
  
  The time for the query works out to be 6.911 seconds.

Thus, you can see that the query running time for the partitioned table was found to be lesser than that of the non-partitioned table.

```shell
hive> use demo;
OK
Time taken: 0.994 seconds

hive> create table if not exists dyn_part_user_info (id int, age int, gender string, ratings int) partitioned by (profession string) row format  delimited fields terminated by '|' lines terminated by '\n';
OK
Time taken: 0.107 seconds
hive> drop table dync_part_user_info;
OK
Time taken: 0.278 seconds

hive> set hive.exec.dynamic.partition=true;
hive> set hive.exec.dynamic.partition.mode=nonstrict;
hive> insert into table dyn_part_user_info partition(profession) select id, age, gender, reviews, profession from user_info;
Query ID = hadoop_20220327213943_c2f8706b-75a1-4c04-8223-4f3d2e4d9cd6
Total jobs = 1
Launching Job 1 out of 1
Tez session was closed. Reopening...
Session re-established.
Status: Running (Executing on YARN cluster with App id application_1648410869414_0014)

----------------------------------------------------------------------------------------------
        VERTICES      MODE        STATUS  TOTAL  COMPLETED  RUNNING  PENDING  FAILED  KILLED
----------------------------------------------------------------------------------------------
Map 1 .......... container     SUCCEEDED      1          1        0        0       0       0
Reducer 2 ...... container     SUCCEEDED      2          2        0        0       0       0
----------------------------------------------------------------------------------------------
VERTICES: 02/02  [==========================>>] 100%  ELAPSED TIME: 7.37 s
----------------------------------------------------------------------------------------------
Loading data to table demo.dyn_part_user_info partition (profession=null)

Loaded : 21/21 partitions.
     Time taken to load dynamic partitions: 2.296 seconds
     Time taken for adding to write entity : 0.007 seconds
OK
Time taken: 23.788 seconds
hive>
    > ;
hive> exit;
[hadoop@ip-172-31-24-155 ~]$ hadoop fs -ls /user/hive/warehouse/demo.db/dyn_part_user_info
Found 21 items
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=administrator
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=artist
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=doctor
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=educator
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=engineer
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=entertainment
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=executive
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=healthcare
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=homemaker
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=lawyer
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=librarian
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=marketing
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=none
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=other
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=programmer
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=retired
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=salesman
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=scientist
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=student
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=technician
drwxrwxrwt   - hadoop hdfsadmingroup          0 2022-03-27 21:40 /user/hive/warehouse/demo.db/dyn_part_user_info/profession=writer

[hadoop@ip-172-31-24-155 ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j2.properties Async: false
hive> use demo;
OK
Time taken: 1.309 seconds
hive> show partitions part_user_info;
OK
profession=doctor
profession=engiener
Time taken: 0.508 seconds, Fetched: 2 row(s)
hive> alter table part_user_info drop partition (profession='doctor');
Dropped the partition profession=doctor
OK
Time taken: 1.446 seconds
hive> show partitions part_user_info;
OK
profession=engiener
Time taken: 0.194 seconds, Fetched: 1 row(s)
hive> select gender, sum(ratings) from user_info where profession= ??[hadoop@ip-172-31-24-155 ~]$ ;
[hadoop@ip-172-31-24-155 ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j2.properties Async: false
hive> use demo;
OK
Time taken: 1.431 seconds
hive> select gender, sum(ratings) from user_info where profession = 'engineer' group by gender ;
FAILED: SemanticException [Error 10004]: Line 1:19 Invalid table alias or column reference 'ratings': (possible column names are: id, age, gender, profession, reviews)
hive> select gender, sum(reviews) from user_info where profession = 'engineer' group by gender ;
Query ID = hadoop_20220327215302_290cb39d-8d8f-43ba-8176-cb79c2935535
Total jobs = 1
Launching Job 1 out of 1
Status: Running (Executing on YARN cluster with App id application_1648410869414_0016)

----------------------------------------------------------------------------------------------
        VERTICES      MODE        STATUS  TOTAL  COMPLETED  RUNNING  PENDING  FAILED  KILLED
----------------------------------------------------------------------------------------------
Map 1 .......... container     SUCCEEDED      1          1        0        0       0       0
Reducer 2 ...... container     SUCCEEDED      2          2        0        0       0       0
----------------------------------------------------------------------------------------------
VERTICES: 02/02  [==========================>>] 100%  ELAPSED TIME: 6.93 s
----------------------------------------------------------------------------------------------
OK
F   81982
M   3077991
Time taken: 11.092 seconds, Fetched: 2 row(s)
hive> select gender, sum(ratings) from dyn_part_user_info where profession = 'engineer' group by gender ;
Query ID = hadoop_20220327215340_b2f7ae1e-335f-4232-8147-a7fd7c66c033
Total jobs = 1
Launching Job 1 out of 1
Status: Running (Executing on YARN cluster with App id application_1648410869414_0016)

----------------------------------------------------------------------------------------------
        VERTICES      MODE        STATUS  TOTAL  COMPLETED  RUNNING  PENDING  FAILED  KILLED
----------------------------------------------------------------------------------------------
Map 1 .......... container     SUCCEEDED      1          1        0        0       0       0
Reducer 2 ...... container     SUCCEEDED      2          2        0        0       0       0
----------------------------------------------------------------------------------------------
VERTICES: 02/02  [==========================>>] 100%  ELAPSED TIME: 5.18 s
----------------------------------------------------------------------------------------------
OK
F   81982
M   3077991
Time taken: 6.637 seconds, Fetched: 2 row(s)
```

In the next segment, you will learn about bucketing.

#### Partitioning

Qn: Suppose there is a table that contains a column “year”. This column contains the years from 2001 to 2010. Now suppose you create the partitions on a year column then how many partitions will be created when you use the dynamic partitioning.

- A total of 10 partitions will be created in which data will be allocated automatically.

- If a new year comes into the list, say, 2011, then in dynamic partitioning, a new partition will be created automatically.

- You need to activate dynamic partitioning using “set hive.exec.dynamic.partition = true ;”

- If a new year comes into the list, say, 2011, then unless you enter the data manually in the 2011 partition, no data will be allocated automatically.

Ans: A, B & C.

- When you perform dynamic partitioning, data will be allocated automatically into the partitions, and in this case, 10 partitions will be created automatically.

- *When you perform dynamic partitioning, data will be allocated automatically into the partitions, and in this case, the 11th partition will be created automatically.*

- By default, dynamic partitioning is inactive in Hive; you need to activate dynamic partitioning using  `set hive.exe.dynamic.partition = true;`.
