----

# Serialization & Deserialization

## 1. Client–Server Communication (Big Picture)

In a typical web system, we have a **client** (usually a browser running a JavaScript app like React, Angular, or Vue) and a **server** (running somewhere locally or in the cloud, written in languages like Rust, Java, Python, Go, etc.).

These two communicate over a **network** using protocols such as:

- HTTP (REST APIs) — most common
    
- WebSockets
    
- gRPC
    

For this discussion, we focus on **HTTP-based REST APIs**.

The client sends an HTTP request (method, URL, headers, body), and the server responds with an HTTP response that the client parses and uses to render UI or run business logic.

---

## 2. Core Problem: Different Languages, Different Data Types

The client and server often use **different programming languages**:

- Client → JavaScript (dynamic, loosely typed)
    
- Server → Rust (strict, strongly typed, compiled)
    

These languages have **completely different data types and internal representations**.

So the key question is:

> How can data sent from one language be understood by another language across the internet?

---

## 3. Role of a Common Data Format

To solve this, both client and server **agree on a common standard format** for data exchange.

The flow looks like this conceptually:

- Client converts its internal data → **common format**
    
- Data is transmitted over the network
    
- Server converts the common format → **its own data structures**
    
- Server processes the request
    
- Server converts response → **common format**
    
- Client converts response → **its own data structures**
    

This conversion process is where **serialization and deserialization** come in.

---

## 4. Serialization and Deserialization (Definitions)

**Serialization**

- Converting in-memory data (objects, structs, variables)
    
- Into a **common, transferable format**
    
- Used before **sending or storing** data
    

**Deserialization**

- Converting data from the common format
    
- Back into language-specific data structures
    
- Used after **receiving or reading** data
    

In short:

> **Serialization = language data → common format**  
> **Deserialization = common format → language data**

---

## 5. Where This Happens in Networking

At the **application layer** (from a backend engineer’s perspective):

- Client serializes data (e.g., JS object → JSON)
    
- Network layers handle packets, frames, bits (OSI model details)
    
- Server receives data and deserializes it (JSON → Rust struct)
    

As backend engineers, we **do not worry about IP packets or bits**.  
Our responsibility starts and ends at the **application layer format**.

---

## 6. Serialization Standards

To enable this language-agnostic communication, we use **serialization standards**.

### Two main categories:

#### 1️⃣ Text-based serialization

- Human-readable
    
- Easy to debug
    
- Common in HTTP APIs
    

Examples:

- JSON (most popular)
    
- XML
    
- YAML
    

#### 2️⃣ Binary serialization

- Compact
    
- Faster
    
- Not human-readable
    
- Often used in high-performance systems
    

Examples:

- Protocol Buffers (Protobuf)
    
- Avro, Thrift, etc.
    

For REST APIs over HTTP, **JSON dominates (~80% usage)**.

---

## 7. JSON (JavaScript Object Notation)

JSON is the most widely used serialization format for client–server communication.

Although it looks similar to JavaScript objects, it is:

- **Language-agnostic**
    
- Used across Java, Rust, Python, Go, PHP, etc.
    

### JSON Rules (Important)

- Data is enclosed in `{ }`
    
- Keys:
    
    - Must be **strings**
        
    - Must be inside **double quotes**
        
- Values can be:
    
    - String
        
    - Number
        
    - Boolean
        
    - Array
        
    - Nested object
        

Example:

```json
{
  "id": 1,
  "title": "Book",
  "author": "Someone",
  "address": {
    "country": "India",
    "phone": 123456
  }
}
```

---

## 8. JSON in HTTP Requests and Responses

### Request flow:

- Client serializes data into JSON
    
- Sends JSON in HTTP request body
    
- Server deserializes JSON into internal data structures
    
- Server processes business logic
    

### Response flow:

- Server serializes response data into JSON
    
- Sends JSON in HTTP response body
    
- Client deserializes JSON into JavaScript objects
    
- Client renders UI or runs logic
    

This entire round-trip is **serialization + deserialization in action**.

---

## 9. Mental Model to Remember

You can simplify everything to this:

> At the application layer, **client talks JSON, server talks JSON**.  
> Everything else (packets, bits, hardware) is abstracted away.

---

## 10. Why This Matters for Backend Engineers

Understanding serialization helps you:

- Work across languages and frameworks
    
- Design clean APIs
    
- Debug request/response issues
    
- Transition easily between technologies (Node → Rust, Python → Go, etc.)
    

You don’t need to master every serialization format —  
**knowing JSON deeply is enough to get started and be productive**.

---

## 11. Final One-Line Summary

> **Serialization and deserialization are the processes of converting data to and from a common, language-agnostic format (like JSON) so that clients and servers can reliably exchange and understand data across the network.**

----