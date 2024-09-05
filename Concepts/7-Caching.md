# Introduction

## What is it?

- Caching is key to the performance of any kind of application. It ensures low latency and high throughput.
- Intercepting the database requests allows the database to free its resources to work with other requests.
- A cache can always handle more read requests than a DB since it stores the data in a key-value pair and does not have relational logic.
- Frequently requested data queried from the database with the help of serval table joins can be cached to avert the same joins query to be run every time the same data is required
    - This increases throughput
- **Caching Dynamic Data**
    - Dynamic data changes more often and has a TTL, when TTL ends, the data is purged from the cache and newly updated data is stored. Also known as ***cache invalidation***.
    - The data TTL should be long enough to make effective use of caching
        - Caching wont help if data changes too often, e.g, the price of a stock changes too fast
- **Caching Static Data**
    - Static data can consist of fonts, images, CSS and similar files
    - This is the kind of data that doesn’t change often and can be easily cached

<br>

# Do you need a Cache?

- It is always a good idea to use cache as opposed to not using it, especially if we have static data in our application.
- It can be used in any layer of the application.
    - The most common usage is database caching to help alleviate the stress on the DB by intercepting requests
- Look for patterns
    - Maybe caching frequently accessed content on our website
- Think of joins in relational databases
    - A cache can avert the need for running joins every time by storing data in demand
- Even if DB goes down for awhile, the cache can be used to continue to serve data requests

<br>

# Examples

- Stock market-based multiplayer game
    - In this lesson, I will share an insight from a stock market-based multiplayer game that I developed and deployed on the cloud.
    - The game had stocks of numerous companies listed on the stock market and the algorithm would trigger the stocks’ price movement based on certain parameters every second, if not before.
    - Initially, I persisted the updated stock price in the database as soon as the prices changed to create a stock price movement timeline at the end of the day. However, the number of database writes for the stocks price movement for the whole day was very high, having the potential to create a crater in my pocket.
    - Eventually, I decided not to persist the updated price every second in the database and rather use a cache (Memcache) to persist the updated stock prices. ***I then scheduled a batch operation that would run throughout the day every few hours to update the database with the stock prices.***
    - In the cloud, writing to Memcache was cheaper than writing to the database by quite an extent. The cache served all the stock price read-write requests, and the database got the updated values when the batch operation ran.
    - This tweak may not be ideal for a real-life Fintech app. However, it helped me save a truckload of money and allowed me to run the game for a more extended period of time.
    - So, this is one instance where you can leverage the caching mechanism to cut down costs. There are use cases where you might not want to persist every little information in the database and instead use the cache to store that not-so-mission-critical information.
- Polyhaven - 3d asset library
    
    https://scaleyourapp.com/application-hosting-how-polyhaven-manages-5-million-page-views-and-80tb-traffic-a-month-for-400/
    

# Caching Strategies

## Cache-aside

- Most commonly used caching strategy
- The cache works along with the database intending to reduce the hits on it as much as possible.
- The data is ***lazy-loaded*** in the cache. When the user sends a request for particular data, the system first looks for it in the cache. If present, it is simply returned. This is called a ***cache hit***.
    - If not, this is called a ***cache miss***. In this case, the application fetches the data from the database and returns it to the user, *also updating the cache for future requests*.
- When it comes to data write, it is directly written to the database. Now, this could cause data inconsistency between the cache and the database. To avoid this, the data on the cache has a TTL (Time to live). After it expires, the data is invalidated from the cache and is newly updated with new data AKA ***cache invalidation***.
    - See ***dynamic caching*** in the introduction above
- **This caching strategy works best with read-heavy workloads.**
- The data that does not get updated too frequently, like customer data (name, account number, etc.) is cached using the cache aside strategy. We can assign a long TTL to this kind of data.

## Read-through

- Read-through caching is a strategy where the cache sits between the application and the database.
- When a read request comes in:
  1. The application first checks the cache for the data.
  2. If the data is in the cache (cache hit), it's returned immediately.
  3. If the data is not in the cache (cache miss), the caching system automatically fetches it from the database, stores it in the cache, and then returns it to the application.
- Key differences from cache-aside:
  - The application doesn't need to know about the underlying data source. It always reads from the cache.
  - The cache itself manages data retrieval from the database, not the application code.
- Advantages:
  - Simplifies application code as cache management is abstracted away.
  - Ensures that the cache is always consistent with the database for read operations.
- Disadvantages:
  - Initial reads for uncached data might be slower due to the extra step of caching.
  - Not suitable for data that changes very frequently, as the cache might become outdated quickly.
- Like cache-aside, read-through also uses lazy loading, populating the cache only when data is requested.
- Often used in combination with write-through or write-behind strategies to handle data updates.

## Write-through

- In write-through caching, the cache sits between the application and the database.
- When data is written:
  1. The application writes the data to the cache.
  2. The cache immediately writes the data to the database.
  3. The write operation is considered complete only after both the cache and database are updated.
- Advantages:
  - Ensures strong consistency between the cache and the database.
  - Reduces the risk of data loss in case of cache failures.
- Disadvantages:
  - Slightly higher latency for write operations due to the two-step write process.
  - May increase the load on the database, as every write operation hits the database.
- Best used in scenarios where data consistency is critical and write operations are not too frequent.

## Write-back (also known as Write-behind)

- In write-back caching, the cache again sits between the application and the database, but handles writes differently.
- When data is written:
  1. The application writes the data only to the cache.
  2. The write operation is immediately considered complete.
  3. The cache asynchronously writes the data to the database after a delay or when certain conditions are met.
- Advantages:
  - Significantly reduces write latency as seen by the application.
  - Can greatly reduce database load by batching multiple writes together.
  - Excellent for write-heavy workloads or when there are frequent updates to the same data.
- Disadvantages:
  - Risk of data loss if the cache fails before writing to the database.
  - Potential consistency issues between cache and database during the delay.
- Implementation variations:
  - Time-based: Cache writes to database after a set interval.
  - Size-based: Cache writes to database when it accumulates a certain amount of changes.
  - Combination: Using both time and size thresholds.
- Often used in scenarios where performance is critical and some level of data inconsistency can be tolerated.

Both strategies can be combined with read-through or cache-aside strategies for a comprehensive caching solution tailored to specific application needs.