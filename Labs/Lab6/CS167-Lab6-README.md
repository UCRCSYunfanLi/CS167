# Lab 6

## Student information

* Full name:Yunfan Li
* E-mail:yli971@ucr.edu
* UCR NetID:yli971
* Student ID:862449438

## Answers

* (Q1) What are these two arguments?
val command: String = args(0) 
val inputfile: String = args(1)

* (Q2) What is the type of the attributes `time` and `bytes` this time? Why?
It is string. When we commented out the schema inference, Spark treated all values as strings instead of detecting their actual types, even though time and bytes were originally integers.

* (Q3) How many jobs does your program have? List them here, and describe what each job is performing.
3 jobs
Collect: Brings all results into driver memory as an array
SortKey: Used internally for sorting operations in Spark SQL queries
Countbykey: filters through log lines and maps the key value pairs then saves them 

* (Q4) How many stages does your program have? Why does it have two stages for the countByKey job?
7 stages in total but stage 4 is skipped

* (Q5) What are the longest two stages? Why do you think they are the longest?
Longest stages are stage 0 count by key, and stage 2 map function on this stage take 4s to complete

* (Q6) Copy the output of the command including the code, and Avg(bytes) table, as well as the runtime.
Using Spark master 'spark://class-162:7077'
Average bytes per code for the file 'nasa_19950630.22-19950728.12_lgall045.tsv'
Code,Avg(bytes)
200,22739.652244386536
302,79.0597341807485
304,0.0
403,0.0
404,0.0
500,0.0
501,0.0
Command 'avg-bytes-by-code' on file 'nasa_19950630.22-19950728.12_lgall045.tsv' finished in 9.840532516000001 seconds

* (Q7) How many jobs does your program have? List them here, and describe how they compare to the previous program.
4 jobs instead of 3 first job is load and second is show and the third is collect so is the fourth

* (Q8) How many stages does your program have? How did the number of stages affect the run time? Is this program slower or faster?
5 stage and stage 3 is skipped
Average bytes per code for the file 'nasa_19950630.22-19950728.12_lgall045.tsv'
Code,Avg(bytes)
200,22739.652244386536
302,79.0597341807485
501,0.0
404,0.0
403,0.0
500,0.0
304,0.0
Command 'avg-bytes-by-code' on file 'nasa_19950630.22-19950728.12_lgall045.tsv' finished in 3.430740119 seconds
The program ran faster with SQL it didn't have to countbykey and map which made the program longer


* (Q9) Visit this link [http://localhost:4040/SQL](http://localhost:4040/SQL), on that page click on the `collect at AppSQL`. Observe the graph which represents how your query was processed. Then, scroll to the end of bottom of the page, and click on `> Details`. You will notice two parts `+- == Final Plan ==` and `+- == Initial Plan ==`. Copy those plans here, and discuss. Which plan is longer and more complicated? Which plan do you think is more optimal?
The final plan has more steps and more variables that it needs to work through the initial plan is much more optimal as it has less amount of steps
+- == Final Plan ==
   
HashAggregate (6)+- AQEShuffleRead (5)+- ShuffleQueryStage (4), Statistics(sizeInBytes=1080.0 B, rowCount=27)+- Exchange (3)+- * HashAggregate (2)+- Scan csv  (1)
+- == Initial Plan ==
   HashAggregate (9)
   +- Exchange (8)
      +- HashAggregate (7)
         +- Scan csv  (1)
