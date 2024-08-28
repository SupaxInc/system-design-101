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