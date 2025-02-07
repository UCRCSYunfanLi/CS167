# Lab 5

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

* (Q1) Do you think it will use your local cluster? Why or why not?
  It does not, since the configuration is not set to use any external cluster manager, Spark will not use your local cluster. 
* (Q2) Does the application use the cluster that you started? How did you find out?
  It does, the web interface shows a completed application.
* (Q3) What is the Spark master printed on the standard output on IntelliJ IDEA?
  Using Spark master 'local[*]'
* (Q4) What is the Spark master printed on the standard output on the terminal?
  Using Spark master 'spark://class-162:7077'
* (Q5) For the previous command that prints the number of matching lines, how many tasks were created, and how much time it took to process each task.
  2 tasks were created, with the following times
  1-481ms
  2-518ms
* (Q6) For the previous command that counts the lines and prints the output, how many tasks in total were generated?
  Number of lines in the log file 30970. The file 'nasa_19950801.tsv' contains 27972 lines with response code 200.
* (Q7) Compare this number to the one you got earlier.
  I got 4 tasks, 2 more than before.
* (Q8) Explain why we get these numbers.
  These extra tasks correspond to the identification of lines with the desired code.
* (Q9) What can you do to the current code to ensure that the file is read only once?
  Stores the data in memory after the first read. Subsequent operations will use the in-memory data rather than reading the file again. 
* (Q10) How many stages does your program have, and what are the steps in each stage? 
  2 stages.
  Stage 0: Textfile, map, countByKey.
  Stage 1: countByKey.
* (Q11) Why does your program have two stages?
  Spark breaks down the job into multiple stages based on shuffle boundaries.
  Stage 0: Reads data and applies the mapToPair transformation before the shuffle.
  Stage 1: Executes countByKey(), which involves shuffling data to group response codes together.
