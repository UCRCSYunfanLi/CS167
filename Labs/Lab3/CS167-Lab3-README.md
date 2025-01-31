# Lab 3

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

1. ***(Q1) Compare `bytesRead` and `length`, are they equal? Use one sentance to explain why.***
The bytesRead and length are not equal because the program encounters an IOException: No FileSystem for scheme "hdfs", preventing it from successfully reading any bytes from the file. This issue occurs because the Hadoop environment or configuration is not properly set up.


2. ***(Q2) Copy the output of this command.***
cs167@class-200:~/cs167$ hdfs dfsadmin -report
Configured Capacity: 207929917440 (193.65 GB)
Present Capacity: 180683001856 (168.27 GB)
DFS Remaining: 180682977280 (168.27 GB)
DFS Used: 24576 (24 KB)
DFS Used%: 0.00%
Replicated Blocks:
        Under replicated blocks: 0
        Blocks with corrupt replicas: 0
        Missing blocks: 0
        Missing blocks (with replication factor 1): 0
        Low redundancy blocks with highest priority to recover: 0
        Pending deletion blocks: 0
Erasure Coded Block Groups: 
        Low redundancy block groups: 0
        Block groups with corrupt internal blocks: 0
        Missing block groups: 0
        Low redundancy blocks with highest priority to recover: 0
        Pending deletion blocks: 0

-------------------------------------------------
Live datanodes (1):

Name: 169.235.28.200:9866 (class-200.cs.ucr.edu)
Hostname: class-200.cs.ucr.edu
Decommission Status : Normal
Configured Capacity: 207929917440 (193.65 GB)
DFS Used: 24576 (24 KB)
Non DFS Used: 27230138368 (25.36 GB)
DFS Remaining: 180682977280 (168.27 GB)
DFS Used%: 0.00%
DFS Remaining%: 86.90%
Configured Cache Capacity: 0 (0 B)
Cache Used: 0 (0 B)
Cache Remaining: 0 (0 B)
Cache Used%: 100.00%
Cache Remaining%: 0.00%
Xceivers: 0
Last contact: Fri Jan 31 12:24:24 PST 2025
Last Block Report: Fri Jan 31 12:22:30 PST 2025
Num of Blocks: 0


3. ***(Q3) How many live datanodes are in this cluster?***
HDFS cluster currently has 1 live DataNode

4. ***(Q4) How many replicas are stored on the namenode? How many replicas are stored in the datanodes?***
The Namenode does not store actual replicas of data blocks. There are 0 live Datanodes in the cluster.


5. ***(Q5) How many replicas are stored on the datanode uploading the file? How many replicas are stored across other datanodes?***
Replicas on the uploading datanode: 0
Replicas on other datanodes: 0


7. ***(Q6) Compare your results of Q4 and Q5, give one sentence to explain the results you obtained.***
Both Q4 and Q5 confirm that no replicas exist. The namenode stores metadata but not actual file replicas, and since the cluster has only one datanode with Num of Blocks: 0, no replicas have been created or stored in the system.


8. ***(Q7) Include the output of the three cases above in your README file.***


  | offset | length | bytesRead  | numMatchingLines |
  | ------ | ------ | ---------- | ---------------- |
  | 500    | 1000   |    1000    |      11          |
  | 12000  | 1000   |    1000    |      13          |
  | 100095 | 1000   |    1000    |      10          |
