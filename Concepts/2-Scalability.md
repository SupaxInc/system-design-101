# Scalability

**What is it?**

Means the application's ability to handle and withstand increased workload without sacrificing performance.

**Example:**

If an app takes x seconds to respond to a user request. It should take the same x seconds to respond to each of your app's million concurrent users.

*Be able to handle heavy traffic load and maintain system **latency***
<br>

## Latency

**What is it?**

Latency is the time a system takes to respond to a user request. In successful apps, no matter the traffic load the latency should not go up.

**Example:**

In terms of big O notation. Ideally we want something O(1) which is constant time like a map or a key-value database. If something is O(n^2) then it is not scalable because as the size of the data set increases, the slower the program becomes.

---

### Measuring Latency & Solutions

**How does it work?:**

- Latency is measured by the time difference between the action that a user takes and the response to that action. Like clicking a button or scrolling down a web page.
- Divided into two parts:
    
    ![latency](resources/latency.png)
    
    - **Network latency**
        - Time that the network takes to send a data packet from point A to point B.
        - Network should be efficient enough to handle increased traffic load.
            - Solution: Use a CDN to deploy servers across the globe as close to the end-user as possible. AKA *Edge* locations.
    - **Application latency**
        - Time that the application takes to process a user request.
        - Again should also be efficient enough to handle increased traffic load.
            - Solution: Run stress and load tests on the application and scan for bottlenecks that slow down the system as a whole.

**Why is it important?:**

- Latency plays a big role in finding out if an online business wins or loses a customer. This online age makes us very impatient.
    - If something is too slow, the customer will find the information off another website.
- Examples:
    - In an MMO game, a slight lag in-game ruins whole experiences. Reaction times are very important.
    - Algorithmic trading services need to process events in milliseconds.
<br><br>

## Types of Scalability

An application needs solid computing power, servers should be powerful enough to handle increased traffic loads.

---

### Vertical Scaling

**What is it?** It means to add more power to our server.<br>
**Example:** Our app is hosted by a server with 16 gigs of RAM. To handle load, we augment the RAM to 32 gigs. Here we have *vertically* scaled the server.

**How does it work?:**

- We augment the power of the hardware running the app, this is the simplest way to scale as it doesn't require code refactoring or for complex configurations.
- There is a limit to the compute power we can augment for a single server.
    - Why?: Imagine a multi-story building. If we keep adding floors and more people need to live in the building, we can't raise the number of floors to the moon.
    - Solution: Build more buildings, this is where *horizontal* scalability comes in.
- When traffic is too large to be handled by one server, we bring in more servers to work together.
- Significant downside: ***Availability*** risk, making our servers powerful but if we have few in number, there is a risk of that server going down and the entire website going offline. Another reason for *horizontal scalability*.

---

### Horizontal Scaling

**What is it?**

It means to add more servers to the existing hardware resource pool. This increases the computational power of the system as a whole.

**Example:**

![horizontal-scaling](resources/horizontal-scaling.png)

**How does it work?:**

- We can now efficiently handle increased traffic influx.
- No limit to how much we can scale horizontally. We can keep adding servers and setting up data centers.
- Allows for ***high availability***.
- It allows us to scale dynamically in real-time as the traffic in the website climbs.
    - ***Dynamic scaling is not possible when scaling vertically.***

---

### Cloud Elasticity

**What is it?** Most prominent reason why cloud computing is mainstream is the ability to *scale dynamically*. Process of adding, removing, stretching and returning back to original infrastracture on the fly is *cloud elasticity*.

**How does it work?:**

- As traffic climbs, we are able to add additional servers to the *hardware resource pool*, and if it drops, we remove servers.
- Gives us the concept of ***high availability***
    - Multiple server nodes on the back end helps the website stay online even if some server nodes crash
<br><br>

## Scalability Design Approach

### Vertical vs Horizontal Scaling

- We need to find out if we require high availability or low availability.
    - High availability means horizontal scaling
    - Low availability means to vertical scale but have more powerful servers
- Upsides of horizontal scaling is no limit to augmenting hardware capacity, data is replicated across different geographical regions across the globe
- Examples:
    - If your app is a utility or tool and is expected to receive ***minimal predictable traffic***.
        - No need to host it in a distributed environment. A single server is enough to manage traffic so we can go with vertical scaling.
    - If app is a public facing social app, like a social network or fitness app ***where traffic is unpredictable.***
        - High availability in horizontal scaling is important
    - Build these apps to deploy them on cloud and always have horizontal scalability in mind.

---

### Thinking about code on multiple machines, stateless

- Running code in a distributed environment must be ***stateless***.
- No *static instances* in classes.
    - Static instances hold application data when a particular server goes down, all static data/state is lost.
- In OOP, instance variables hold object state.
    - Static variable hold state that spans across multiple objects.
    - Static variable data is not application wide.
- **Problem**: Using static instances can lead to issues in a distributed environment. If a server holding static data goes down, the data is lost, and other servers in the cluster won't have access to this data.
- **Example**: Avoid patterns like Singleton which rely on static instances.
    
    ```java
    public class Singleton {
        private static Singleton instance;
    
        private Singleton() {}
    
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }
    ```
    
- In distributed systems, this pattern fails as each server will have its instance of **`Singleton`**, leading to inconsistent states across the system.
- Solutions:
    - Implement Redis or Memcache to manage a consistent state across the application by storing shared data in these distributed caching systems. This approach ensures that all instances of the application, regardless of which server they are on, can access and modify the same state information in real-time. <br>**Example:** Amazon Elasticache
        - Ensures all application instances access the same state information in real-time
        - Improves application performance by reducing database load
        - Automatically handles node failures and replacements
        - Provides easy scalability to handle increasing workloads
        - Offers monitoring and metrics through Amazon CloudWatch
    - Avoid using static instances in classes to prevent state persistence that is local to a single machine.
    - Consider adopting functional programming principles where functions do not retain state, although this can also be achieved with careful design in OOP.
    - Employ containerization technologies like Docker to ensure consistent environments across multiple machines, facilitating scalability and state management.

---

### Which Scalability is Right?

- Always have an estimate in mind for how much traffic will it have to deal with when designing an app.
- Possibly adopting a ***microservices*** architecture and workloads are meant to be deployed on the cloud. Inherently the workloads are *horizontally scaled* out on the fly
<br><br>

## Primary Bottlenecks of Scalability

### Database

- Scenario:
    - Imagine a well architected application. Workload runs on multiple nodes and can scale horizontally.
    - But the *database* is a poor single ***monolith***. One server has the onus of handling data requests from all of the server nodes.
- The scenario above is a ***bottleneck***
    - The architecture of the nodes are great and handle millions of requests at one point in time efficiently.
    - Yet the response time/latency of the requests are abysmal due to just having *one database*.
- **Solution:** Use database partitioning/sharding with multiple database servers to make the system efficient.

---

### Poor Application Design

- A poorly designed architecture can become a bottle neck.
- A typical architecture mistake is not *using asynchronous processes* and modules wherever required, rather than process are scheduled sequentially.
- Scenario:
    - If a user uploads a document in a portal, tasks such as sending confirmation email or a notification to all subscribers/listeners to the upload event should be done *asynchronously*.
- **Solution:** Tasks like these should be forwarded to a *messaging server* or a *task queue*.

---

### Not Using Caching Wisely

- Caching can be deployed at several layers of the application.
    - Speeds up the response time by notches
    - Intercepts requests before they hit origin servers
- If the system has static data, caching can bring down deployed costs significantly.

---

### Inefficient Config and Setup of Load Balancers

- *Load balancers* are the gateway to the application.
- Using too many or few of them impacts the latency of the application.

---

### Adding Business Logic to Database

- Business logic in the database makes the application components ***tightly coupled***.
    - Make us have a ton of code refactoring when migrating to a different database.
    - Testing gets very complex.

---

### Not Picking the Right Database

- Picking the right database is vital for businesses
- Need transactions and strong consistency?
    - Use relational database
- Don't require strong consistency rather than need *horizontal scalability*
    - Pick NoSQL database

---

### At the Code Level

- Inefficient and poorly written code has the potential to bring down an entire service in production.
    - Writing tighly coupled code
    - Unnecessary loops or nested loops
    - Not paying attention to Big-O complexity
- Do a ***DENTAL*** check of our code when doing a dry run
    - Documentation, Exception handling, Null pointers, Time complexity, Test Coverage