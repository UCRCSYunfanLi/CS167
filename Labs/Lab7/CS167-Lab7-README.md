# Lab 8

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

* (Q1) What is the schema of the file? Copy it to the README file and keep it for your reference.

    ```
    root                                                                            
     |-- hashtags: array (nullable = true)
     |    |-- element: string (containsNull = true)
     |-- id: long (nullable = true)
     |-- lang: string (nullable = true)
     |-- place: struct (nullable = true)
     |    |-- country_code: string (nullable = true)
     |    |-- name: string (nullable = true)
     |    |-- place_type: string (nullable = true)
     |-- text: string (nullable = true)
     |-- time: string (nullable = true)
     |-- user: struct (nullable = true)
     |    |-- followers_count: long (nullable = true)
     |    |-- statuses_count: long (nullable = true)
     |    |-- user_id: long (nullable = true)
     |    |-- user_name: string (nullable = true)
    ```

* (Q2) What is your command to import the `tweets.json` file?

    ```shell
    mongoimport --db cs167 --collection tweets --file /home/cs167/tweets.json
    ```

* (Q3) What is the output of the import command?

    ```text
    2025-02-18T14:59:23.649-0800    connected to: mongodb://localhost/
    2025-02-18T14:59:23.696-0800    1000 document(s) imported successfully. 0 document(s) failed     to import.
    ```

* (Q4) What is your command to count the total number of records in the `tweets` collection and what is the output of the command?

    ```javascript
    command: db.tweets.countDocuments()
    output: 1000
    ```

* (Q5) What is your command for this query?

    ```javascript
        db.tweets.find(
        { "place.country_code": "JP", "user.statuses_count": { $gt: 50000 } },
        { "user.name": 1, "user.followers_count": 1, "user.statuses_count": 1, "_id": 0 }
        ).sort({ "user.followers_count": 1 })
    ```

* (Q6) How many records does your query return?
  16 records

* (Q7) What is the command that retrieves the results without the _id field?

    ```javascript
    db.tweets.find(
    { "place.country_code": "JP", "user.statuses_count": { $gt: 50000 } },
    { "user.name": 1, "user.followers_count": 1, "user.statuses_count": 1}
    ).sort({ "user.followers_count": 1 })
    ```

* (Q8) What is the command to insert the sample document? What is the result of running the command?

    ```javascript
    db.tweets.insertOne({ "user": { "name": "NewUser", "followers_count": "12345", "statuses_count": 100 }
    })
    ```


* (Q9) Does MongoDB accept this document while the followers_count field has a different type than other records?
    Yes, MongoDB does accept this document while the followers_count field has a different type than other records. MongoDB is a schema-less database, it allows for inconsistencies in data types within the same collection. 

* (Q10) What is your command to insert this record?

    ```javascript
    db.tweets.insertOne({
      "id": NumberLong("921633456941125634"),
      "place": "Tokyo"
    })
    ```


* (Q11) Where did the two new records appear in the sort order?
    user name "xyz2" is between followers_count {2100} and {higher value}.
    user name "xyz3" is the last.

* (Q12) Why did they appear at these specific locations?
    The placement of the two new records in the sorted order is determined by MongoDB's sorting rules.
    Descending Order: Higher values appear first. If a new record has a high followers_count, it will appear near the top.
    Ascending Order: Lower values appear first. If a new record has a relatively low followers_count, it will appear nearer the beginning of the list.

* (Q13) Where did the two records appear in the ascending sort order? Explain your observation.
    If followers_count is stored as a string, those records will appear at the beginning of the list when sorted in ascending order.
    BSON-type sorting places strings before numbers when sorting in.

* (Q14) Is MongoDB able to build the index on that field with the different value types stored in the `user.followers_count` field?
    MongoDB can build an index on user.followers_count, but if the field has mixed types (numbers and strings), the index will be inconsistent. Queries relying on indexing may fail or return incorrect results because sorting behavior is different for strings vs. numbers.

* (Q15) What is your command for building the index?

    ```javascript
    db.tweets.createIndex({ "user.followers_count": 1 })
    ```

* (Q16) What is the output of the create index command?

    ```text
    user.followers_count_1
    ```

* (Q17) What is your command for this query?

    ```javascript
    db.tweets.find(
      { "hashtags": { $in: ["job", "hiring", "IT"] } },
      { "text": 1, "hashtags": 1, "user.name": 1, "user.followers_count": 1, "_id": 0 }
    ).sort({ "user.followers_count": 1 })
    ```

* (Q18) How many records are returned from this query?

    ```
    24
    ```

* (Q19) What is your command for this query?
    ```javascript
    db.tweets.aggregate([
      { $group: { _id: "$place.country_code", tweets_count: { $sum: 1 } } },
      { $sort: { tweets_count: -1 } },
      { $limit: 5 }
    ```

* (Q20) What is the output of the command?
        [
  { _id: 'US', tweets_count: 153 },
  { _id: 'JP', tweets_count: 105 },
  { _id: 'GB', tweets_count: 89 },
  { _id: 'TR', tweets_count: 65 },
  { _id: 'IN', tweets_count: 56 }

]

* (Q21) What is your command for this query?
    ```javascript
    db.tweets.aggregate([
      { $unwind: "$hashtags" },
      { $group: { _id: "$hashtags", count: { $sum: 1 } } },
      { $sort: { count: -1 } },
      { $limit: 5 }
    ])
    ```

* (Q22) What is the output of the command?
  [
  { _id: 'ALDUBxEBLoveis', count: 56 },
  { _id: 'FurkanPalalı', count: 31 },
  { _id: 'LalOn', count: 31 },
  { _id: 'no309', count: 31 },
  { _id: 'job', count: 19 }

]

* (Q23) Are there any existing indexes? Explain your answer.
  
    Yes, there is the _id index and the user.follwers_count index.

    [
  { v: 2, key: { _id: 1 }, name: '_id_' },
  {
    v: 2,
    key: { 'user.followers_count': 1 },
    name: 'user.followers_count_1'
  }
  
]
    
* (Q24) What's the running time of the second query? Comparing to Q23, it is faster or slower?
    
    Query time for regex search 'happy': 10ms
    Total result count: 11

* (Q25) What do winningPlan and COLLSCAN mean?
    
    winningPlan: This describes how MongoDB executed the query.

    COLLSCAN: This means MongoDB scanned all documents instead of using an index.

* (Q26) What is the winningPlan? Did your query use the text index?
    
    The winningPlan is how MongoDB executed the query.

     Yes, the query used the text index, resulting in a much faster execution time.

* (Q27) You may notice that the nReturned of your query does not match the regex query. Do some explornations and explain why.
    
    Text search returns more results because it includes stemming and case insensitivity.
