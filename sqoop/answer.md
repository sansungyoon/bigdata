#1. From the accounts table, import only the primary key, along with the first name, last name to HDFS directory /loudacre/accounts/user_info. Please save the file in text format with tab delimiters

* 처음 시도한 방법
[training@localhost ~]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --target-dir /loudacre/acoounts/user_info --fields-terminated-by '\t'

*  acoouts.java에서 컬럼이름 확인
[training@localhost ~]$ ls
accounts.java      codegen_basestations.java  Documents  eclipse  Pictures  Templates           Videos
basestations.java  Desktop                    Downloads  Music    Public    training_materials  workspace
[training@localhost ~]$ cat accounts.java

* 만들어진 폴더 확인 후 삭제
[training@localhost ~]$ hdfs dfs -ls /loudacre
Found 5 items
drwxrwxrwx   - training supergroup          0 2019-04-07 22:03 /loudacre/acoounts
-rw-rw-rw-   1 training supergroup      15191 2019-04-07 21:34 /loudacre/base_stations.tsv
drwxrwxrwx   - training supergroup          0 2019-04-07 21:44 /loudacre/basestations_import
drwxrwxrwx   - training supergroup          0 2019-04-07 21:48 /loudacre/basestations_import_parquet
drwxrwxrwx   - training supergroup          0 2019-04-07 21:34 /loudacre/kb
[training@localhost ~]$ hdfs dfs -rm -r /loudacre/acoounts
Deleted /loudacre/acoounts

* tab으로 pk, first name, last name 포함하여 다시 생성
##[training@localhost ~]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --target-dir /loudacre/acoounts/user_info --columns "acct_num, first_name, last_name" --fields-terminated-by '\t'

>19/04/07 22:15:56 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:15:56 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:15:56 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 22:15:56 INFO tool.CodeGenTool: Beginning code generation
19/04/07 22:15:56 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:15:56 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:15:56 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/8b2f7a7d39d00d12fdc38f3b55145f01/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 22:15:59 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/8b2f7a7d39d00d12fdc38f3b55145f01/accounts.jar
19/04/07 22:15:59 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 22:15:59 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 22:15:59 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 22:15:59 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 22:15:59 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 22:15:59 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 22:15:59 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 22:16:00 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 22:16:00 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 22:16:02 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 22:16:02 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/04/07 22:16:02 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/04/07 22:16:02 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 22:16:02 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554698003223_0004
19/04/07 22:16:03 INFO impl.YarnClientImpl: Submitted application application_1554698003223_0004
19/04/07 22:16:03 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554698003223_0004/
19/04/07 22:16:03 INFO mapreduce.Job: Running job: job_1554698003223_0004
19/04/07 22:16:11 INFO mapreduce.Job: Job job_1554698003223_0004 running in uber mode : false
19/04/07 22:16:11 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 22:16:17 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 22:16:22 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 22:16:27 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 22:16:33 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 22:16:33 INFO mapreduce.Job: Job job_1554698003223_0004 completed successfully
19/04/07 22:16:33 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=560464
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=2615920
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=15252
		Total vcore-seconds taken by all map tasks=15252
		Total megabyte-seconds taken by all map tasks=3904512
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=242
		CPU time spent (ms)=3560
		Physical memory (bytes) snapshot=494936064
		Virtual memory (bytes) snapshot=8262238208
		Total committed heap usage (bytes)=251920384
	File Input Format Counters
		Bytes Read=0
	File Output Format Counters
		Bytes Written=2615920
19/04/07 22:16:33 INFO mapreduce.ImportJobBase: Transferred 2.4947 MB in 33.6312 seconds (75.9595 KB/sec)
19/04/07 22:16:33 INFO mapreduce.ImportJobBase: Retrieved 129761 records.
[training@localhost ~]$ hdfs dfs -tail /loudacre/acoounts/user_info/part-m-00003
Janessa	Lewis
129713	Megan	Silva
129714	Debra	Horner
129715	Angela	Powers
129716	Werner	Torres
129717	Lisa	Mitchell
129718	Darlene	Deluna
129719	Julie	Daniel
129720	Gary	Wang
129721	Bertha	Romero
129722	Josephine	Moe
129723	Tanya	Pacheco
129724	David	Davis
129725	Mikki	Oleson
129726	Richard	Almaraz
129727	Eric	Mitchum
129728	Charles	Farley
129729	Carlton	Boring
129730	Chung	Rodgers
129731	David	Rivera
129732	Lucille	Aucoin
129733	Rose	Lynch
129734	Lori	Smith
129735	Mary	Fowlkes
129736	Darryl	Williams
129737	Bradley	Nilson
129738	Sue	Holland
129739	Jane	Ross
129740	Robert	Newman
129741	Scott	Robinson
129742	Franklin	Walker
129743	Marianna	Brown
129744	Shelton	Mack
129745	Rosario	Sauceda
129746	Mary	Tatum
129747	Justin	Kieffer
129748	Michael	Johnson
129749	Sharon	Sommers
129750	Enrique	Thomas
129751	Sylvia	Boykin
129752	Gary	Johnson
129753	Fernando	Tarrant
129754	Maria	Godoy
129755	John	Hale
129756	Danny	Graf
129757	John	McCall
129758	Robert	Pitt
129759	Deborah	Hutchings
129760	Zola	Tedder
129761	Ruth	Ebersole

#2. This time save the same in parquet format with snappy compression. Save it in /loudacre/accounts/user_compressed. Provide.a screenshot of HUE with the new directory created.

##[training@localhost ~]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --target-dir /loudacre/acoounts/user_compressed --as-parquetfile --compression-codec org.apache.hadoop.io.compress.SnappyCodec
19/04/07 22:25:53 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:25:53 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:25:53 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 22:25:53 INFO tool.CodeGenTool: Beginning code generation
19/04/07 22:25:53 INFO tool.CodeGenTool: Will generate java class as codegen_accounts
19/04/07 22:25:54 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:25:54 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:25:54 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/9f39973b876210d0be17827719402261/codegen_accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 22:25:56 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/9f39973b876210d0be17827719402261/codegen_accounts.jar
19/04/07 22:25:56 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 22:25:56 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 22:25:56 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 22:25:56 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 22:25:56 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 22:25:56 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 22:25:57 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 22:25:57 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:25:57 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:25:59 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 22:25:59 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 22:26:01 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 22:26:01 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/04/07 22:26:01 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/04/07 22:26:01 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 22:26:01 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554698003223_0005
19/04/07 22:26:01 INFO impl.YarnClientImpl: Submitted application application_1554698003223_0005
19/04/07 22:26:01 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554698003223_0005/
19/04/07 22:26:01 INFO mapreduce.Job: Running job: job_1554698003223_0005
19/04/07 22:26:10 INFO mapreduce.Job: Job job_1554698003223_0005 running in uber mode : false
19/04/07 22:26:10 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 22:26:20 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 22:26:28 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 22:26:37 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 22:26:46 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 22:26:46 INFO mapreduce.Job: Job job_1554698003223_0005 completed successfully
19/04/07 22:26:46 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=569280
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=65906
		HDFS: Number of bytes written=5904609
		HDFS: Number of read operations=272
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=40
	Job Counters
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=31567
		Total vcore-seconds taken by all map tasks=31567
		Total megabyte-seconds taken by all map tasks=8081152
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=1090
		CPU time spent (ms)=15490
		Physical memory (bytes) snapshot=772907008
		Virtual memory (bytes) snapshot=8296497152
		Total committed heap usage (bytes)=251920384
	File Input Format Counters
		Bytes Read=0
	File Output Format Counters
		Bytes Written=0
19/04/07 22:26:46 INFO mapreduce.ImportJobBase: Transferred 5.6311 MB in 47.6156 seconds (121.0994 KB/sec)
19/04/07 22:26:46 INFO mapreduce.ImportJobBase: Retrieved 129761 records.

#3.Finally save in /loudacre/accounts/CA only clients whose state is from California. Save the filein avro format and compressed using snappy. From the terminal, display some of the records that you just imported. Take a screenshot and save it as CA_only.

[training@localhost ~]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --target-dir /loudacre/acoounts/CA --as-avrodatafile --where "state='CA'" --compression-codec org.apache.hadoop.io.compress.SnappyCodec
19/04/07 22:44:23 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:44:23 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:44:23 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 22:44:23 INFO tool.CodeGenTool: Beginning code generation
19/04/07 22:44:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:44:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:44:24 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/27e14ba15a250a0dcd72a2e249d7e092/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 22:44:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/27e14ba15a250a0dcd72a2e249d7e092/accounts.jar
19/04/07 22:44:26 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 22:44:26 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 22:44:26 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 22:44:26 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 22:44:26 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 22:44:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 22:44:27 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 22:44:28 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:44:28 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:44:28 INFO mapreduce.DataDrivenImportJob: Writing Avro schema file: /tmp/sqoop-training/compile/27e14ba15a250a0dcd72a2e249d7e092/accounts.avsc
19/04/07 22:44:28 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 22:44:28 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 22:44:30 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 22:44:30 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts` WHERE ( state='CA' )
19/04/07 22:44:30 INFO db.IntegerSplitter: Split size: 32439; Num splits: 4 from: 1 to: 129760
19/04/07 22:44:30 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 22:44:30 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554698003223_0006
19/04/07 22:44:30 INFO impl.YarnClientImpl: Submitted application application_1554698003223_0006
19/04/07 22:44:31 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554698003223_0006/
19/04/07 22:44:31 INFO mapreduce.Job: Running job: job_1554698003223_0006
19/04/07 22:44:39 INFO mapreduce.Job: Job job_1554698003223_0006 running in uber mode : false
19/04/07 22:44:39 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 22:44:47 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 22:44:54 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 22:45:01 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 22:45:08 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 22:45:09 INFO mapreduce.Job: Job job_1554698003223_0006 completed successfully
19/04/07 22:45:09 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=566924
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=5669076
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=23793
		Total vcore-seconds taken by all map tasks=23793
		Total megabyte-seconds taken by all map tasks=6091008
	Map-Reduce Framework
		Map input records=92416
		Map output records=92416
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=426
		CPU time spent (ms)=11430
		Physical memory (bytes) snapshot=614539264
		Virtual memory (bytes) snapshot=8283320320
		Total committed heap usage (bytes)=251920384
	File Input Format Counters
		Bytes Read=0
	File Output Format Counters
		Bytes Written=5669076
19/04/07 22:45:09 INFO mapreduce.ImportJobBase: Transferred 5.4065 MB in 41.6744 seconds (132.8444 KB/sec)
19/04/07 22:45:09 INFO mapreduce.ImportJobBase: Retrieved 92416 records.
[training@localhost ~]$ hdfs dfs -tail /loudacre/acoounts/CA/part-m-00000.avro
N�����P
esWardX 46 Essex �
                        bank��5R
                                 818554995>�_
qpZ�82)�                                      ������C�WnLan�617 G
 �������atHErin
                  Little"2247 �	e1F,3295181364B�
                                                             �!�
                                                                ���=
                                                                      !�@Graves&4447 Farl.<Berkele��47!!19��909210>b�!!�����QLeo�	Lamm 900 Cloverm	Jr
                 ��ߥ!!
                      JuanakHndel(2573 McDowellj$�Y�
90a067>h�A���                                         8152>�`8����MDorothy*�($4717 Jerom5�Z�4
(mble&2306 ��VN���326260Bd���


Mar
Pott!�(086 Concordd:�>
                        5592a�6>���������M��ŧ�P
                                                    JudithLindgren(1890 Ta��BU�=14135497>g��g@����M�����QDar�GonzalA� 2777 HighE�{
 Mojav�
       3566193884B
                    �!�
                       �Яg8MarjorieMorri�^($4417 CoffmdR01213280������a�Ű

He�	P
Perez$1093 Columbia))!�taB	�	
                                         1213B!�

                                                 ����a�tViolet
                                                               Searcy(2601 Twin Oaks-'fF^0!. 415232719>2�!.,����N�����OD
                  �
My$z Reeve"
J�723�
        8246F��A�
<                ������
