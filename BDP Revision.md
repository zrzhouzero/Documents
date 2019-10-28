### Week 1 Introduction

Three main dimensions of big data:
**Volume**, **Velocity**, **Variety**, Veracity*
*Slide 13*

**Scale "out", not "up"**
*Slide 39, 40*

Failures are common
*Slide 41*

Moving and running the processing code to the nodes where the data are stored
*Slide 42*

---

### Week 2 Hadoop, HDFS, YARN

Virtualisation, Hypervisor
*Slide 20*

Storage Hierarchy, Accessing Efficiency
*Slide 47, 48*

HDFS (Hadoop Distribution File System)
**write-once, read-many**

highly fault-tolerant, deployed on low-cost hardware
high throughput
streaming access to file system data enabled

Name Node: **file system tree** and **metadata**
HDFS **supports** traditional hierarchical file organisation

Data Nodes: serving read and write requests from clients
Block report: contains a list of all blocks on a data node
Heartbeat: receipt of a heartbeat implies the data node functions properly
**Replica Placement**
Communication tetween two nodes in different racks has to go through switches

Placing replicas on unique racks

1. prevents losing data when an entire rack fails
2. allows use of bandwidth from multiple racks when reading data
3. increase the cost of writes - one write needs to transfer blocks to multiple racks

*Slide 69-105*

YARN - computing resources management (scheduling)

Hadoop 1.0 Issues:

1. Scalability bottleneck due to a single Job Tracker
2. The Hadoop framework became limited only to MapReduce processing paradigm

YARN is to relieve MapReduce by taking over the responsibility of **Resource Management and Job Scheduling**

Resource Manager (**Scheduler** and **Application Manager**)
Application Master
Container (a collection of physical resoures)
Node Manager (Monitoring resources usage)

YARN Workflow *Slide 161*

---

### Week 3 Hive and Pig

MapReduce is a major step backwards

1. MapReduce is a step backwards in database access (Schemas are good)
2. MapReduce is a poor implementation (no index, no other option but brute force)
3. MapReduce is not novel (Partition a large dataset into small datasets, Cluster using a combination of partitioned tables, partitioned execution, and hash based splitting, Execute aggregates with and without group by clauses **in parallel**, Parallel database system) 20 years old
4. MapReduce is **incompatible** with DBMS tools
5. MapReduce misses features
   * Bulk loader
   * Indexing
   * Updates
   * Transactions
   * Integrity constraints
   * Referential integrity
   * Views

*Slides 3-34*

Hive
ETL (Extract, Transform, Load) and Data warehousing tool
Developed on top of HDFS
*Slides 41-62*

Pig
Pig Latin, SQL-like language, built-in operators
Data flow language, procedural language, can handle structured, unstructured and semi-structured data
*Slides 64-124*

---

### Week 4 Mahout, Solr, Storm, Maven

**Mahout**
primarily for scalable machine learning algorithms
ready-to-use framework
data mining on large volumes of data
Mahout recommender engine
*Slides 9-45*

**Solr**
search servers / search engines
inverted index
*Slides 49-82*

**Storm**
Real-time computation system
spout, stream, bolt
**Spout** accepts input data from raw data sources
**Stream** is an unordered sequence of tuples
**Bolts** are logical proessing units. Spout pass data to bolts, bolts process and produce a new output stream
**Bolts** can perform the operations of filtering, aggregation, joining, and interating with data source and databases.
Storm keeps the topology always running.
**Task** - the execution of each and every spout and bolt by Storm is called as "Tasks"
**Worker** - a topology runs in a distributed manner, on multiple worder nodes. A worker may process more than 1 tasks.
*Slides 85-101*

**Maven**
Maven is a software project management and comprehension tool
Java-based, POM (project object model)
*Slide 103-123*

---

### Week 5 MapReduce Basics

**MAP**
iterate over a large number of records
extract something of interest from each

**Combine** and **Partition**

Shuffle (moving mapper outputs to the reducers) and sort intermediate results

**Reduce**
aggregate intermediate results
generate final output

**Local Aggregation**
intermediate results are written to local disk befor being sent over the network
it uses in-mapper combiner
it preserves state across multiple inputs

In-mapper combining

Pros:

* provide programmers control over (when and how)
* More efficient than using actual combiners
  * With in-mapper combining, the mappers will generate only those key-value pairs need to be shuffled across the network

Cons:

* Scalability bottleneck
  * Memory size
  * Solution to limited memory, periodically
    * "block" input key-value pairs
    * "flush" in-memory data structure

---

### Week 6 MapReduce Co-occurrence Matrix

**Co-occurrence matrix**
distributional profiles as a way of measuring semantic distance
semantic distance is useful for many language processing tasks

**Pairs Approach** vs **Strips Approach** (better performance for most situations)
*Slides 58-109*

---

### Week 7 Clustering and Classification

|            Classification            |         Clustering         |
| :----------------------------------: | :------------------------: |
|              Supervised              |        Unsupervised        |
|       Known number of classes        | Unknown number of clusters |
|       Based on a training set        |     No prior knowledge     |
| Used to classify future observations |  Used to understand data   |

**Clustering**

Similarity or Distance Measure - Norms
Euclidean distance (L<sub>2</sub>-Norm)
Manhattan distance (L<sub>1</sub>-Norm)
L<sub>r</sub>-Norm d(x, y) = $[\sum_{i = 1}^{n} |x_{i} - y_{i}|^{r}]^{1/r}$

Similarity or Distance Measure - Cosine
$sim(x,y) = \frac{\sum_{i = 1}^{n}x_{i}y_{i}}{\sum_{i = 1}^{n}x_{i}^{2}\sum_{i = 1}^{n}y_{i}^{2}}$

Similarity or Distance Measure - Jaccard
$J(A,B) = \frac{|A \cap B|}{|A \cup B|}$

**K-Means**

Lloyd's Algorithm
Select - Assign - Refine
Euclidean Distance
Objective function, Knee finding (Elbow finding)

**Classification***

Gradient Descent

---

### Week 8 Graphs

*shortest paths*
*minimum spanning trees*
*max flow*
*identify special nodes and communities*
*bipartite matching*
*PageRank*

**Graph Representations**

1. Adjacency List
   * Advantages
     * much more compact representation
     * easy to compute over outlinks
   * Disadvantages
     * much more difficult to compute over inlinks
2. Adjancency Matrices
   * Advantages
     * amenable to mathematical manipulation
     * iteration over rows and columns corresponds to computations on outlinks and inlinks
   * Disadvantages
     * lots of zeroes for sparse matrices
     * lots of wasted space

**Algorithms**

1. Dijkstra' Algorithm

   efficient but not suitable for MapReduce (single processor machine)

2. Parallel Breadth-First Search

   Stopping Criterion: the algorithm can terminate when shortest distances at every node no longer changes

---

### Week 9 Inverted Indexing for Text Retrieval

Three components: **Crawler / Spider**, **Inverted Index Builder**, **Response to user queries**

Nearly all information retrieval search engines rely on Invered Index
Each posting is comprised of a document id and **a payload** (information about occurrences of the term in the document)
*Slide 32*

Given a query, retrieval involves fetching postings lists associated with query terms and traversing the postings to compute the result set

The size of an inverted index **varies**, depending on the payload stored in each posting

Postings lists are usually too large to store in memory and most be held on disk, usually in compressed form

**value-to-key** conversion design pattern
*Slide 80*

Improved Algorithm (Partitioner)

**Distributed Retrieval***
Document Partitioning and Term Partitioning

---

### Week 10 Processing Relational Data

**"Value-to-key conversion" design pattern**: form composite intermediate key

$k$ &rarr; $(k,v_{i})$  --------> $(k, v_{i})$ &rarr; $(v_{i},r)$

**Data-warehousing & Hadoop**

**OLAP vs. OLTP**
OLTP (OnLine Transactional Processing) systems provide source data to data warehouses
OLAP (Online Analytical Processing) Systems help to analyse it
Hadoop-based Solution: **HIVE** and **PIG**

**Reduce-Side Join**
MapReduce guarantees all values with the same key are brought together
one-to-one join / one-to-many join
inner join: *if no tuple in the other dataset shared the join key, the reducer should do nothing for inner join*

**Map-Side Join**

Prerequisite of Map-Side Join

1. S and T are both divided into files, partitioned in the same manner by the join key
2. In each file, the tuples are sorted by the join key

***A map-side join is far more efficient than a reduce-side join since there is no need to shuffle the datasets over the network***

*No reducer is required, unless the programmer wishes to repartition the output or perform further processing*

**Memory-backed Join**

one of the two datasets completely fits in memory on each node

In-memory join > map-side join > reduce-side join

Limitations of each:

* In-memory join: memory
* Map-side join: sort order and partitioning
* Reduce-side join: general purpose

---

### Week 11 Spark*

Spark RDD (Resilient Distributed Dataset)
Spark automatically partitions RDDs and distribute partitions across nodes

Spark Transformation is a function: it takes existing RDDs as input and produces one or more RDDs as output.
Transformations are lazy: Transformations get executed when an action is called, they are not execute immediately. Spark maintains the record of which transformation is being called (through DAG (Directed Acyclic Graph)).

RDD lineage
Applying transformation built an RDD lineage, with the entire parent RDDs of the final RDD(s)
RDD lineage, also known as RDD operator graph or RDD dependency graph

Apache Spark RDD Operations - RDD Transformation
map(func), flatMap(), filter(func), union(dataset), intersaction(other-dataset), distinct(), reduceByKey(func, [numTasks])

Apache Spark Rdd Operations - RDD Action
count(), take(n), collect()

RDD lineage
While we create a new RDD from an existing Spark RDD, that new RDD also carries a pointer to the parent RDD in Spark

Lineage graph deals with RDDs so it is applicable up-till transformations

DAG shows the different stages of a spark job, it shows the complete task

By using RDD lineage graph, we can recompute missing or damaged partitions due to node failures - "Resilience" of RDD

---

