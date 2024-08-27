# Software Architecture

## Architecture Tiers

**What is a tier?** Logical and physical separation of components in an application or service. This separation is at a **component** level.

**What is a component?** Database, back-end server, user interface, messaging, caching, etc.

![web-app-architecture](../resources/web-app-architecture.png)

 

### Single Tier

**What is it?** In a single-tier application, the user interface, back-end logic, and database **reside in the same machine**.

**Examples:** MS Office, PC Games, image editing like Gimp and Photoshop

![single-tier](../resources/single-tier.png)

**Pros:**

1. No network latency, every **component** is located on the same machine. This makes data readily available.
    1. Other n-tier architectures have to send data requests to the back-end server often. This adds up network latency, making it slower.
2. Actual performance depends on the application's hardware requirements and how powerful the machine it runs on.
3. **Data privacy** and **safety** is of the **highest order** since data always stays in their machine and does not need to be transmitted over a network for **persistence of data**.

**Cons**:

1. Big downside is application's publisher has no control over the application. For patches/updates, customer must manually update it by connecting to the server.
    1. Ex. In the 90's, when a game was shipped with buggy code, studios could do nothing to fix it.
2. Code is very vulnerable and can be changed/reverse engineered. Someone can profit of it.

### Two Tier

**What is it?** Involves a client and a server. The **client contains the user interface *with business logic on one machine***. While the back-end server includes the database running on a different machine hosted by a business. This is also known as **client server architecture**.

This service largely depends on business requirements and use case. Moving the **business logic to a dedicated back-end server** turns it into a three tier application.

**Examples:** Web/mobile apps (to-do list, planner, productivity), web/mobile games, etc.

![two-tier](../resources/two-tier.png)

**Pros**

- The code is vulnerable but if it is accessed it won't cause the business harm
- **Business logic and UI resides in the same machine** so fewer network calls to the back-end server, keeping latency low.
    - **Examples:** To do-lists, the app only makes a call to the database server only when the user has finished creating their list and wants to **persist** the data.
    - Browser/mobile games, files are downloaded once the user uses the app for the first time but only make network calls to **persist game state**.
- Fewer server calls means less money spent to keep servers running.

### Three Tier

**What is it?** The user interface, business logic and database **all reside on different machines** thus have different tiers.

**Examples:** Largely used in the web (blogs, news websites, etc)

![three-tier](../resources/three-tier.png)

**Pros:**

- A three tier architecture works best for simple use cases
- A simple blog for example:
    - The user interface (**client**) will be written using HTML, Javascript, CSS
    - Back-end logic (**server**) will run on a server like Apache
    - **Database** will be MySQL

### N Tier

**What is it?** An application has more than three components (UI, back-end server, database)+

**These other components could be:** Cache, message queue (asynchronous behaviour), load balancers, search servers (searching massive amount of data), components involved in processing massive amounts of data, heterogeneous tech (web services, micro-services)

**Examples:**

Instagram, Facebook, Tiktok, Uber, Airbnb, Roblox, etc.

It is important for the two following software design principles:

#### Single Responsibility Principle

- Means giving one dedicated responsibility to a client and letting it execute flawlessly.
    - Saving data, running app logic, or ensuring delivery of the messages throughout system
- This gives a lot of flexibility making management easy. Can have dedicated teams and code repositories for individual components keeping things cleaner.
- Making a change to one component does not impact functionality in others
    - Ex. Upgrading a database server with a new operating system won't affect other components, if something happens with updates only the database will go down.
- Prevents us from using *stored procedures* in databases.
    - Database should not hold business logic, only used for **persisting data**
    - Should just have a separate tier to handle the business logic another way.

#### Separation of Concerns

- Allows members to only be concerned about their own work.
- Keeping components separate makes them reusable.
    - Different services can use the same database, messaging server, or any other component as long as they are not *tightly coupled*.
    - *Loosely coupled* components allows us to scale our service easily.

## Differences Between Layers and Tiers

- **Layers** are UI layer, business, service and data access layers
    
    ![layers](../resources/layers.png)
    
    - It represents conceptual organization of code
- **Tiers** are UI client, servers, database, messaging queues, load balancers, search servers, etc.
    - It represents the physical separation of components

<br><br>


# Web Architecture

**What is it?**

Involves multiple components like database, message queue, cache, UI client, etc. All running in conjunction to form an online service.

**Example:**

The picture below dictates a web application architecture.

![web-app-architecture](../resources/web-app-architecture2.png)

## Client-Server Architecture

**What is it?** The client-server architecture is the one tier, two tier, three tier, and n tier architecture.

**How does it work?:**

- It works in a request-response model
    - *Client* sends a request to the server for information and the *Server* responds to it.
- Differs from a peer-to-peer architecture

### Client

**What is it?** Holds the user interface, it is the presentation part of the application written in HTML, Javascript, CSS.

**How does it work?:**

- UI runs on the client, it is the gateway of our application.
- Can be a mobile app, website, desktop, or tablet.
- It could use a variety of different technologies and frameworks including Javascript, jQuery, React, Angular, Vue, Svelte, etc.

**Different type of clients:** Thin and thick clients

---

#### Thin

**What is it?** A thin client just holds the user interface of the application and contains **no business logic**. For every action, the client sends a request to the backend server where the business logic is. Similarly to a **three-tier** application.

**Example:**

The picture below dictates how it works (three tier application), back-end logic resides in back-end server which connects to database.

![thin-client](../resources/thin-client.png)

#### Thick

**What is it?** Thick client holds all or some part of the business logic. Similarly to a **two-tier** application.

**Example:**

Online games, utility apps like to do, etc.

![thick-client](../resources/thick-client.png)

### Server

**What is it?** Receives requests from the client and provides a response after executing the business logic based on request parameters received from the client.

**How does it work?:**

- Every online service needs a server to run, aka application servers.
- Besides application servers, there are other kind of servers that run specific tasks
    - Includes: proxy, mail, file, virtual data storage, batch job servers.
- Server configuration can differ depending on the use case
    - **Example:** If we run a back-end application code written in Javas, we would pick *Apache Tomcat* or *Jerry*
    - For a hosting website, we would pick an *Apache HTTP Server*
- **Server-side rendering**, often devs use a server to render the user interface on the backend, then send the generated data to the client
    - **Example:** In an online game like VALORANT, if we buy a gun skin, it will show that gun skin to everyone in the game since the data has been persisted on the database.
- **Client-side rendering**, for one-tier or two tier applications, we may be rendering the user interface on the client
    - **Example:** In Runescape, users are able to use popular Cheat Engine to change the code of the business logic within the user interface to give themselves more money. However, it will only show on their client and not other clients since the data has not been persisted.
<br>

## Client-Server Communication

**What is it?** It uses the request-response model, the *client* sends the request and the *server* responds. If there is no request then there is no response.

**How does it work?:** It works using the following:

- HTTP Protocols
    - Entire communication happens over HTTP protocol (request-response protocol), used for data exchange over the world wide web.
    - Defines how the information is transmitted over the web.
- REST API and API Endpoints
    - Modern n-tier web applications, every *client* has to hit a REST endpoint to fetch the data from the backend.
    - Backend application code has a REST API implemented.
    - **Example:**
        
        ![rest-api](../resources/rest-api.png)
        
        - Objective:
            - Let's say we want to write an application to keep track of birthdays to all Facebook friends and send a reminder beforehand.
        - Implementation:
            - First step is to get ALL of the birthdays of our friends
                - Write a client to hit the *Facebook Social Graph API* which is a REST API to get the data then run business logic on the data.

### REST API

**What is it?** Stands for *Representational State Transfer*. It is an architectural style for implementing web services.
https://www.youtube.com/watch?v=_YlYuNMTCc8

**How does it work?:**

- It acts as an interface
- Takes advantage of HTTP methodologies to establish communication between the client and the server.
- Allows servers to cache the response to improve application performance.
- It is a **stateless process**, meaning communication between client and server is a *new* one.
    - No information or memory is carried over from previous communications.
    - ***So everytime the client interacts with the server, the authentication must also be sent with it.***

#### Rest Endpoints

**What is it?** Means URL of the service that the client could hit: `https://myservice.com/users/{username}` for fetching user details of a particular user.

1. **GET** (Retrieve a list of books or a specific book): **THERE IS ALSO PATH/QUERY STRINGS SEE BELOW**
    
    ```jsx
    // Get all books
    app.get('/api/books', (req, res) => {
        // Logic to fetch and return all books
    });
    
    // Get a specific book by ID
    app.get('/api/books/:id', (req, res) => {
        // Logic to fetch a book by req.params.id
    });
    ```
    
2. **POST** (Create a new book):
    
    ```jsx
    app.post('/api/books', (req, res) => {
        // Logic to create a new book from req.body
    });
    ```
    
3. **PUT** (Update a specific book):
    
    ```jsx
    app.put('/api/books/:id', (req, res) => {
        // Logic to update an existing book using req.params.id and req.body
    });
    ```
    
4. **DELETE** (Delete a specific book):
    
    ```jsx
    app.delete('/api/books/:id', (req, res) => {
        // Logic to delete a book using req.params.id
    });
    ```
    
5. **PATCH** (Partially update a specific book):
    
    ```jsx
    app.patch('/api/books/:id', (req, res) => {
        // Logic to partially update a book using req.params.id and req.body
    });
    ```
    
6. **OPTIONS** (Discover communication options for the API):
    
    ```jsx
    app.options('/api/books', (req, res) => {
        // Logic to return the supported HTTP methods for the /api/books endpoint
        res.header('Allow', 'GET, POST, PUT, DELETE, PATCH, OPTIONS').send();
    });
    ```
    
7. **HEAD** (Retrieve headers for a specific book resource):
    
    ```jsx
    app.head('/api/books/:id', (req, res) => {
        // Logic to fetch headers for a book using req.params.id
        // The body of the response should be empty
    });
    ```
    

**Front End code**

1. **Front-End Code for Path Parameter Example with Axios (GET a specific book):**
    
    Fetching a specific book by its ID using a path parameter:
    
    ```html
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
        function getBookById(bookId) {
            axios.get(`/api/books/${bookId}`)
                .then(response => {
                    console.log('Book:', response.data);
                    // Handle the book data (e.g., display it on the page)
                })
                .catch(error => console.error('Error:', error));
        }
    
        getBookById('1'); // Example: Fetch the book with ID '1'
    </script>
    
    ```
    
2. **Front-End Code for Query String Example with Axios (GET books with optional filters):**
    
    Fetching books with optional filters like author and year using query strings:
    
    ```html
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
        function getBooksByFilter(author, year) {
            const params = {};
            if (author) params.author = author;
            if (year) params.year = year;
    
            axios.get('/api/books', { params })
                .then(response => {
                    console.log('Filtered Books:', response.data);
                    // Handle the filtered books data (e.g., display it on the page)
                })
                .catch(error => console.error('Error:', error));
        }
    
        getBooksByFilter('Author A', 2001); // Example: Fetch books by 'Author A' published in 2001
    </script>
    
    ```
    

**Back End code**

1. **Path Parameters**: These are used to identify a specific resource. They are part of the URL path.
    
    ```jsx
    // Example: GET /api/books/:bookId
    app.get('/api/books/:bookId', (req, res) => {
        const bookId = req.params.bookId;
        // Logic to retrieve the book with the given bookId
    });
    ```
    
    In this case, **`:bookId`** is a path parameter that would be replaced with the actual ID of the book you want to retrieve.
    
2. **Query Strings**: These are used for filtering, searching, or detailed specification of the request, usually in GET requests.
    
    ```jsx
    // Example: GET /api/books?author=JKRowling&published=1997
    app.get('/api/books', (req, res) => {
        const author = req.query.author;
        const publishedYear = req.query.published;
        // Logic to retrieve books based on author and published year
    });
    ```
    
    Here, **`author`** and **`published`** are query parameters that can be used to filter the books based on the author's name and the year they were published.
    

#### Decouples Clients & Backend Service

**What is it?** Back-end service does not have to worry about client implementation. It just tells clients that "*Here is a URL address of the resource/information you need. Hit it when you need it. Any client with the required authorization can access it.*"

Developers can have different implementations for different clients, allowing to leverage different technologies. This means clients and backend service are ***decoupled***.