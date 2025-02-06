# Lab 4

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

* (Q1) What do you think the line `job.setJarByClass(Filter.class);` does?
  This line specifies which class Hadoop should use for packaging and distributing the job to the cluster.
* (Q2) What is the effect of the line `job.setNumReduceTasks(0);`?
  It makes the job map-only, meaning there are no reduced steps, data goes straight from the map output to the final output.
* (Q3) How many lines do you see in the output?
  27972 lines
* (Q4) How many files are produced in the output?
  5 files were created for the big dataset, and 1 file was created for the small dataset.
* (Q5) Explain this number based on the input file size and default block size.
  The large file is 148.5 MB, and with a default block size of 32 MB, it results in approximately 5 blocks, leading to 5 output files.
  The small file is only 3 MB, which fits within a single block, so only one output file is produced.
* (Q6) How many files are produced in the output directory and how many lines are there in each file?
  A total of 3 files are generated: 1 _SUCCESS file and 2 output files.
  /part-r-00000 contains 4 lines.
  /part-r-00001 contains 0 lines.
* (Q7) Explain these numbers based on the number of reducers and the number of response codes in the input file.
  Since two reducers were used, Hadoop created two output files along with a _SUCCESS file. However, all four response codes were assigned to the first reducer, leading to all data being written to /part-r-00000 while /part-r-00001 remains empty.
* (Q8) How many files are produced in the output directory and how many lines are there in each file?
  A total of 3 files are produced: 1 _SUCCESS file and 2 output files.
  /part-r-00000 contains 5 lines.
  /part-r-00001 contains 2 lines.
* (Q9) Explain these numbers based on the number of reducers and the number of response codes in the input file.
  With two reducers, Hadoop generated two output files and one _SUCCESS file. Since there are seven unique response codes in the dataset, each reducer's output file contains the response codes assigned to it—five codes in the first reducer's file and two in the second.
* (Q10) How many files are produced in the output directory and how many lines are there in each file?
  A total of 3 files are produced: 1 _SUCCESS file and 2 output files.
  /part-r-00000 contains 1 line.
  /part-r-00001 contains 0 lines.
* (Q11) Explain these numbers based on the number of reducers and the number of response codes in the input file.
  Since two reducers were used, Hadoop created two output files and one _SUCCESS file. However, after filtering, only response code 200 remained, which hashes (via hashCode mod 2) to the first reducer. As a result, all data is stored in /part-r-00000, leaving /part-r-00001 empty.
