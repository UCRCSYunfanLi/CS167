# Lab 10

## Student information

* Full name: Yunfan Li
* E-mail: yli971@ucr.edu
* UCR NetID: yli971
* Student ID: 862449438

## Answers

* (Q1) Fill the following table.
    
    The parameter values and the accuracy are based on the best model you obtained. The run time is the total run time printed by the program.
    
    | Parameter       | Value              |
    |-----------------|--------------------|
    | numFeatures     |    2048            |
    | fitIntercept    |    false           |
    | regParam        |    0.01            |
    | maxIter         |    15.0            |
    | threshold       |    0.0             |
    | tol             |    0.01            |
    | Test accuracy   | 0.8220790508868493 |
    | Run time        | 252.93066883400002 |



* (Q2) Fill the following table.

The parameter values and the accuracy are based on the best model you obtained. The run time is the total run time printed by the program.

| Parameter                                 | Value             |
|-------------------------------------------|-------------------|
| Number of worker nodes in your cluster    |        1          |
| Total number of cores in your cluster     |        4          |
| numFeatures                               |       2048        |
| fitIntercept                              |      false        |
| regParam                                  |       0.01        |
| maxIter                                   |       10.0        |
| threshold                                 |       0.25        |
| tol                                       |       0.01        |
| Test accuracy                             | 0.8200170508868493|
| Run time                                  | 287.74132723401231|



* (Q3) What difference do you notice in terms of the best parameters selected, the accuracy, and the run time between running the program locally and on your Spark cluster having multiple nodes?
    In a cluster environment, due to more abundant resources, a wider parameter grid can be explored, which helps find better parameter settings and potentially improve model accuracy. Also, the parallel processing capabilities of a cluster can significantly reduce the total runtime, especially when dealing with large datasets. In contrast, running locally might involve fewer parameter combinations due to hardware constraints, and it generally takes longer for data processing and model training, which could result in lower performance and accuracy of the model.

