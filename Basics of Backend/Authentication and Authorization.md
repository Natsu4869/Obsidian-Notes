# Authentication and Authorization - Comprehensive Notes

## Core Definitions

**Authentication** answers "Who are you?" - It's the process of assigning an identity to a subject in a given context (platform, OS, application).

**Authorization** answers "What can you do?" - It's the process of determining permissions and capabilities a user has within a particular context.

## Historical Evolution of Authentication

### Pre-Industrial Era

Authentication was intrinsic and based on human contextual trust. Village elders vouched for individuals, and deals were sealed with handshakes. This system couldn't scale beyond familiar circles.

### Medieval Period - Seals and Early Cryptography

Society needed systems that scaled beyond personal recognition. Wax seals emerged as early cryptographic mechanisms - unique patterns on documents acting as signatures. These were physical authentication tokens based on possession ("something you have"). However, seals were vulnerable to forgery, marking the first authentication bypass attacks. This led to more sophisticated mechanisms like watermarks and encrypted codes.

### Industrial Revolution - Passphrases and Shared Secrets

With the telegraph becoming critical infrastructure, operators used pre-agreed passphrases for secure message validation. This was an early form of shared secrets and static passwords, shifting from "something you possess" to "something you know."

### Mid-20th Century - Mainframes and Digital Authentication

In 1961, MIT researchers working on the Compatible Time Sharing System (CTSS) introduced passwords for multi-user systems. Initially, passwords were stored in plain text, which proved disastrous when someone printed the password file. This incident sparked the philosophy of secure password storage, leading to hashing algorithms that transform passwords into irreversible, fixed-length representations.

### 1970s - Cryptographic Revolution

Whitfield Diffie and Martin Hellman invented the Diffie-Hellman key exchange, introducing asymmetric cryptography. This allowed two parties to establish shared secrets over untrusted mediums. Asymmetric cryptography became the backbone of modern authentication protocols (PKI - Public Key Infrastructure). The Kerberos protocol emerged, introducing ticket-based authentication with trusted third parties - a precursor to modern token-based systems.

### 1990s - Internet Era and MFA

As the internet grew, simple username/password systems proved insufficient against brute force and dictionary attacks. Multi-Factor Authentication (MFA) combined three principles:

- **Something you know** (passwords, PINs)
- **Something you have** (smart cards, OTP generators)
- **Something you are** (biometric data - fingerprints, retina scans)

Biometric authentication introduced challenges like false positives/negatives and template security concerns.

### 21st Century - Modern Authentication

Cloud computing, mobile devices, and API-based architectures demanded advanced frameworks. Key developments include:

- OAuth 2.0 and OpenID Connect
- JWTs (JSON Web Tokens)
- Zero Trust Architecture
- Passwordless authentication (WebAuthn using public/private keys in hardware)

### Future Candidates

- **Decentralized Identity** using blockchain
- **Behavioral Biometrics**
- **Post-Quantum Cryptography** - algorithms designed to resist quantum computer attacks, as current cryptographic methods will become vulnerable when quantum computing becomes widespread

## Three Critical Components

### 1. Sessions

HTTP was designed to be stateless - treating every request as isolated with no memory of past exchanges. This worked for early static web pages but became a bottleneck for dynamic, interactive websites (e-commerce carts, persistent login states).

**How Sessions Work:**

- **Session Creation**: When a user logs in, the server creates a unique session ID and stores it alongside user data (role, cart items, authentication status) in persistent storage (database or Redis)
- **Session ID Transmission**: The session ID is sent to the client's browser as a cookie
- **Subsequent Requests**: All future requests include this cookie, allowing the server to fetch user data from persistent storage
- **Expiration**: Sessions are short-lived with expiry dates (e.g., 15 minutes). After expiration, a new session is created

**Evolution of Session Storage:**

- **File-based**: Early implementations stored data in files but suffered from scalability issues
- **Database-backed**: Moved to databases for faster lookups and persistence across server restarts
- **Distributed architectures**: Modern systems use in-memory stores like Redis or Memcached for faster access compared to disk-based databases

### 2. JWTs (JSON Web Tokens)

By the mid-2000s, stateful systems with session storage became costly and created bottlenecks:

- **Memory overhead**: Maintaining session data for millions of users was expensive
- **Replication challenges**: Synchronizing session data across globally distributed servers introduced latency and consistency issues

JWTs emerged as a stateless solution for transferring claims between parties.

**JWT Structure** (three base64-encoded parts):

1. **Header**: Metadata including signing algorithm
2. **Payload**: User data (sub for user ID, iat for issued-at timestamp, optional fields like name and role)
3. **Signature**: Verifies the token hasn't been tampered with, created using a secret key

**Advantages:**

- **Statelessness**: No server-side storage required
- **Scalability**: In microservices architecture, multiple servers can verify the same JWT using a shared secret key
- **Portability**: Lightweight, URL-friendly, can be stored in cookies or local storage, easily passed between systems

**Disadvantages:**

- **Token theft**: If compromised, attackers can impersonate users until expiration
- **Revocation challenges**: No built-in mechanism to invalidate tokens before expiry without changing the server's secret key (which would invalidate all user tokens)

**Hybrid Approach Solution:** Combine statelessness with statefulness by maintaining a blacklist of JWTs in persistent storage. After verifying the JWT, check against the blacklist to temporarily block or revoke access. While this reintroduces some state, it addresses security concerns while maintaining most JWT benefits.

**Best Practice**: Use an authentication provider (Auth0, Clerk, etc.) rather than implementing your own system in production. They handle the complexity of secure authentication, including proper algorithms, hashing, salting, and security measures.

### 3. Cookies

Cookies are a mechanism for servers to store information in a user's browser. This enables servers to set data on the client side that automatically gets sent back with subsequent requests to that same server.

**Key Features:**

- Server sets a cookie in the client's browser via HTTP headers
- Browsers automatically send cookies with all subsequent requests to that server
- Cookies from one server are isolated and cannot be accessed by other servers (security feature)

**Authentication Workflow:**

1. Client authenticates with username/password
2. If successful, server sets a cookie containing an authorization token (JWT or session ID)
3. Browser automatically includes this cookie in all subsequent requests
4. Server validates the token to identify and authorize the user

Cookies automate the token exchange process, eliminating the need for manual token management on each request.

## Types of Authentication

### 1. Stateful Authentication

**Process:**

1. Client sends username/password to server
2. Server validates credentials
3. Server generates session ID and bundles with user data
4. Stores in Redis (or database)
5. Sends session ID back to client in an HTTP-only cookie (JavaScript cannot access it)
6. Subsequent requests include this cookie
7. Server retrieves session ID, checks Redis for validity/expiry and user data
8. Identifies and authorizes user

**Pros:**

- Centralized control over all sessions
- Real-time information about active sessions
- Easy revocation - can log out users and revoke sessions directly
- Well-suited for applications with moderate traffic and strict session requirements
- More secure - recommended for most applications

**Cons:**

- Limited scalability in very large systems
- Higher operational complexity with distributed systems
- Latency issues when synchronizing across multiple regions

### 2. Stateless Authentication

**Process:**

1. Client sends username/password
2. Server validates credentials
3. Server creates signed JWT with user information (ID, role, etc.)
4. Sends JWT to client
5. Client includes JWT in Authorization header of subsequent requests
6. Server verifies JWT signature with secret key
7. Extracts user information from token for authorization

**Pros:**

- No server-side storage required
- Highly scalable - multiple servers can verify tokens with shared secret
- Ideal for distributed architectures and mobile applications
- No cookie dependencies

**Cons:**

- Token revocation is complex - cannot invalidate before expiry without changing secret key (affecting all users)
- Token theft allows impersonation until expiration

**When to Use:**

- Stateful for main web applications (browsers)
- Stateless for APIs, mobile apps, and third-party integrations
- Consider hybrid approach combining both

### 3. API Key Based Authentication

API keys provide programmatic access to servers without UI-based authentication flows.

**Use Case Example:** ChatGPT has a web UI where users interact visually. OpenAI also provides API keys so developers can access the underlying models programmatically for custom applications or server-to-server integration.

**How It Works:**

1. User generates API key through platform UI
2. Receives cryptographically random string
3. Uses this key to authenticate requests programmatically
4. Server validates key and grants access to resources

**Advantages:**

- Easy to generate
- Ideal for machine-to-machine communication (server to server, no human interaction)
- No complex username/password/token workflow needed
- Suitable for programmatic integrations

**Machine-to-Machine Example:** A server uses ChatGPT's API key to access ChatGPT's summarization capabilities, processes the response, and returns it to its own UI - all without human intervention in the authentication flow.

### 4. OAuth 2.0 and OpenID Connect

**The Problem - Delegation:** Platforms needed to access resources from other platforms. For example:

- Travel app needing access to Gmail to scan flight tickets
- Social media app wanting to import contacts from Google

**Initial Flawed Solution:** Users shared passwords between platforms, which was disastrous:

- Gave full access to everything in the account
- No way to limit permissions
- Impossible to revoke access without changing passwords everywhere

**OAuth 1.0 (2007) - The Revolution:** Engineers from Google and Twitter standardized sharing access without sharing passwords using tokens with specific permissions.

**Four Components:**

1. **Resource Owner**: The user who owns the data
2. **Client**: The app requesting access (e.g., Facebook)
3. **Resource Server**: Where the data lives (e.g., Google)
4. **Authorization Server**: Issues tokens after authentication

**OAuth 1.0 Flow:**

1. Client redirects user to authorization server
2. User authenticates and grants permissions
3. Authorization server sends token to client
4. Client uses token to access resources on resource server

**OAuth 2.0 (2010) - Simplified and Enhanced:**

**Improvements:**

- Introduced bearer tokens (simpler than OAuth 1.0's cryptographic signatures, though more vulnerable)
- Multiple flows for different app types:
    - **Authorization Code Flow**: For server-side apps
    - **Implicit Flow**: For browser-based apps (now discouraged due to security)
    - **Client Credentials Flow**: For machine-to-machine communication
    - **Device Code Flow**: For limited input devices (Smart TVs)

**Limitation**: OAuth 2.0 solved authorization (what you can do) but not authentication (who you are).

**OpenID Connect (2014) - Adding Authentication:**

Built on top of OAuth 2.0 by introducing the **ID Token** (typically a JWT) containing:

- User ID
- Issued-at timestamp
- Issuing authority
- User's name, email, profile information

**OpenID Connect Flow:**

1. Client redirects user to authorization server
2. User logs in and grants permissions
3. Authorization server sends authorization code and ID token to client
4. Client exchanges authorization code for access token (and ID token if not received earlier)
5. Client can now:
    - Authenticate user using ID token
    - Access resources using access token on behalf of user

**Modern Use Cases:**

- "Sign in with Google/Facebook/Discord" features
- One platform authenticating users through another without maintaining separate credentials
- Secure resource sharing with granular permissions

OAuth 2.0 and OpenID Connect transformed the internet from password-sharing chaos into a secure, interconnected system with controlled delegation.

## Authorization - Role-Based Access Control (RBAC)

Authorization determines what users can do after authentication - not all users have the same permissions.

**Common Scenario:** A note-taking platform where:

- Regular users can create, read, update, delete their notes
- Deleted notes move to trash, then to a "dead zone" after 30 days
- Platform creators/admins need special access to manage dead zone notes
- Need to grant varying permission levels to different team members

**RBAC Solution:** Define roles with specific permission sets:

- **User role**: Read, write, delete notes
- **Admin role**: Read, write, delete notes + access dead zone
- **Moderator role**: Read, write (custom permissions as needed)

**How It Works:**

1. User registers and is assigned a role (user, admin, moderator)
2. In subsequent requests, user sends token/session ID
3. Server deduces user's role from token or database lookup
4. Role information is passed through middleware chain
5. Each API endpoint checks if user's role has required permissions
6. If insufficient permissions, returns 403 Forbidden error

This provides granular control over who can access what resources and perform which operations.

## Critical Security Considerations

### 1. Error Messages

**Bad Practice** (provides attackers with clues):

- "User not found" → Attacker knows to try different username
- "Incorrect password" → Attacker knows username is valid, focuses on password
- "Account locked due to failed attempts" → Reveals account existence

**Best Practice:** Always use generic messages like "Authentication failed" for all authentication errors. Don't give attackers information about what failed.

### 2. Timing Attacks

**The Vulnerability:** Authentication typically follows these steps:

1. Verify username exists
2. Check if account is locked
3. Hash provided password and compare with stored hash

If username is invalid, response is fast (fails at step 1). If password is incorrect, response is slower (fails at step 3 after expensive hashing operation). Attackers can measure this timing difference to determine whether username or password was incorrect.

**Defense Strategies:**

1. **Constant-time operations**: Use cryptographically secure comparison functions that don't vary execution time based on input
2. **Simulate response delay**: Add artificial delay (e.g., 200ms) even when username doesn't exist, so timing doesn't reveal which step failed

These measures equalize response times, preventing attackers from gathering intelligence about the authentication process.

---

**Final Recommendation**: For production systems, use established authentication providers (Auth0, Clerk, etc.) unless you're very confident in your implementation. They handle the complexity of secure authentication, including proper hashing, salting, timing attack prevention, and keeping up with evolving security standards.