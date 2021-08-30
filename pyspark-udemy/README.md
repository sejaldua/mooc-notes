# Python-and-Spark-for-Big-Data

## SYLLABUS

* Course Introduction
  * Promo/Intro Video
  * Course Curriculum Overview
  * Introduction to Spark, RDDs, and Spark 2.0

* Course Set-up
  * Set-up Overview
  * EC2 Installation Guide
  * Local Installation Guide with VirtualBox
  * Databricks Notebooks
  * Unix Command Line Basics and Jupyter Notebook Overview

* Spark DataFrames
  * Spark DataFrames Section Introduction
  * Spark DataFrame Basics
  * Spark DataFrame Operations
  * Groupby and Aggregate Functions
  * Missing Data
  * Dates and Timestamps

* Spark DataFrame Project
  * DataFrame Project Exercise
  * DataFrame Project Exercise Solutions

* Machine Learning
  * Introduction to Machine Learning and ISLR
  * Machine Learning with Spark and Python and MLlib
  * Consulting Project Approach Overview

* Linear Regression
  * Introduction to Linear Regression
  * Discussion on Data Transformations
  * Linear Regression with PySpark Example (Car Data)
  * Linear Regression Consulting Project (Housing Data)
  * Linear Regression Consulting Project Solution

* Logistic Regression
  * Introduction to Logisitic Regression
  * Logistic Regression Example
  * Logistic Regression Consulting Project (Customer Churn)
  * Logistic Regression Consluting Project Solution

* Tree Methods
  * Introduction to Tree Methods
  * Decision Tree and Random Forest Example
  * Random Forest Classification Consulting Project - Dog Food Data
  * RF Classification Consulting Project Solutions
  * RF Regression Project - (Facebook Data)

* Clustering
  * Introduction to K-means Clustering
  * Clustering Example - Iris Dataset
  * Clustering Consulting Project - Customer Segmentation (Fake Data)
  * Clustering Consulting Project Solutions

* Recommender System
  * Introduction to Recommender Systems and Collaborative Filtering
  * Code Along Project - MovieLens Dataset
  * Possible Consulting Project ? Company Service Reviews

* Natural Language Processing
  * Introduction to Project/NLP/Naive Bayes Model
  * What are pipelines?
  * Code Along

* Spark Streaming
  * Introduction to Spark Streaming
  * Spark Streaming Code-along!

## NOTES

### Introduction to Spark Concepts

* Big Data
  * a local process will use the computation resources of a single machine
  * a distributed process has access to the computational resources across a number of machines connected through a network
  * after a certain point, it is easier to *scale out* to many lower CPU machines, than to try to *scale up* to a high CPU

* Hadoop
  * distribute very large files across multiple machines using HDFS (Hadoop Distributed File System)
  * allows a user to work with large data sets
  * duplicates blocks of data for fault tolerance
  * uses MapReduce to allow for computations on large data

* MapReduce
  * MapReduce is a way of splitting a computation task to a distributed set of files (such as a HDFS)
  * consists of a Job Tracker and multiple Task Trackers
    * Job Tracker sends code to run on the Task Trackers
    * Task Trackers allocate CPU and memory (RAM) for the tasks and monitor the tasks on the worker nodes
  
* Big Data Key Concepts
  * using HDFS to distribute large data sets
  * using MapReduce to distribute a computational task to a distributed data set

* Spark
  * flexible alternative to MapReduce
  * can use data stored in a variety of formats: Cassandra, AWS S3, HDFS, etc.

* Spark vs MapReduce
  * MapReduce requires files to be stored in HDFS, Spark does not!
  * Spark can perform operations up to 100x faster than MapReduce
    * How? MapReduce writes most data to disk after each map and reduce operation; Spark keeps most of the data in memory after each transformation and can spill over to disk if the memory is filled

* Spark RDDs (Resilient Distributed Datasets)
  * 4 main features
    * distributed collection of data
    * fault tolerant
    * parallel operation, the ability to be partitioned
    * ability to use many data sources
  * components
    * Driver Program
    * Cluster Manager
    * Worker Nodes
  * with a large dataset, you don't want to calculate all the transformations until you are sure you want to perform them
  * Spark 2.0 is moving towards a DataFrame based syntax
  
* Machine Learning with MLLib
  * need to format data so it eventually just has one or two columns:
    * features, labels (supervised)
    * features (unsupervised)
  * requires a little more data processing work than some other machine learning libraries, but the big upside is that this exact same syntax works with distributed data

### Linear Regression

* History: all started in the 1800s with Francis Galton, who was investigating the relationship between the heights of fathers and their son
  * the son's hight tended to be closer to the average height of all people
  * a father's son's height tends to *regress* (drift towards) the mean (average) height
* Least Squares Method
  * fit a line by minimizing the ***sum of squares of the residuals***
    * residuals for an observation = difference between the observation and the fitted line
* Evaluation metrics for regression
  * Mean Absolute Error (MAE): mean of the absolute value of errors (just average error)
  $$\frac{1}{n}\Sigma_{i=1}^n |y_i - \hat{y}_i|$$
  * Mean Squared Error (MSE): mean of the squared error
    * larger errors are noticed / penalized more with MSE than MAE
  $$\frac{1}{n}\Sigma_{i=1}^n (y_i - \hat{y}_i)^2$$
  * Root Mean Squared Error (aka coefficient of determination): statistical measure of regression model
    * units make much more sense
  $$\sqrt{\frac{1}{n}\Sigma_{i=1}^n (y_i - \hat{y}_i)^2}$$
  * R Squared Values
    * just knowing R squared won't tell you the "whole story"
    * measure of how much variance your model accounts for
    * between 0 and 1
    * there is also adjusted R squared

### Logistic Regression

* Logistic Regression
  * classification = predicting categories
  * binary classification examples
    * spam vs "ham" emails
    * loan default (yes/no)
    * disease diagnosis
  * sigmoid (aka logistic) function takes in any value and outputs it to be between 0 and 1

* Confusion Matrix

|  | Predicted Positive | Predicted Negative | Prevalence $\frac{\Sigma \text{condition positive}}{\Sigma \text{total population}}$|
|--|--------------------|--------------------|------------|
| True Condition Positive | True Positive (TP) | False Negative (FN) (*type II error*) | True Positive Rate (TPR), Sensitivity, Recall $\frac{\Sigma \text{TP}}{\Sigma \text{condition positive}}$ |
| True Condition Negative | False Positive (FP) (*type I error*) | True Negative (TN) | False Positive Rate (FPR), Fall-out $\frac{\Sigma \text{FP}}{\Sigma \text{condition negative}}$ |
| Accuracy $\frac{\Sigma \text{TP} + \text{TN}}{\Sigma \text{total population}}$ | Positive Predictive Value (PPV), Precision $\frac{\Sigma \text{TP}}{\Sigma \text{prediction positive}}$ | False Omission Rate (FOR) $\frac{\Sigma \text{FN}}{\Sigma \text{prediction negative}}$ | Positive Likelihood Ratio (LR+) $\frac{TPR}{FPR}$ |
| | False Discovery Rate (FDR) $\frac{\Sigma \text{FP}}{\Sigma \text{prediction positive}}$ | Negative Predictive Value (NPV) $\frac{\Sigma \text{TN}}{\Sigma \text{prediction negative}}$ | Negative Likelihood Ratio (LR-) $\frac{FNR}{TNR}$ |

* Model Evaluation
  * ROC = Receiver Operator Characteristic / Curve
  * TPR (sensitivity) plotted versus FPR (1 - specificity)
  * perfect classification has AUC of 1 and hugs left corner

### Decision Trees and Random Forests

* Tree Methods Theory
  * root = the node that performs the first split
  * leaves = terminal nodes that predict the outcome
  * intuition behind splitting: entropy or information gain
  * random forests
    * use many trees with a random sample of features chosen as the split
      * works for both classification and regression tasks
    * if there is one very strong feature in the data set, most of the trees will use that feature as the top split-- random forests "decorrelate" the trees such that the average process can reduce the variance of the resulting model
  * gradient boosting involves 3 elements:
    * loss function to be optimized
      * function / equation you use to determine "how far off" your predictions are
        * examples: squared error or log loss
        * spark chooses this for us
    * weak learner to make predictions
      * decision trees are used as the weak learner in gradient boosting
      * it is common to constrain the weak learners: such as maximum number of layers, nodes, splits, or leaf nodes
    * an additive model to add weak learners to minimize the loss function
      * trees are added one at a time, and existing trees in the models are not changed
      * gradient descent procedure is used to minimize the loss when adding trees

* Gradient Boosting: 3-step process
  1. Train a weak model **m** using data samples drawn according to some weight distribution
  2. Increase the weight of samples that are misclassified by model m, and decrease the weight of samples that are classified correctly by model m
  3. Train next weak model using samples drawn according to updated weight distribution

In this way, the algorithm always trains models using data samples that are "difficult" to learn in previous rounds, which results in an ensemble of models that are good at learning different "parts" of training data. Basically, we are "boosting" weights of samples that were difficult to get correct.

### K-means Clustering

* Typical clustering problems
  * Cluster Similar Documents
  * Cluster Customers based on Features
  * Market Segmentation
  * Identify similar physical groups

* Overall goal is to divide data into distinct groups such that observations within each group are similar

* Algorithm
  * choose number of clusters "k"
    * elbow method: compute the sum of squared error (SSE) [sum of squared distance between each member of cluster to its centroid] for some values of k; plot k against SSE; choose the k at which the SSE decreases abruptly
  * randomly assign each point to a cluster
  * until clusters top changing, repeat the following:
    * for each cluster, compute the cluster centroid by taking the mean vector of points in the cluster
    * assign each data point to the cluster for which the centroid is the closest

### Recommender Systems

* Collaborative filtering produces recommendations based on the knowledge of users' attitude to items
  * Uses "wisdom of the crowd" to recommend items
* Content-based recommender systems focus on the attributes of the items and give you recommendations based on the similarity between them
* Collaborative filtering is more commonly used than content-based systems because gives better results and easier to implement
  * The algorithm has the ability to do feature learning on its own, which means that it can start to learn for itself what features to use
* These techniques aim to fill in the missing entries of a user-item association matrix
* `spark.ml` currently supports model-based collaborative filtering, in which users and products are described by a small set of latent factors that can be used to predict missing entries
* `spark.ml` uses the alternating least squares (ALS) algorithm to learn these latent factors
  * ALS = Matrix Factorization approach
  * to implement a recommendation algorithm, you decompose your large user / item matrix into lower dimensional user factors and item factors

### Natural Language Processing

* Create models from a text data source (straight from corpus of words)
* examples
  * Clustering News Articles
  * Suggesting similar books
  * Grouping Legal Documents
  * Analyzing Consumer Feedback
  * Spam Email Detection
* Basic NLP process
  * compile all documents (corpus)
  * featurize the words to numerics
  * compare features of documents
* TF-IDF = term freqency - inverse document frequency
  * can base everything off of word count (bag of words approach)
    * compare vectors with cosine similarity in N-dimensional space
  * improve BoW by adjusting word counts based on their frequency in corpus
  * Term Frequency = importance of the term within that document
    * TF(x,y) = number of occurrences of term x in document y
  * Inverse Document Freqency = importance of the term in the corpus
    * IDF(t) = log(N/dfx) where
      * N = total number of documents
      * dfx = number of documents containing the term x
  * $$w_{x,y} = TF_{x,y} * log(\frac{N}{df_x})$$

### Spark Streaming

* Spark Streaming is an extension of the core Spark API that enables scalable, high-throughput, fault-tolerant stream processing of live data streams
* Internally, Spark Streaming receives live input data streams and divides the data into batches, which are then processed by the Spark Engine to generate the final stream of results in batches
* Example
  * Split input line into a list of words
  * *MAP* each word to a tuple: (word, 1)
  * Then group (*REDUCE*) the tuples by the word (key) and sum up the second argument
