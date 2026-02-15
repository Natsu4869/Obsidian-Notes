----
## 1. What is a Backend? (Traditional View)

- A **backend** is a computer/server that:
    
    - Listens for requests (HTTP, HTTPS, WebSocket, gRPC, etc.)
        
    - Uses open ports like **80 (HTTP)** or **443 (HTTPS)**
        
    - Is accessible over the internet
        
- It **serves data**:
    
    - Static files (HTML, CSS, JS, images)
        
    - Dynamic data (usually JSON)
        
- It also **accepts data** from clients (e.g., form submissions, likes, messages)
    

 We call it a _server_ because it **serves content and data**.

---

## 2. How a Backend Request Travels (End-to-End Flow)

### Step-by-step flow:

1. **Browser**
    
    - User enters a URL (e.g., `backend-demo.xyz`)
        
2. **DNS (Domain Name System)**
    
    - Converts domain name → IP address
        
    - Uses records:
        
        - **A record** → maps domain to IP
            
        - **CNAME** → maps domain to another domain
            
3. **AWS EC2 Instance**
    
    - The IP points to a virtual machine (EC2)
        
4. **Firewall (Security Group)**
    
    - AWS security group acts as a firewall
        
    - Only allowed ports (22, 80, 443, etc.) can receive traffic
        
5. **Reverse Proxy (Nginx)**
    
    - Sits in front of backend servers
        
    - Handles:
        
        - SSL (HTTPS)
            
        - Routing
            
        - Redirection
            
6. **Local Backend Server**
    
    - Nginx forwards requests to `localhost:3001`
        
    - Node.js backend runs here
        
7. **Response**
    
    - Backend processes request
        
    - Sends response back to browser
        


![[Pasted image 20260205162435.png]]
Network tools at work to check server requests and status codes
[[![[Pasted image 20260205162549.png]]]]


![[Pasted image 20260205162612.png]]

![[Pasted image 20260205162807.png]]
DNS Server
![[Pasted image 20260205163054.png]]
AWS EC2 Screen

![[Pasted image 20260205163330.png]]
1. AWS Security Group assigned to **AWS Instance**
2. SSH Port used to login to AWS Instance through terminal or command prompts.








### Summary Flow:

```
Browser → DNS → AWS IP → Firewall → Nginx → Node.js Server (local server) → Response
```

---

## 3. Why Use a Reverse Proxy (Nginx)?

- Centralized configuration
    
- Easier SSL management
    
- Routes different domains/subdomains to different local ports
    
- Example:
    
    - `frontend-demo.xyz` → localhost:3000
        
    - `backend-demo.xyz` → localhost:3001
        

---

## 4. Why Do We Need Backends?

### Real-life Example (Instagram Like)

When you click **Like**:

1. Frontend sends request to backend
    
2. Backend:
    
    - Identifies the user
        
    - Saves the like in the database
        
    - Finds post owner
        
    - Triggers notification
        
3. Friend receives notification
    

👉 All this **cannot happen without a centralized server**

### Core Responsibility of Backend:

**DATA**

- Fetch data
    
- Receive data
    
- Persist data
    
- Trigger actions based on data

---




![[Pasted image 20260205164642.png]]
Backend Config files

---

## 5. How Frontends Work (End-to-End)

1. Browser requests a domain
    
2. Server sends **HTML**
    
3. Browser fetches:
    
    - JavaScript
        
    - CSS
        
    - Fonts
        
    - Images
        
4. Browser:
    
    - Paints UI after CSS loads
        
    - Adds event listeners after JS loads
        
5. All frontend logic runs **inside the browser**
    

👉 Browser is the **runtime environment** for frontend code.


![[Pasted image 20260205164159.png]]
1. First the browser requests domain and gets HTML file as response.
2.  Further more, relevant resources like css, js, images, fonts, scripts etc. are fetched according to the HTML response in different requests. (As you can see from the image).


![[Pasted image 20260205164547.png]]
Frontend Config File in EC2 Instance


---

## 6. Key Difference: Frontend vs Backend

|Frontend|Backend|
|---|---|
|Code runs in browser|Code runs on server|
|Browser executes JS|Server executes logic|
|User’s device|Centralized machine|

---

## 7. Why Backend Logic Cannot Live in Frontend

### 1. Security

- Browser is a **sandbox**
    
- No access to:
    
    - File system
        
    - OS processes
        
    - Environment variables
        
- Prevents malicious websites from stealing user data
    

---

### 2. CORS Restrictions

- Browsers block requests to other domains by default
    
- Backend servers have **no such restriction**
    
- Backend often needs to call many external services
    

---

### 3. Databases

- Backend:
    
    - Uses native DB drivers (Postgres, MongoDB, etc.)
        
    - Maintains **connection pools**
        
    - Handles thousands of requests efficiently
        
- Browser:
    
    - Cannot maintain persistent DB connections
        
    - Would overwhelm DB if each user connected directly
        

---

### 4. Computing Power

- Frontends run on:
    
    - Phones
        
    - Old laptops
        
    - Low-RAM devices
        
- Heavy business logic would:
    
    - Lag
        
    - Crash
        
- Backend servers:
    
    - Can scale CPU/RAM
        
    - Handle large workloads reliably
        

---

## 8. Final Takeaway

- Backend exists to:
    
    - Secure data
        
    - Centralize state
        
    - Handle heavy logic
        
    - Communicate with databases & services
        
- Frontend:
    
    - Displays UI
        
    - Captures user interaction
        
    - Delegates work to backend
        

📌 **Backend = Data + Logic + Security + Scale**

---