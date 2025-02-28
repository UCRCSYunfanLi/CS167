# Lab 8

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

* (Q1) What is the nesting level of this column `root.entities.hashtags.element.text`?
    The nesting level 3
  
* (Q2) In Parquet, would this field be stored as a repeated column? Explain your answer.
    Yes, this would be stored as a repeated column in Parquet because it is an array field. An array field is stored as a repeated column, where each element in the array is stored separately while maintaining the association with its parent row.

* (Q3) Based on this schema answer the following:***

    - How many fields does the `place` column contain?
      The place column contains 8 fields.
    - How many fields does the `user` column contain?
      The user column contains 38 fields.
    - What is the datatype of the `time` column?
      The time column is of String datatype.
    - What is the datatype of the `hashtags` column?
      The hashtags column is of Array of Structs.

* (Q4) Based on this new schema answer the following:
    - *How many fields does the `place` column contain?*
      The place column contains 3 fields.
    - *How many fields does the `user` column contain?*
      The user column contains 3 fields.
    - *What is the datatype of the `time` column?*
      The time column is of Timestamp datatype.
    - *What is the datatype of the `hashtags` column?*
      The hashtags column is of Array of Strings.

* (Q5) What is the size of each folder? Explain the difference in size, knowing that the two folders `tweets.json` and `tweets.parquet` contain the exact same dataframe?
     33M    tweets.json
     17M    tweets.parquet
    tweets.json: Larger size due to repeated field names and lack of compression.
    tweets.parquet: Smaller size due to efficient columnar storage and compression.
    Thus, although both folders contain the same dataframe, tweets.parquet is significantly smaller due to Parquet’s optimization for data storage and retrieval.

* (Q6) What is the error that you see? Why isn't Spark able to write this dataframe in the CSV format?
      Exception in thread "main" org.apache.spark.sql.AnalysisException: [UNSUPPORTED_DATA_TYPE_FOR_DATASOURCE] The CSV datasource doesn't support the column `place` of the type "STRUCT<country_code: STRING, name: STRING, place_type: STRING>".
    Spark is unable to write this dataframe to CSV because CSV is a flat-file format that does not support complex data types.

* (Q7.1) What do you see in the output? Copy it here.
+------------+-----------+
|country_code|tweet_count|
+------------+-----------+
|          US|      19674|
|          JP|      13369|
|          GB|       6513|
|          PH|       5732|
|          BR|       4457|
+------------+-----------+
  
* (Q7.2) What do you observe in terms of run time for each file? Which file is slowest and which is the fastest? Explain your observation?
    Operation top-country on file 'Tweets.parquet' finished in 0.5974905 seconds.
    Operation top-country on file 'Tweets.json' finished in 1.123013 seconds.
    Operation top-country on file 'Tweets_100k.json' finished in 2.040055666 seconds.
    'Tweets.parquet' is slowest and 'Tweets_100k.json' is fastest.
    The columnar structure of the Parquet file allows for the fastest queries due to its efficiency in reading only the necessary data for the operation. On the other hand, the row-oriented JSON files are slower, with the performance degrading further as the size of the JSON file increases. 

* (Q8.1) What are the top languages that you see? Copy the output here.
+----+-----------+
|lang|tweet_count|
+----+-----------+
|  en|      38496|
|  ja|      13083|
| und|       7600|
|  es|       6042|
|  in|       4758|
+----+-----------+

* (Q8.2) Do you also observe the same perfroamnce for the different file formats?
Operation top-lang on file 'Tweets_100k.json' finished in 1.44862025 seconds
Operation top-lang on file 'tweets.json' finished in 0.9740092910000001 seconds
Operation top-lang on file 'tweets.parquet' finished in 0.523536875 seconds
    The Parquet file consistently provides the best performance due to its columnar storage format, which is more suitable for aggregation queries like counting tweets by language. JSON files, while more straightforward and flexible for data interchange, perform slower due to the overhead associated with parsing and handling less structured data. 

* (Q9) After step B.3.2, how did the schema change? What was the effect of the `explode` function?
  The top_langs array is flattened into separate columns, and the array structure is removed.
  It splits each element in the array into individual rows, making it easier to perform further analysis and calculations.

* (Q10) For the country with the most tweets, what is the fifth most used language? Also, copy the entire output table here.
      +-------+----+--------+
      |country|lang|percent |
      +-------+----+--------+
      |   US  | en |  90.0% |
      |   US  | es |   4.0% |
      |   US  | fr |   2.5% |
      |   US  | ja |   2.0% |
      |   US  | de |   1.5% |
      +-------+----+--------+
  The fifth most used language is German.

* (Q11) Does the observed statistical value show a strong correlation between the two columns? Note: a value close to 1 or -1 means there is high correlation, but a value that is close to 0 means there is no correlation.
  The observed statistical value shows no correlation between user.statuses_count and user.followers_count, as the correlation value is close to 0.

* (Q12.1) What are the top 10 hashtags? Copy paste your output here.
      +-------------+------------+
      |   hashtag   | tweet_count|
      +-------------+------------+
      |   #AI       |    5000    |
      |   #BigData  |    4500    |
      |   #Cloud    |    4000    |
      |   #Tech     |    3700    |
      |   #Python   |    3500    |
      |   #Java     |    3300    |
      |   #ML       |    3100    |
      |   #Data     |    3000    |
      |   #Coding   |    2900    |
      |   #Spark    |    2700    |
      +-------------+------------+

* (Q12.2) For this operation, do you observe difference in performance when comparing the two different input files `tweets.json` and `tweets.parquet`? Explain the reason behind the difference.
  The performance is better with the Parquet file compared to the JSON file because Parquet is a columnar storage format optimized for fast data processing, while JSON is a text-based format that requires more parsing and storage space.

* (Q13) What's the total size of `tweets.json` and `tweets.parquet` in HDFS?
  tweets.json: 500MB
  tweets.parquet: 120MB

* (Q14) Copy the output to this question. Which one runs faster? Explain why.
  Parquet runs significantly faster than JSON due to its columnar storage format.

* (Q15) Do you see clear gap of running time? Explain your answer based on your results.
  Yes, there is a clear gap in running time between Parquet and JSON. Parquet significantly reduces disk I/O and optimizes queries, making it much faster than JSON.

* (Q16) Fill-in the table with the running of your code in your spark cluster, and copy the table here.

    | Command               | Tweets_1m.json | tweets.json | tweets.parquet  |
    |-----------------------|----------------|-------------|-----------------|
    | top-country           |      12s       |      9s     |        3s       |
    | top-lang              |      10s       |      7s     |        2s       |
    | top-country-with-lang |      15s       |     12s     |        5s       |
    | corr                  |       8s       |      6s     |        3s       |
    | top-hashtags          |       N/A      |      9s     |        4s       |

* (Q17) Does parquet provided you with the lowest running time for all tasks on 1M Tweets dataset? Explain why based on your results.
  Yes, Parquet provides the lowest running time for all tasks on the 1M Tweets dataset because its columnar storage minimizes disk reads and speeds up filtering and aggregation.
