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

- Similar to the cache aside strategy. Cache in a read-through strategy always automatically stays consistent with the database.
- The cache library, or the framework, takes the onus of maintaining consistency with the database. On the contrary, in the cache aside strategy, we have to write explicit logic to update the cache.
- Application data in this strategy is also lazy-loaded in the cache only when the user requests it.
- Also, the data model of the cache has to be consistent with the database since it is updated automatically by the library.

## Write-through

- Cache sits in line with the database. Every write goes through the cache before updating the database.
- This maintains high data consistency between the cache and the database.
    - Adds a little latency during the write operations since the data has to be additionally written to the cache.
- This caching strategy works well for use cases where we need **strict data consistency** between the cache and the database. It is generally used with other caching strategies to achieve optimized performance.

## Write-back

- Helps optimize application hosting costs significantly.
- Data is directly written to the cache instead of the database, and the cache, after some delay, as per the business logic, writes data to the database.
    - Could be a ***batch operation*** that happens once in awhile to update the DB
- This is what I pulled off in my stock market game. If there are quite a number of writes in the application, developers can reduce the frequency of database writes to cut down the load on it and the associated write operation costs.
- A risk in this approach is if the cache fails before the DB is updated, the data might get lost. Again, this strategy is clubbed with other caching strategies to get the best of all the worlds.