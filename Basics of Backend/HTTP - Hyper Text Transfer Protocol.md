----

## Scope of Backend Topics

Backend engineering is massive. If we try to cover **every possible component**, we’ll be stuck for years.  
So instead, the focus is on the **~90% of concepts that appear in most backend codebases**.

The first and most fundamental of these is **HTTP** — the medium through which browsers and servers communicate.

---

## HTTP Protocol — The Medium of Communication

HTTP is the primary protocol used by browsers and servers to:

- Send data (requests)
    
- Receive data (responses)
    

Although other protocols exist, HTTP is the most widely used, so it becomes the foundation for backend understanding.

At the heart of HTTP are **two core ideas**:

1. **Statelessness**
    
2. **Client–Server Model**
    

---

## Statelessness in HTTP

HTTP is a **stateless protocol**, meaning the server has **no memory** of past requests.

Each request is:

- Independent
    
- Self-contained
    
- Carries all information needed to process it
    

This includes:

- URL
    
- Method
    
- Headers
    
- Authentication tokens or cookies
    

After sending a response, the server **forgets everything** about that interaction.

### Example

When accessing a user profile:

- The client must send authentication data **every time**
    
- The server does not “remember” previous requests
    

---

## Why Statelessness Is Powerful

**Simplicity**  
The server does not need to manage session memory, reducing complexity.

**Scalability**  
Requests can be distributed across multiple servers since no single server owns session state.

**Fault tolerance**  
If a server crashes, no session state needs to be restored.

Because HTTP is stateless, developers reintroduce “state” using:

- Cookies
    
- Sessions
    
- Tokens (JWT, OAuth, etc.)
    

These are application-level solutions, not protocol-level features.

---

## Client–Server Model

HTTP follows a strict **client–server architecture**.

- **Client**
    
    - Browser or application
        
    - Always initiates the request
        
    - Provides all required request information
        
- **Server**
    
    - Hosts resources (websites, APIs)
        
    - Processes requests
        
    - Sends responses (HTML, JSON, errors, files, etc.)
        

A key rule:

> The server never initiates communication.  
> The client always starts the interaction.

---

## HTTP vs HTTPS (High-Level)

HTTPS is essentially **HTTP + security**:

- Encryption
    
- TLS certificates
    
- Secure transport
    

For backend concepts, HTTP and HTTPS are treated as interchangeable because the **core principles remain the same**.

---

## Transport Layer: Where HTTP Lives

HTTP needs a **reliable transport mechanism** to send messages.

- HTTP commonly uses **TCP**
    
- TCP is:
    
    - Connection-based
        
    - Reliable
        
    - Error-aware
        

Although HTTP does not strictly require TCP, TCP satisfies its reliability needs better than UDP (historically).

This is part of the **OSI model**:


![[Pasted image 20260205173203.png]]



- Backend engineers mostly operate at **Layer 7 (Application Layer)**
    
- Lower layers (TCP handshakes, TLS) are useful to know but are not the main focus
    

---

## Evolution of HTTP

Over time, HTTP evolved to improve performance.

### HTTP/1.0

- One connection per request
    
- Very inefficient
    

### HTTP/1.1

- Persistent connections
    
- Multiple requests over one TCP connection
    
- Chunked transfers
    
- Better caching  
    _(Still the most widely used today)_
    

### HTTP/2

- Multiplexing
    
- Binary framing
    
- Header compression
    
- Server push
    
- Still suffers from head-of-line blocking (TCP-level)
    

### HTTP/3

- Built on **QUIC over UDP**
    
- Faster connection setup
    
- Reduced latency
    
- No head-of-line blocking
    


![[Pasted image 20260205173250.png]]


For backend engineers:

> All you need to remember is that **clients and servers exchange messages over a network connection**.

---

## HTTP Messages: Requests and Responses

HTTP communication happens via **messages**.

- **Request message** → sent by client
    
- **Response message** → sent by server
    

Each message has:

1. Start line
    
2. Headers
    
3. Blank line
    
4. Body (optional)


![[Pasted image 20260205173349.png]]

---

## HTTP Request Structure (High Level)

A request includes:

- **Method** (GET, POST, etc.)
    
- **Resource URL**
    
- **HTTP version**
    
- **Headers**
    
- **Body** (optional)
    

Headers end at a blank line, after which the body begins.

---

## HTTP Response Structure (High Level)

A response includes:

- **HTTP version**
    
- **Status code** (e.g., 200 OK)
    
- **Headers**
    
- **Body**
    

---

## Why HTTP Headers Exist

Headers are **key–value metadata** sent with requests and responses.

Why not put everything in the URL or body?

### Real-world analogy: Parcel delivery

- Address and metadata are written **outside the package**
    
- Delivery systems need metadata without opening the parcel
    

Similarly:

- Headers allow systems (servers, proxies, browsers) to inspect metadata **without touching the body**
    

---

## Categories of HTTP Headers

### Request Headers

Sent by the client to describe the request.

Examples:

- `User-Agent`
    
- `Authorization`
    
- `Accept`
    
- `Cookie`
  

They help the server understand:

- Client type
    
- Capabilities
    
- Expectations
    

---

### General Headers

Used in both requests and responses.

Examples:

- `Date`
    
- `Cache-Control`
    
- `Connection`
    

They describe the **message itself (metadata)**, not the content.

---

### Representation Headers

Describe the **body** of the message.

Examples:

- `Content-Type`
    
- `Content-Length`
    
- `Content-Encoding`
    
- `ETag`
    

They tell clients and servers **how to interpret the data**.

---

### Security Headers

Control browser behavior to prevent attacks.

Examples:

- `Strict-Transport-Security`
    
- `Content-Security-Policy`
    
- `X-Frame-Options`
    
- `X-Content-Type-Options`
    
- Secure & HttpOnly cookies
    

They protect against:

- XSS
    
- Clickjacking
    
- MIME sniffing
    
- Protocol downgrade attacks
    

---

## Two Big Ideas Behind Headers

### 1. Extensibility

HTTP can evolve **without changing the protocol**.

- Custom headers
    
- New security features
    
- Content negotiation
    

### 2. Remote Control

Headers act like **instructions** from client to server.

They influence:

- Response format
    
- Caching
    
- Authentication
    
- Access control
    

---

## HTTP Methods and Intent

HTTP methods define **intent**, not just action.

Common methods:

- **GET** → Fetch data (no modification)
    
- **POST** → Create data
    
- **PATCH** → Partial update
    
- **PUT** → Full replacement
    
- **DELETE** → Remove resource
    

Rule of thumb:

> Use **PATCH** unless you explicitly need full replacement.

---

## Idempotent vs Non-Idempotent Methods

**Idempotent** → Same result no matter how many times called:

- GET
    
- PUT
    
- DELETE
    

**Non-idempotent** → Produces different results:

- POST
    

Example:

- Creating a note with POST multiple times → multiple notes
    

---

## OPTIONS Method and CORS

The **OPTIONS** method is mostly used by browsers for **CORS preflight checks**.

CORS exists because browsers enforce the **Same-Origin Policy**:

- A webpage cannot access resources from another origin by default
    

---

## Simple CORS Request Flow

Conditions:

- Simple method (GET, POST, HEAD)
    
- Simple headers
    
- Simple content types
    

Flow:

1. Browser sends request with `Origin`
    
2. Server responds with `Access-Control-Allow-Origin`
    
3. Browser checks header
    
4. Response allowed or blocked



![[Pasted image 20260205235917.png]]

Simple Request in action. Software used is **Burp Suite**.

![[Pasted image 20260206000351.png]]






---

## Preflight CORS Request Flow

Triggered when:

- Method is PUT or DELETE
    
- Non-simple headers (e.g., Authorization)
    
- JSON content type
    

Flow:

1. Browser sends **OPTIONS** request
    
2. Server responds with allowed:
    
    - Origins
        
    - Methods
        
    - Headers
        
3. Browser validates response
    
4. Browser sends actual request
    

Preflight responses often use:

- **204 No Content**
    
- `Access-Control-Max-Age` to cache permissions
    


---

**OPTIONS RESPONSE**


![[Pasted image 20260206000951.png]]
---


-  **Green** -> Different Origin (SOP kicks in).

-  **Pink** -> Options request always returned with **204 - No content** (It is an inquiry request and no data is sent).

-  **Blue** -> Server returns all permissions .


**PRE-FLIGHT RESPONSE**

![[Pasted image 20260206001707.png]]

-  **Green** -> PUT method is not simple.
-  **BLUE** -> Authorization header is also an alert.
-  **PINK**  -> Content type is also JSON and not plain.

*NOTE : Any of the 3 conditions constitutes for a pre-flight request.*
## Key Takeaway

CORS is:

- A **browser-enforced security feature**
    
- Controlled via **HTTP headers**
    
- Not a server-side restriction
    

Understanding CORS from first principles prevents:

- Trial-and-error debugging
    
- Misconfigured APIs
    
- Confusing frontend–backend blame loops
    


---

# HTTP Deep Dive (From CORS → Status Codes → Caching → Streaming → TLS)

These notes cover **how browsers and servers actually behave**, not just definitions.

---

## 1. CORS Preflight Flow (OPTIONS → Actual Request)

### Why a Preflight Request Happens

A request is **NOT “simple”** if **any one** of these is true:

- Method is **not** `GET`, `POST`, or `HEAD`
    
- Uses **non-simple headers** (e.g. `Authorization`)
    
- `Content-Type` is **application/json**
    
- Sends a JSON request body
    

👉 Even **one condition** triggers a preflight  
👉 In your case, **all three** were present

---

### Preflight Request Characteristics

- Method: **OPTIONS**
    
- No request body
    
- No response body
    
- Purpose: **Ask permission**
    

The browser is asking the server:

> “Are these methods and headers allowed from this origin?”

---

### Server Preflight Response (204 No Content)

Key headers returned:

- `Access-Control-Allow-Origin`
    
    - Must match frontend domain/port
        
- `Access-Control-Allow-Methods`
    
    - e.g. `GET, POST, PUT, DELETE`
        
- `Access-Control-Allow-Headers`
    
    - Must include requested headers (`Authorization`, `Content-Type`)
        
- `Access-Control-Max-Age`
    
    - How long the browser can cache this permission
        

`204 No Content` is used because:

- Preflight is a **general inquiry**
    
- No data exchange happens
    

---

### After Successful Preflight

1. Browser validates response headers
    
2. Browser sends the **actual request**
    
3. Server processes it normally
    
4. Response body is returned
    

👉 **Preflight + Actual Request = Complete CORS flow**

---

## 2. Why HTTP Status Codes Exist

Status codes allow clients to:

- Understand outcome **without parsing body**
    
- Make decisions programmatically
    
- Handle errors consistently
    

They are:

- **Standardized**
    
- **Language-agnostic**
    
- **Framework-independent**
    

Without them, clients would have to guess intent from response bodies.

---

## 3. Status Code Categories (High-Level)

|Range|Meaning|
|---|---|
|1xx|Informational|
|2xx|Success|
|3xx|Redirection|
|4xx|Client errors|
|5xx|Server errors|

---

## 4. Most Important Status Codes (95% Use Cases)

### ✅ 2xx – Success

- **200 OK**
    
    - Request successful
        
    - Response body present
        
- **201 Created**
    
    - New resource created
        
    - Usually after POST
        
- **204 No Content**
    
    - Successful, but no body
        
    - Used for:
        
        - Preflight OPTIONS
            
        - DELETE requests
            

---

### 🔁 3xx – Redirection

- **301 Moved Permanently**
    
    - Resource permanently moved
        
    - Clients should update URLs
        
- **302 Found (Temporary)**
    
    - Temporary redirect
        
    - Clients keep original URL
        
- **304 Not Modified**
    
    - Cached resource still valid
        
    - Used with `ETag` / `If-Modified-Since`
        

---

### ❌ 4xx – Client Errors (Backend Engineers see these most)

- **400 Bad Request**
    
    - Invalid or illogical input
        
- **401 Unauthorized**
    
    - Missing / expired auth credentials
        
- **403 Forbidden**
    
    - Authenticated, but no permission
        
- **404 Not Found**
    
    - Resource doesn’t exist
        
- **405 Method Not Allowed**
    
    - Wrong HTTP method
        
- **409 Conflict**
    
    - Resource conflict (e.g. duplicate)
        
- **429 Too Many Requests**
    
    - Rate limiting
        

---

### 💥 5xx – Server Errors

- **500 Internal Server Error**
    
    - Unexpected server failure
        
- **501 Not Implemented**
    
    - Feature not supported yet
        
- **502 Bad Gateway**
    
    - Invalid response from upstream
        
- **503 Service Unavailable**
    
    - Server overloaded / maintenance
        
- **504 Gateway Timeout**
    
    - Upstream server didn’t respond in time
        

---

## 5. HTTP Caching (Conditional Requests)

### Goal of Caching

- Reduce bandwidth
    
- Improve load time
    
- Reduce server load
    

---

### Key Response Headers

- `Cache-Control`
    
    - e.g. `max-age=10`
        
- `ETag`
    
    - Hash of response content
        
- `Last-Modified`
    
    - Timestamp of last update
        

---

### First Request (200 OK)

- Server sends:
    
    - Response body
        
    - `ETag`
        
    - `Last-Modified`
        

Browser caches the response.

---

### Subsequent Request (Conditional GET)

Browser sends:

- `If-None-Match` (ETag)
    
- `If-Modified-Since`
    

Server checks:

- If unchanged → **304 Not Modified**
    
- If changed → **200 OK + new data**


![[Pasted image 20260206004756.png]]
----
![[Pasted image 20260206004836.png]]
POST Request (Resource Update)



![[Pasted image 20260206005040.png]]
GET Request
- **GREEN** -> Max Age (10 seconds in this case)
-  **YELLOW** -> ETags. Browser asks server to check if ETag is changed or not. If not 304 (Not modified is returned) and browser uses same cached resource.
- **GREEN** -> Last modified is also used like ETag. If any one is changed resource is again sent by the server.

---

### Key Insight

HTTP caching is:

- Powerful
    
- Easy to get wrong
    
- Requires careful ETag management
    

Modern apps often prefer **client-side caching** (e.g. React Query).

---

## 6. Content Negotiation

### What It Solves

Allows client and server to agree on:

- Data format
    
- Language
    
- Compression
    

---

### Important Headers

- `Accept`
    
    - JSON, XML, HTML
        
- `Accept-Language`
    
    - en, es, fr
        
- `Accept-Encoding`
    
    - gzip, deflate, br
        

---

### Server Behavior

Server:

- Reads client preferences
    
- Responds with best supported format
    
- Falls back if needed
    
----
### Example Scenario

Languages accepted -> English, Spanish
Data Format -> JSON, XML
Encoding -> gzip, 

![[Pasted image 20260206005903.png]]
---![[Pasted image 20260206010137.png]]

- **YELLOW** -> Client prefers JSON
-  **ORANGE** -> English is language of choice
-  **YELLOW** -> Preferred formats

Requested resource is shared in English and JSON
	
![[Pasted image 20260206010512.png]]
- **GREEN** -> XML file format
- **YELLOW** -> Spanish Language

Resource in Spanish in XML format.

## 7. HTTP Compression

### Why Compression Exists

Large responses waste:

- Bandwidth
    
- Time
    
- Server resources
    

---

### Compression Flow

1. Client sends `Accept-Encoding: gzip, deflate, br (just examples)`
    
2. Server compresses response
    
3. Response header:
    
    - `Content-Encoding: gzip`
        
4. Browser decompresses automatically to get same resource
    

Example:

![[Pasted image 20260206011036.png]]

This is the case with and without compression. Same file with 11K entries. 

![[Pasted image 20260206011136.png]]

The resource is encoded in gzip and is of size **3.8 MB** , whereas uncompressed **26 MB**.


---

## 8. Persistent Connections (Keep-Alive)

### Before HTTP/1.1

- One TCP connection per request
    
- Very inefficient
    

---

### HTTP/1.1 Behavior

- Connections are **persistent by default**
    
- Multiple requests over one connection
    
- Reduced latency
    

---

### Headers

- `Connection: keep-alive`
    
- Optional parameters:
    
    - `timeout`
        
    - `max`
        

Usually handled automatically by browser & server.

---

## 9. Large Data Transfers

### Uploading Large Files (Multipart)

- `Content-Type: multipart/form-data`
    
- Data sent in **parts**
    
- `boundary` separates parts
    

Used for:

- Images
    
- Videos
    
- Audio
    
- Files

![[Pasted image 20260206012406.png]]
![[Pasted image 20260206012516.png]]

It is a POST request. We are specifying content length 912097b. Content type is multipart/form -data.
- **YELLOW** -> Boundary. Everything after = is the boundary. Found at the start and end of the request. Since our binary data of the file is transmitted in parts, the boundary acts as the delimiter. We can see the boundary at the bottom after connection, that is the start of the binary format of the file.

![[Pasted image 20260206012917.png]]

Delimiter (boundary) at the end of the file. The file in this example is a png file (image).

The server responds with the accepted file details (on the right ).

---

### Streaming Large Responses (Chunked Transfer)

- Server sends data in **chunks**
    
- Client processes progressively
    

Important headers:

- `Content-Type: text/event-stream`
    
- `Connection: keep-alive`
    

Used for:

- Logs
    
- Live feeds
    
- Large downloads
    


**Scenario** 
We are receiving large text files from the server. After the first GET request, we continuously receive response from server ie file received in chunks. 


![[Pasted image 20260206013344.png]]
---
![[Pasted image 20260206013619.png]]

Until file is completely sent, server keeps sending data in chunks. 


![[Pasted image 20260206013721.png]]
Content-Type is not text/content , but  text/event-stream i.e. it will stream the data to the client.
Connection -  keep-alive. Keep the connection alive till all data is sent.

Data is continuously sent and the client keeps appending the data until whole file is reconstructed.
## 10. SSL, TLS, HTTPS (High-Level)

- **SSL (Secure Socket Layer)**
    
    - Old, insecure (deprecated)
        
- **TLS (Transport Layer Security)**
    
    - Modern encryption protocol
        
    - Current version: **TLS 1.3**
        
- **HTTPS (Hyper Text Transfer Protocol Secure)**
    
    - HTTP over TLS
        

TLS ensures:

- Encryption
    
- Server authentication
    
- Data integrity
    

You rarely work with TLS directly, but it protects **everything**.

---

## Final Mental Model

If you understand:

- CORS & preflight
    
- Status codes
    
- Headers
    
- Caching
    
- Compression
    
- Streaming
    
- TLS
    

👉 You can **debug 90% of backend HTTP issues**  
👉 You can read **any backend codebase confidently**

----