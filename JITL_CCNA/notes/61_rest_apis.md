# REST APIs

## Part 1

### CRUD

- CRUD refers to the operations we perform using REST APIs
- **Create**
  - `Create` operations are used to create new variables and set their initial values
    - Create variable `ip_address` and set the value to `10.1.1.1`
- **Read**
  - `Read` operations are used to retrieve the value of a variable
    - What is the value of variable `ip_address`?
- **Update**
  - `Update` operations are used to change the value of a variable
    - Change the value of variable `ip_address` to `10.2.3.4`
- **Delete**
  - `Delete` operations are used to delete variables
    - Delete variable `ip_address`
- HTTP uses *verbs* (aka methods) that to these CRUD operations
- REST APIs typical use HTTP/S

| Purpose                         | CRUD Operation | HTTP Verb      |
|:-------------------------------:|:--------------:|:--------------:|
| Create new variable             | **Create**     | **POST**       |
| Retrieve value of variable      | **Read**       | **GET**        |
| Change the value<br>of variable | **Update**     | **PUT, PATCH** |
| Delete variable                 | **Delete**     | **DELETE**     |

### HTTP

![HTTP Header](./images/http_header.png)

- The **HTTP Request** can include additional headers which pass additional information to the server
  - An example would be an **Accept** header, which informs the server about the type(s) of data that can be sent back to the client
    - **Accept: application/json**
    - **Accept: application/xml**
  - When a REST client makes an API call (request) to a REST server, it will send an HTTP request
    - REST APIs don't *have* to use HTTP for communication, although HTTP is the most common choice
- The **HTTP Response** will come from the server and include a status code indicating if it succeeded or failed, as well as other details
  - The first digit indicates the class of the response
    - **1xx** *informational* - the request was received, continuing process
      - **102 Processing** indicates that the server has received the request and is processing it, but the response is not yet available
    - **2xx** *successful* - the request was successfully received, understood and accepted
      - **200 OK** indicates that the request succeeded
      - **201 Created** indicates that the request succeeded and a new resource was created (ie in response to POST)
    - **3xx** *redirection* - further action needs to be taken in order to complete the request
      - **301 Moved Permanently** indicates that the requested resource has been moved, and the server indicates its new location
    - **4xx** *client error* - the request contains bad syntax or cannot be fulfilled
      - **401 Unauthorized** means the client must authenticate to get a response
      - **404 Not Found** means the requested resource was not found
    - **5xx** *server error* - the server failed to fulfill an apparently valid request
      - **500 Internal Server Error** means the server encountered something unexpected that it doesn't know how to handle

### REST

- REST stands for Representational State Transfer
- **REST APIs** are also known as **REST-based APIs** or **RESTful APIs**
  - REST isn't a specific API
  - Instead, it describes a set of rules about how the API should work
- The six constraints of RESTful architecture are:
  - Uniform Interface
  - Client-server
  - Stateless
  - Cacheable or non-cacheable
  - Layered system
  - Code-on-demand (optional)

#### REST: Client-Server

- **REST APIs** use a client-server architecture
- The client uses API calls (HTTP requests) to access the resources on the server
- The separation between the client and server means they can both change and evolve independently of each other
  - When the client application changes or the server application changes, the interfaces between them must not break

#### REST: Stateless

- **REST API** exchanges are stateless
- This means that each API exchange is an independent even of all past exchanges between the client and server
  - The server does not store information about previous requests from the client to determine how it should response to new requests
  - If authentication is required, the client must authenticate with the server for each request it makes
- TCP is an example of a stateful protocol
  - A connection is established
  - Each packet has a sequence number
- UDP is an example of stateless
  - Date is sent
- *Although REST APIs use HTTP/S, which uses TCP (stateful) as its Layer 4 protocol, HTTP and REST APIs themselves aren't stateful. The functions of each layer are separate!

#### REST: Cacheable or Non-Cacheable

- **REST APIs** must support caching of data
- *Caching* refers to storing data for future use
  - This improves performance for the client and reduces the load of the server
- Not all resources have to be cacheable, but cacheable resources MUST be declared as cacheable

## Part 2 - REST API Authentication
