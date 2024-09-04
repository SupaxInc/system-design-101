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