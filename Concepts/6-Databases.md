# Types of Data

## Structured Data

**What is it?** Type of data that conforms to a certain structure. Stored in a database in a *normalized* fashion. 
**Example:** Customer stored in a database row. The `customerId` would be an `integer type`, `name` would be a `string type`, `age` would be `integer type`.

**How does it work?:**

- Does not need any sort of data preparation before it can be interacted.
- Every column of a database row has pre-defined rules for data that is meant to be ***persisted*** in it.
    - We know the exact type we are dealing with
    - So if we are working on a `string type` then we can run string operations without worries of errors
- Managed by SQL (structure query language)
<br>

## Unstructured Data

**What is it?**

Has no definite structure. It is a mixed type of data consisting of text, image files, videos, pdfs, blob, word documents, etc.

**Example:**

This kind of data is great when running ***data analytics***, where we receive data streams from multiple different sources like IoT devices, social networks, web portals, sensors, etc.

![unstructured-data](resources/unstructured-data.png)

**How does it work?:**

- Cannot directly interact with unstructured data.
    - We need to make it flow through a preparation stage and segregates based on business logic to extract information.
<br>

## Semi-structured Data

**What is it?** A mix of structured and unstructured data. Often stored in data transport formats like XML, JSON.
<br>

## User State

**What is it?** Contains the information of activity the user performs on the website.
**Example:** In an e-commerce website: user browses through several products, sorts products, clicks on recommended products, adds items to wish list, etc. These are all user states.

**How does it work?**

- Storing user state helps business improve UX.
- Persisting user state allows us to also continue where they left off on the next log-in.
<br><br>

# Types of Databases

## Relational Databases

**What is it?** Persists data containing relationships: 1 to 1, 1 to many, many to many, many to one. It uses a data model and data is organized in tables having rows and columns and is interacted using SQL.
**Example:** MySQL, PostgreSQL. Apps like Facebook and LinkedIn.

**How does it work?**

- **Data consistency**
    - Data is saved in a normalized fashion.
    - ***Normalized data*** occurs in only one table in its simplest and atomic form and is not spread throughout the database.
        - Updating a table should only affect that table and not other tables in the database
    - Helps maintain consistency
        - If we need to update data we can just update it in one place.
        - As opposed to updating the entity throughout multiple tables which could prove *inconsistency*.
    - Example:
        1. **Initial Table - "Customers":**
            - Before normalization, you have a single table "Customers" with the columns: “CustomerID”, “CustomerName”, “Address”, “City”, “State”, and “Zip”.
            - The issue with this structure is that it can lead to data redundancy. For example, if the same customer has multiple addresses, their name will be repeated for each address.
        2. **Normalization Process:**
            - The goal is to divide the data into two tables to reduce redundancy and improve the organization.
        3. **Creating "Customers" and "Addresses" Tables:**
            - **"Customers" Table**: This table is simplified to hold only customer-related information, i.e., “CustomerID” and “CustomerName”. Each customer has a unique ID.
            - **"Addresses" Table**: This table holds address-related information: “AddressID”, “Address”, “City”, “State”, and “Zip”. Importantly, it includes an “AddressID” which is a unique identifier for each address.
        4. **Linking Tables with a Foreign Key:**
            - To maintain the relationship between customers and their addresses, the tables are linked.
            - This can be done by adding a “CustomerID” column to the “Addresses” table. Each entry in the “Addresses” table relates to a customer in the “Customers” table.
            - The “CustomerID” in the “Addresses” table acts as a foreign key referencing the “CustomerID” in the “Customers” table.
        5. **Benefits of This Normalization:**
            - **Reduces Redundancy**: A customer's name is stored only once in the “Customers” table, no matter how many addresses they have.
            - **Improves Data Integrity**: Changes to a customer's name only need to be made in one place.
            - **Flexibility**: Easily accommodate customers with multiple addresses.
        6. **Illustration with Sample Data:**
            
            "Customers" Table:
            
            | **CustomerID** | **CustomerName** |
            | --- | --- |
            | 1 | John Doe |
            | 2 | Jane Smith |
            
            "Addresses" Table:
            
            | **AddressID** | **CustomerID** | **Address** | **City** | **State** | **Zip** |
            | --- | --- | --- | --- | --- | --- |
            | 101 | 1 | 123 Main St | Townville | TX | 12345 |
            | 102 | 2 | 456 Oak Ave | Cityside | NY | 23456 |
            | 103 | 1 | 789 Pine Rd | Lakeview | CA | 34567 |
            
            Here, John Doe (CustomerID 1) has two addresses (AddressID 101 and 103), but his name is stored only once in the “Customers” table.
            
        
        This normalization approach, typically up to the third normal form, is a common practice in relational database design, striking a balance between reducing redundancy and maintaining efficient data access.
        
- **ACID transactions**
    - Atomicity, Consistency, Isolation, Durability (ACID)
    1. **Atomicity**:
        - **Concept**: Transactions are all-or-nothing. This means every operation within the transaction must succeed; if any part of the transaction fails, the entire transaction fails and the database state is left unchanged.
    2. **Consistency**:
        - **Concept**: Transactions must bring the database from one valid state to another valid state, maintaining all predefined rules, such as constraints, cascades, and triggers.
    3. **Isolation**:
        - **Concept**: Transactions are isolated from each other until they’re complete. The results of a transaction are not visible to other transactions until the transaction is committed.
    4. **Durability**:
        - **Concept**: Once a transaction has been committed, it is permanent, even in the event of a system failure.
    
    **ACID Transaction Example - Bank Account Transfer:**
    
    - **Scenario**: Transferring $100 from Account A to Account B.
    - **Steps in the Transaction**:
        1. Begin the transaction.
        2. Debit $100 from Account A.
        3. Credit $100 to Account B.
        4. Commit the transaction.
    - **ACID Properties Applied**:
        - **Atomicity**: If any step fails, say debiting from Account A succeeds but crediting to Account B fails, the whole transaction is rolled back. Account A is returned to its original state as if the transaction never happened.
        - **Consistency**: The transaction will ensure that the total amount across both accounts remains the same. It adheres to the rule that money can neither be created nor destroyed during the transaction.
        - **Isolation**: Until the transaction is committed, no other transaction can see the intermediate steps (like the money debited but not yet credited).
        - **Durability**: Once the transaction is committed and $100 has moved from Account A to Account B, this change is permanent. Even if the system fails immediately after, this change will not be lost.
    
    **Conclusion:**
    
    - The system transitions from State A (before transaction) to State B (after transaction), maintaining consistency. State A and State B are both valid and consistent states of the database.

---

### When to pick relational databases

- When you need:
    - **Strong consistency**
        - Some examples are stock trading and personal banking
    - **Transactions** (ACID)
    - **Relationships** (1 to 1, 1 to many, many to 1, many to many)
- **Transactions and data consistency**
    - Software that has anything to do with money or numbers and contains transactions.
        - ***ACID*** and ***consistency*** is very important
    - Relational DBs comply with the ACID rule
- **Large community**
    - Seasoned engineers on the tech are readily available.
- **Storing relationships**
    - If we require a ton of relationships, like:
        - a social network app
        - knowing what friends live in a particular city or what restaurant they ate in
<br>

## NoSQL Database

**What is it?** Essentially a JSON-based database built for Web 2.0 that is unstructured data.
<br>**Example:** Used for high-frequency read/writes like real-time sport apps, MMO's, micro-blogging, etc.

**How does it work?:**

- **Scalability**
    - One big limitation in SQL is scalability.
        - SQL databases needs to be sharded, replicated to make them run smoothly on a cluster.
        - Needs careful planning, human intervention and a certain skillset.
    - As opposed to NoSQL
        - Databases can add new server nodes on the fly very easily without any human intervention.
    - Nowadays, there are billions of users connected with each other on social networks.
    - Massive amount of data is generated every microsecond, and we need infrastructure to manage exponential growth.
- **Ability to run on clusters**
    - NoSQL databases are designed to run intelligently on clusters (no human intervention)
        - Allows to scale horizontally over a cluster and across data centers
    - However, it sacrifices: strong consistency, ACID transactions
        - Instead it uses: ***eventually consistent***
        - Many NoSQL systems adhere to the CAP theorem, which states that a distributed data system can only simultaneously provide two of the following three guarantees: Consistency, Availability, and Partition Tolerance. **NoSQL databases often prioritize Availability and Partition Tolerance, sometimes at the expense of strong Consistency.**
        - This leads to the concept of "eventual consistency," where the database does not guarantee immediate consistency across all nodes but assures that all changes will propagate over time, resulting in consistency at a later point.

### Pros/Cons

- **Pros**
    - **Learning curve is not steep and schemaless**
        - More *flexibility* as opposed to SQL databases where our time needs to go in designing well-normalized tables and setting up relationships.
        - You don't have to be a *pro* at database design
    - **Fast lookup**
        - Instead of a SQL query, we can fetch the JSON object using its key
        - Allowing for a constant O(1) operation.
- **Cons**
    - **Inconsistency**
        - Data is not normalized which introduces risk of it being inconsistent.
        - Hard for DEVs to remember all locations of an entity in the database leads again to inconsistency
    - **No support for ACID transactions**
        - Limited to a certain entity hierarchy or a small deployment region where they can lock down nodes to update them.

---

### When to pick a NoSQL Database?

- Handling a large number of read-write operations
    - The ability to add nodes on the fly allows them to handle more concurrent traffic
    - Built to handle big data with minimal latency
    - Scalability is very easy
- Flexibility with data modelling
    - Expectation of things changing at a rapid pace and unsure of data model during initial phase
- Eventual consistency over strong consistency
- Running data analytics
<br>

---

### Is NoSQL More Performant Than SQL?

- From a performance benchmarking standpoint, SQL and NoSQL stand side-by-side
    - What matters the most is how we design our systems to **leverage** one of the two

<br>

## Document-Oriented Database

**What is it?** Leading types of NoSQL databases. It stores data in a document-oriented model in independent documents. The data is generally semi-structured and stored in a JSON-like format.

**Examples:** MongoDB, CouchDB, OrientDB, Google Cloud Datastore, and Amazon DocumentDB.

**Why should I choose it?:**

- Need a flexible schema that will change often
    - Unsure about the database scheme at the beginning and needs to change over time
- The need to scale fast and stay highly available
    - Provides horizontal scalability and performant read-writes since they cater to CRUD use cases.
    - These includes cases where we don’t need complex relational logic and need quick persistence and data retrieval.

## Graph Database

**What is it?** Part of the NoSQL family. It stores data in nodes/vertices and edges in the form of relationships. Each node in a graph DB represent an entity and each edge represents the relationship between entities.

![graph-database](resources/graph-database.png)

**Why use it?**

- There may come a time when relationships stored in a relational DB become way too slow to query.
    - Too many joins across multiple tables
- Graph data model allows for data to be accessed with low latency
- When you need to store complex relationships

**Examples:**

- Relationships between users in social networks
    - Facebooks graph search feature uses graph data structure.
    
    ![graph-connections-db](resources/graph-connections-db.png)
    
    - The relationships are multi-dimensional, the network maps links between users and their favorite restaurants, cuisines, sports teams, etc.
    - These relationships allow for recommendations of where to travel, dine, etc.
    - **How does it work?**
        - Adjacency matrix helps figure out if a relationship between two nodes exists in O(1)
            - If the nodes in a graph contain a lot of edges, we represent it with adjacency matrix
            - If edges are sparse then we represent the graph using adjacency list
        - Graphs are traversed using search algorithms DFS, BFS.
            - DFS is used to find: paths and connectivity between nodes, cycles
            - BFS is used to find the shortest path between nodes

## Key-Value Database

**What is it?** Part of the NoSQL family, uses a simple key-value pairing method to store and quickly fetch data with minimum latency.

**Examples:** Redis, Hazelcast, Riak, Volemort, and Memcached.

**Why should I choose it?:**

- Ensures constant O(1) time to be used for caching application data
- Used to achieve a consistent state in distributed systems
- Caching, persisting user state and sessions, managing real-time data, implementing queues and create leaderboards in online games

## Time Series Database

**What is it?** Optimized for tracking and persisting data that is continually read and written in the system over a period of time. *Time-series data* is data containing points associated with the occurrence of events with respect to time. It is generally ingested from IoT devices, self-driving vehicles, sensors, social networks, stock markets, etc.

It is primarily used for running analytics and deducing conclusions. Helping stakeholders make future business decisions by looking at analytic results.

**Why should I choose it?:**

- Need to manage data in real time, continually over a long period of time
- Built to deal with streaming data in real-time
- Need data for analytics

## Wide-Column Database

**What is it?** Belong to NoSQL family and are used to handle massive amounts of data, typically called *Big Data.*

Wide-column databases store data in a record with a dynamic number of columns. A record can hold billions of columns.

**Examples:** Cassandra, HBase, Google Bigtable, etc.

**When to pick?:**

- When we need to work with massive amounts of data
- Ensuring scalability, performance and high availability when managing Big Data

<br>
<br>

# Polyglot Persistence

**What is it?** Polyglot persistence means using several distinct persistence technologies such as *MySQL, MongoDB, Memcache, Cassandra,* etc., together in an application to fulfill its different persistence requirements.

**Example:** A social networking app like Facebook uses polyglot persistence.

![polyglot-persistence](resources/polyglot-persistence.png)

- It uses relational database like MySQL or a graph DB like Neo4J.
    - To store relationships like persisting friends of a user, friends of friends, and pages a user likes.
- Key-value store like Redis or Memcached
    - For low latency access of frequently accessed data, we can implement a cache using key-value store.
    - We can use key-value data store to store user sessions to achieve a consistent state across clusters.
- Wide column database like Cassandra or HBase
    - We can setup an analytics system to analyze big data generated by users.
- ACID transactions and strong consistency will require the use of relational database
    - Lets say businesses now want to runs ads on our portals
    - To enable the business pay for the ads they intent to run we need to implement a payment system.
- Graph DB
    - To enhance a users browser experience, we need to recommend the latest content to keep users engaged.
    - We need to introduce a recommendation system and a graph DB fits best for this use case.
- Document Oriented Store
    - Now our platform contains multiple features like business pages, friends, groups, etc. How do we run a search on these?
    - We need to use a document-oriented datastore like Elasticsearch. This is used for scalable search features.

<br>
<br>

# Multi-model Databases

**What is it?** 

- Databases used to only support one data model; relational, document-oriented, graph, etc.
- Multi-model databases allows us to leverage different data models by just a single DB system.
- It averts the need for polyglot persistence and reduces operational complexity

![multi-model-DB](resources/multi-model-DB.png)

**Examples:** ArangoDB, CosmosDB, OrientDB, Couchbase, etc.

<br>
<br>

# More Database Keywords

## Replicas

- We might want to add Read Replica Databases if we are reading more than writing
- Helps speed up our queries

<br>

## Database Sharding

- If we have lots of data
    - We can use sharding
    - We can shard databases based on location
- **What is it?**
    - Used to split data into smaller chunks or shards
        
        ![db-sharding](resources/db-sharding.png)
        
        - We now have distributed our data to two machines
    - If data is too large
        - Helps to **horizontal scale** the database
    - Helps speed up computations
    - Very complex solution
- **Problems**
    - Joins
        - If we need to run a join, it will need to do a cross shard which could take longer

<br>

## Database Indexing

- There comes a time when database performance is no longer satisfactory.
- First thing you should do is database indexing
    - Goal of what it does is make it faster to search through the table and find the row or rows that we want
    - Indexes can be created using one or more columns of a database table
- ***An index is a data structure that can be perceived as a table of contents that points to the location where actual data lives.***
        ![db-indexing](resources/db-indexing.png)
        
- An index will not have to scan the entire database table look for matches
    - Instead it will restrict its search to a small portion of the rows based on the index that we create
    - **Example:**
        
        ```
        CREATE INDEX person_first_name_idx
        ON person (first_name);
        ```
        
        - This will create an index for the first name column, now all queries will improve if we filter based on first_name
        `SELECT COUNT(*) FROM person WHERE first_name="Paul"`
    - **Multi-Index example**
        
        ```
        CREATE INDEX person_first_name_last_name_idx
        ON person (last_name, first_name);
        ```
        
        - The order matters in Multi-Indices
        - Above, we are sorting the data first by last_name then by first_name
        `SELECT COUNT(*) FROM person WHERE last_name = 'Andrews' AND first_name = 'Julie'`

---

### Example DB Indexing

Imagine we have a database table called **`Orders`** in an e-commerce system. Here's a basic representation of the table:

```markdown
Orders Table
-----------------------------------------------------
| OrderID | CustomerName | Product | OrderDate      |
-----------------------------------------------------
| 1       | Alice        | Laptop  | 2023-01-01     |
| 2       | Bob          | Tablet  | 2023-01-03     |
| 3       | Alice        | Phone   | 2023-01-05     |
| ...     | ...          | ...     | ...            |
```

This table contains thousands of rows. Common queries on this table might involve searching for orders by **`CustomerName`** or **`OrderDate`**.

---

#### Without Indexing:

- A query to find all orders by "Alice" would scan the entire table row by row to find matches. This is inefficient, especially as the table grows larger.

---

#### Creating an Index:

- To optimize this, we can create an index on the **`CustomerName`** column.

---

#### SQL Statement to Create the Index:

```sql
CREATE INDEX idx_customername ON Orders (CustomerName);
```

- This statement creates an index named **`idx_customername`** on the **`Orders`** table specifically on the **`CustomerName`** column.

---

### How the Index Works:

- The database engine sorts the data in the **`CustomerName`** column and creates an index structure (like a sorted list or a tree) that references the actual rows in the **`Orders`** table.
- When you query for orders by "Alice," the database uses the index to quickly locate these rows, instead of scanning the entire table.

---

### The Result:

- Queries that search by **`CustomerName`** become much faster.
- The database engine can jump to the right part of the index and quickly retrieve the corresponding rows from the **`Orders`** table.

---

### Trade-offs:

- The index takes up additional storage space.
- Inserting, updating, or deleting rows in the **`Orders`** table will require updates to the index, which can add overhead.

<br>

## Eventual Consistency

- Imagine at one point a user in Japan likes the tweet and the count went from 100 to 101
- At the same time a user in America clicks on the tweet but sees the like count as 100 instead of 101
- The reason for this is it takes some time to move from Japan to America and update the server nodes running there.
    - Then reaching a consistent state globally
    - If the user refreshes their page, they would see the counter value updated to 101
- This is called ***eventual consistency***, the data was initially consisted but eventually became consistent across all server nodes:

![jpn-eventual-consistency](resources/jpn-eventual-consistency.png)

- Other use cases:
    - Keeping the count of concurrent users watching a live stream
    - Dealing with massive amounts of analytics data

<br>

## Strong Consistency

- Means the data has to be strongly consistent at all times.
- ***All server nodes across the world should contain the same value of an entity***
    - To implement this, we need to lock down the nodes as they are being updated.
- **Scenario:**
    - Lets say a user in Japan likes a post, all the nodes in different geographical regions have to be locked down to prevent any concurrent updates.
    - This means one user at a time can update the tweet like counter.
        
        ![strong-consistency-jpn](resources/strong-consistency-jpn.png)
        
        - Once the user in Japan updates the like counter from 100 to 101.
        - The value gets ***replicated*** globally across all nodes. Once all nodes reaches consensus, the locks get lifted.

<br>
        
## CAP Theorem

**What is it?** Stands for consistency, availability, and partition tolerance (fault tolerance). The system should ideally keep working even when a few nodes go offline. 

***When a few of the system nodes are down, we have to choose between *availability* and *consistency*.***

- Choosing availability
    - When a few nodes are down, the other nodes are available to the users to perform write operations updating the application data.
    - System becomes inconsistent because nodes that are down don’t get updated with new data.
    - When the nodes restore and a user fetches data, it will return old data.
- Choosing consistency
    - We have to lock down all nodes for further writes until the nodes that have gone down come back online.

The design of distributed systems forces us to choose one, we can’t have both at the same time.

- Many NoSQL systems adhere to the CAP theorem, which states that a distributed data system can only simultaneously provide two of the following three guarantees: Consistency, Availability, and Partition Tolerance. **NoSQL databases often prioritize Availability and Partition Tolerance, sometimes at the expense of strong Consistency.**

<br>

## Critique of the CAP Theorem

While the CAP theorem has been influential in distributed systems design, it's important to note that some experts, including Martin Kleppmann (author of "Designing Data-Intensive Applications"), have critiqued its accuracy and usefulness.

Kleppmann argues that the CAP theorem, as commonly understood, is somewhat misleading:

1. **Partition Tolerance is not optional:** In a distributed system, network partitions are a given. They will happen whether we like it or not. Therefore, the choice isn't really between C, A, and P, but rather between C and A in the presence of partitions.

2. **It's not a binary choice:** The theorem is often presented as if you must choose either consistency or availability, but in reality, there's a spectrum of trade-offs between the two.

3. **Consistency and availability are not binary:** There are many types and degrees of consistency and availability, not just "consistent or not" and "available or not".

4. **It doesn't capture other important properties:** The CAP theorem doesn't address other crucial aspects of distributed systems like latency, which can be more relevant in many real-world scenarios.

Kleppmann suggests that instead of thinking in terms of CAP, it's more useful to reason about specific consistency models (like linearizability, causal consistency, etc.) and their trade-offs with availability and latency in the context of particular system designs and use cases.

This perspective encourages a more nuanced approach to distributed system design, moving beyond the simplistic "pick two out of three" model that the CAP theorem might suggest.

