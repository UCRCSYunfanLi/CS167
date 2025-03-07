# Lab 9

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID:862449438

## Answers

* (Q1) What is the schema of the file after loading it as a Dataframe

    ```text
    root
     |-- Timestamp: string (nullable = true)
     |-- Text: string (nullable = true)
     |-- Latitude: string (nullable = true)
     |-- Longitude: string (nullable = true)
    ```

* (Q2) Why in the second operation, convert, the order of the objects in the  tweetCounty RDD is (tweet, county) while in the first operation, count-by-county, the order of the objects in the spatial join result was (county, tweet)?

    ```text
    The ordering of elements in tweetCounty RDD is strategically chosen based on the operation's primary focus and the nature of data processing involved. This approach ensures that data manipulations are conducted efficiently and align with the specific computational and business logic of the application.
    ```

* (Q3) What is the schema of the tweetCounty Dataframe?

    ```text
    root
     |-- g: geometry (nullable = true)
     |-- Timestamp: string (nullable = true)
     |-- Text: string (nullable = true)
     |-- CountyID: string (nullable = true)
    ```

* (Q4) What is the schema of the convertedDF Dataframe?

    ```text
        root
     |-- CountyID: string (nullable = true)
     |-- keywords: array (nullable = true)
     |    |-- element: string (containsNull = false)
     |-- Timestamp: string (nullable = true)
    ```

* (Q5) For the tweets_10k dataset, what is the size of the decompressed ZIP file as compared to the converted Parquet file?

    | Size of the original decompressed file | Size of the Parquet file |
    | - | - |
    |  `788,908 bytes (791 KB on disk)` | `313,975 bytes (319 KB on disk)` |

* (Q6) Write down the SQL query(ies) that you can use to compute the ratios as described above. Briefly explain how your proposed solution works.

    ```SQL
    WITH KeywordCounts AS (
    SELECT CountyID, count(*) AS KeywordCount
    FROM tweets
    WHERE array_contains(keywords, 'specific_keyword')
    GROUP BY CountyID
    ),
    TotalCounts AS (
    SELECT CountyID, count(*) AS TotalCount
    FROM tweets
    GROUP BY CountyID
    )
    SELECT 
    a.CountyID,
    a.KeywordCount,
    b.TotalCount,
    (CAST(a.KeywordCount AS FLOAT) / b.TotalCount) AS Ratio
    FROM KeywordCounts a
    JOIN TotalCounts b ON a.CountyID = b.CountyID;
    ```

    ```text
    This SQL query begins by calculating the number of tweets containing a specific keyword per county through two Common Table Expressions (CTEs)—KeywordCounts for tweets with the keyword, and TotalCounts for all tweets. It then joins these CTEs and computes the ratio of tweets containing the specified keyword to the total tweets in each county. This approach not only helps analyze the popularity of a keyword across different regions but also provides insights into the distribution of specific topics or discussions among the counties. This kind of analysis is particularly valuable for market analysis or public policy formulation.
    ```

* (Q7) When you run the application, open the spark WebUI and find your task. What's the Job name and its corresponding description?

    ```text
    Job Name: choropleth-map
    Description: This job executes a spatial operation to count the occurrences of a specific keyword across various counties and generates a choropleth map based on the count data. It involves reading a dataset, performing a spatial join, aggregating data by geographic regions, and writing the results to a shapefile for visualization.
    ```

* (Q8) When you run the application, open the spark WebUI and find your task. Can you verify your code is processing file in parquet format? Explain why.

    ```text
    Yes, you can verify that the code is processing a file in parquet format by observing the task details in the Spark WebUI. The job log will specifically mention reading or writing to a parquet file if the code includes operations involving `.parquet` methods. For instance, the use of `sparkSession.read.parquet(inputFile)` to read data and `convertedDF.write.mode(SaveMode.Overwrite).parquet(outputFile)` to write data clearly indicates that the application is handling data in parquet format. Additionally, the WebUI would display the path of the parquet files being accessed during these operations, confirming that the data is indeed in parquet format.
    ```
