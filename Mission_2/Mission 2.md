# Module 5:  **Web Architecture & Node.js Fundamentals**

### **1. Client-Server Architecture**
![Client-Server Architecture](../media/Mission%202-1778357633931.png)

The web works mainly through communication between two sides: the client and the server.

The **client** is the application used by the user, usually a web browser. Its job is to provide the interface and interact with the server. When a user opens a website, clicks a button, logs in, or searches for something, the client creates a request asking for data or resources.

The **server** is a remote computer that stores resources and handles logic. It receives requests from clients, processes them, and returns responses. A response may contain:

- an HTML page
    
- JSON data
    
- images or videos
    
- authentication results
    
- error messages
    

In short:

> Client asks → Server processes → Server responds → Client displays

- Understanding a URL
    

**Example:**

`https://example.com/path`

A URL (Uniform Resource Locator) is the address used to locate resources on the internet.

It is divided into multiple parts:

- `https`
    
    This is the **protocol**.  
    It defines the communication rules between the client and server.
    
    `HTTP` transfers data normally.  
    `HTTPS` transfers data securely using encryption.
    
- `example.com`
    
    This is the **domain name**.  
    Humans use domain names because they are easier to remember than IP addresses.
    
    Behind the scenes, the browser converts the domain into an IP address through DNS.
    
- `/path`
    
    This is the **resource path**.  
    It tells the server which specific resource the client wants.
    
    Examples:
    
    - `/about`
        
    - `/products`
        
    - `/api/users`
        

- What Actually Happens When You Visit a Website
    

When a user enters a URL or clicks a link, several things happen internally:

1. The browser reads the URL.
    
2. DNS translates the domain name into an IP address.
    
3. The browser sends an HTTP/HTTPS request to the server.
    
4. The server receives and processes the request.
    
5. The server returns a response.
    
6. The browser renders the response for the user.
    

This entire process usually happens within milliseconds.

- Rules of Communication
    

Clients and servers cannot communicate randomly. They follow predefined standards called **protocols**.

The most important web protocol is HTTP/HTTPS.

These protocols define:

- how requests are structured
    
- how responses are returned
    
- how status codes work
    
- how data is transferred
    
- how security is handled
    

Without these rules, browsers and servers would not understand each other.

- Key Idea
    

A website is not “opened directly.”

The browser is constantly:

- sending requests
    
- receiving responses
    
- rendering data
    

Modern web applications are basically structured systems of client–server communication.
    


---

### **2. Domain Name & DNS (Domain Name Server)**
![DNS](../media/Mission%202-1778357738536.png)

The internet does not actually understand names like `google.com` or `example.com`. Those are just human-friendly labels.

Behind the scenes, everything runs on numerical addresses called **IP addresses**.

An **IP address** (like `139.59.132.134`) is the real location of a server on the internet. It is how machines find each other. Humans don’t use it directly because it is hard to remember and not scalable.

So instead of remembering numbers, we use a **domain name system**.

- Domain Name vs IP Address
    

A **domain name** is just a readable alias for an IP address.

- Domain Name → `example.com`
    
- Real Address → `139.59.132.134`
    

They are equivalent, but used in different contexts:

- humans use domain names
    
- machines use IP addresses
    

The domain itself does not store data. It simply points to where the data lives.

- DNS (Domain Name System)
    

DNS is basically the internet’s “phonebook”.

It translates:

> domain name → IP address

When you type a URL or click a link, the system does not immediately know the server. It first asks DNS:

Flow:

User request (domain name)  
→ DNS lookup  
→ returns IP address  
→ browser connects to server

Often the result also includes a **port**, which specifies the service:

- `:443` → HTTPS (secure web traffic)
    
- `:80` → HTTP (normal web traffic)
    

So the real connection target becomes something like:  
`139.59.132.134:443`

- What happens when you click a domain
    

When you click a link or enter a domain:

The browser first checks:

- “What IP is this domain mapped to?”
    

If unknown:

- it queries DNS servers
    
- gets back the IP address
    

Then:

- it connects directly to that IP
    
- requests the resource from the server
    
- server responds
    

- Key idea
    

Domain names are just a convenience layer.

They hide:

- raw IP addresses
    
- network complexity
    
- server location details
    

DNS is what makes the internet usable at scale.

---

### **3. HTTP Requests & Rendering**


![HTTP Requests & Rendering](../media/Mission%202-1778358051588.png)

- Connection: From IP to Server Communication
    

After DNS resolves a domain into an IP address, the browser establishes a real network connection with the server.

This connection is typically made using **TCP/IP**, which is a reliable communication protocol over the internet.

- **IP** identifies where the server is
    
- **TCP** ensures data is delivered correctly and in order
    

Once the connection is established, the client and server can exchange structured messages called **requests and responses**.

- HTTP Request Structure
    

A request is the message sent from the client to the server. It is not just “a call”, but a structured object with three main parts:

1. **Method**
    
    This defines what action the client wants to perform.
    
    - `GET` → fetch data
        
    - `POST` → send new data
        
    - `PUT` → replace existing data
        
    - `PATCH` → partially update data
        
    - `DELETE` → remove data
        
    
    The method defines intent, not implementation.
    
2. **Headers**
    
    Metadata about the request.
    
    Examples:
    
    - `Host`: target domain
        
    - `Content-Type`: type of data (e.g. JSON)
        
    - `Authorization`: login token or credentials
        
    - `Accept-Encoding`: supported compression formats
        
    
    Headers help the server understand how to interpret the request.
    
3. **Body**
    
    The actual data sent to the server (only used in methods like POST, PUT, PATCH).
    
    Most modern applications use **JSON** because it is lightweight and language-independent.
    

- HTTP Response Structure
    

After processing the request, the server sends back a response containing:

- **Status Code**
    
    Indicates the result of the request:
    
    - `200` → success
        
    - `201` → created
        
    - `400` → bad request
        
    - `401` → unauthorized
        
    - `404` → not found
        
    - `500` → server error
        
- **Headers**
    
    Metadata about the response:
    
    - content type
        
    - caching rules
        
    - cookies
        
    - security policies
        
- **Body**
    
    The actual returned data:
    
    - HTML page
        
    - JSON object
        
    - file data
        
    - error message
        

####  Rendering Strategies

Once data reaches the client, the UI can be built in two main ways.

**Server-Side Rendering (SSR)**

In SSR, the server builds the full HTML before sending it to the browser.

Process:

- request goes to server
    
- server fetches data from database
    
- server combines data with a template engine (like EJS, Pug, or Handlebars)
    
- server generates complete HTML
    
- browser receives ready-to-display page
    

So the flow is:  
> DB + Request + Template → Full HTML → Browser

This reduces client-side work but increases server load.

**Client-Side Rendering (CSR)**

In CSR, the server does not send full HTML pages. Instead, it sends raw data (usually JSON), and the browser builds the UI.

Process:

- browser sends request to API
    
- server returns JSON from database
    
- frontend framework (React, etc.) builds the UI in the browser
    

So the flow is:  
API → JSON → UI rendered in browser

Advantages:

- reusable backend (same API for web, mobile, desktop)
    
- faster interactions after initial load
    
- scalable architecture
    

This is why modern systems prefer API-first design.

- Key Idea
    

SSR = server builds UI  
CSR = browser builds UI using data

Both rely on the same core system: HTTP request/response over TCP/IP.
        

---

### **4. Node.js Internal Architecture**

**TCP/IP and Data Transmission**

TCP/IP does not send data as one continuous stream. Instead, it splits data into smaller units called packets (chunks). These packets are sent independently over the network and reassembled at the destination.

TCP ensures reliability in this process:

- packets are delivered in order
    
- missing packets are resent
    
- data integrity is maintained
    

IP handles routing of these packets across different networks until they reach the correct destination.

---

**V8 Engine**

V8 is a JavaScript engine used in environments like browsers and Node.js. It is built using C++ with tightly optimized internal systems.

Its main role is to execute JavaScript efficiently by converting it into machine-level instructions.

Process:  
JavaScript code → optimized intermediate form → machine code (binary)

Instead of interpreting code line-by-line, V8 uses Just-In-Time (JIT) compilation to improve performance. This is why JavaScript can run fast enough for modern applications.

---

**LibUV**

LibUV is a C++ library that powers asynchronous operations in environments like Node.js.

It handles tasks that should not block the main execution thread, such as:

- file system operations
    
- network requests
    
- OS-level interactions
    
- timers and event handling
    

Instead of waiting for these tasks to finish, LibUV delegates them to the operating system and continues executing other code. When the task completes, it triggers a callback.

This is what enables non-blocking I/O and event-driven architecture.

---

**Core Idea**

- TCP/IP manages _how data travels across networks_
    
- V8 manages _how JavaScript runs as machine code_
    
- LibUV manages _how asynchronous operations are handled efficiently_
    

### **Event Loop & Thread Pool**
![Event Loop](../media/Mission%202-1778358568803.png)

![Thread Pool](../media/Mission%202-1778358479582.png)

**Event Loop**

The event loop is the core of Node.js execution. It continuously checks for completed asynchronous operations and decides which callback should run next.

Its main job is not to perform heavy work, but to:

- schedule callbacks
    
- manage non-blocking I/O
    
- coordinate execution flow
    

When an async task completes (like a network request), the event loop picks up its callback and executes it in the main thread.

---

**Thread Pool**

Some tasks cannot be handled efficiently on the main thread because they are CPU or system intensive. For these, Node.js uses a background thread pool (managed by LibUV).

It handles operations like:

- file system reading/writing
    
- data compression
    
- cryptographic operations
    
- DNS lookups (in some cases)
    

These tasks run in parallel in the background. Once completed, results are pushed back to the event loop.

Important point: the thread pool is separate from JavaScript execution. JS itself still runs on a single main thread.

---

**Event-Driven Architecture**

Node.js follows an event-driven model where actions are triggered by events instead of continuous polling.

Flow:

Event occurs → EventEmitter emits event → Listener detects event → Callback executes

This pattern is widely used in:

- HTTP servers
    
- file systems
    
- streams
    
- user-defined event systems
    

Instead of blocking execution, Node reacts only when something happens.

---

**Key Insight**

Node.js is single-threaded at the JavaScript level, but not single-threaded in execution overall.

- JS execution → single main thread
    
- Background work → thread pool
    
- Coordination → event loop
    

This hybrid model is what enables high scalability with low resource usage.

---

**Handling CPU-Heavy Tasks**

The default model is not suitable for heavy computation because it can block the event loop.

Modern solutions include:

- Worker Threads (true parallel execution inside Node.js)
    
- Microservices (offloading heavy work to separate processes)
    
- External job queues (Redis-based systems, etc.)
    

So the system scales by moving CPU-heavy work away from the event loop instead of forcing it to handle everything.

![Node.js System Architecture](../media/Mission%202-1778358227666.png)

---

### **5. Streams & Buffers**

Large data in real systems (files, videos, network responses) cannot always be handled in one go. Loading everything at once would consume too much memory and slow the system down. Node.js solves this using streams and buffers.

---

**Streams**

A stream is a way of handling data in small parts (chunks) instead of loading it completely.

Instead of:

> wait for full file → then process

Node.js does:

> receive chunk → process chunk → continue

This makes data processing continuous and efficient.

Streams are commonly used in:

- reading large files
    
- video/audio playback
    
- HTTP requests/responses
    
- real-time data transfer
    

---

**Buffer**

A buffer is temporary memory used to hold raw binary data while it is being processed.

When data arrives in chunks, it is not always immediately usable. The buffer acts as a staging area where data is stored briefly before being consumed or passed along.

In simple terms:

- Stream = flow of data
    
- Buffer = waiting area for data chunks
    

---

**Why Streams Are Useful**

Streams improve performance in two main ways:

- **Lower memory usage**: only small chunks are stored at a time
    
- **Better responsiveness**: processing starts immediately instead of waiting for full data
    

This is critical in large-scale systems like video platforms or file servers.

---

**Types of Streams**

Node.js provides four main stream types:

1. **Readable Stream**  
    Used when data is being read from a source (e.g., file reading, HTTP request body).
    
2. **Writable Stream**  
    Used when data is being written somewhere (e.g., saving a file, sending a response).
    
3. **Duplex Stream**  
    Can both read and write data (e.g., TCP sockets).
    
4. **Transform Stream**  
    A special duplex stream that modifies data while it is passing through (e.g., compression, encryption).
    

---

**Key Idea**

Streams allow Node.js to process data continuously instead of waiting for full load, while buffers temporarily hold chunks during that process.

This combination is what makes Node.js efficient for handling large-scale I/O operations.
    

---

### **6. Modules & Modular Systems**

**IIFE (Immediately Invoked Function Expression)**

An IIFE is a function that is defined and executed immediately after its creation. It is mainly used to create a private scope and avoid polluting the global namespace.

In JavaScript, variables declared in the global scope can accidentally conflict with other code. IIFE solves this by wrapping code inside its own isolated scope.

Syntax:  
`() => { /* code */ }();`

More commonly:  
`(() => { /* code */ })();`

What happens here:

- A function is defined
    
- It is executed instantly
    
- Its variables are not accessible outside
    

This pattern was heavily used before modern module systems existed.

---

**Module Systems in Node.js**

Node.js supports two main module systems, which define how code is organized and shared between files.

**CommonJS**

This is the original module system used in Node.js.

- Uses `require()` to import modules
    
- Uses `module.exports` to export functionality
    

Example behavior:

- Synchronous loading
    
- Works directly in Node.js without configuration
    

It is still widely used in existing codebases.

---

**ES Modules (ESM)**

ES Modules are the modern JavaScript standard.

- Uses `import` and `export`
    
- Supports static analysis (better optimization)
    
- Works in both browsers and Node.js (Node ≥ v14 with configuration)
    

This system is more structured and future-oriented compared to CommonJS.

---

**Module Categories in Node.js**

Node.js organizes modules into three main types:

**1. Local Modules**  
Custom modules written by the developer inside the project. These are used to structure application logic into reusable files.

**2. Built-in Modules**  
Pre-installed modules provided by Node.js runtime. No installation required.

Common examples:

- `fs` → file system operations
    
- `http` → server creation
    
- `path` → file path handling
    
- `os` → system information
    
- `url` → URL parsing
    

**3. Third-Party Modules**  
External packages installed via package managers like npm. These extend functionality beyond core Node.js capabilities.

Examples include frameworks, utilities, and libraries.

---

**Key Idea**

- IIFE creates isolation before modules existed
    
- CommonJS is Node’s traditional system
    
- ES Modules is the modern standard
    
- Node apps are structured using local, built-in, and third-party modules
    

Together, these define how JavaScript code is organized, shared, and executed in real applications.
    

### Non - blocking I/O handling

---

![Node.js System Architecture](../media/Mission%202-1778358227666.png)

In Node.js, the system is designed to handle multiple operations simultaneously without waiting for one to finish before starting the next. This is particularly evident in how it manages concurrent user requests.

#### **Execution Scenario**

- **User 1**: Attempting to fetch data from a database.
    
- **User 2**: Attempting to read a file from the system asynchronously.
    
- **User 3**: Attempting to fetch additional data.
    

#### **How Node.js Handles Multiple Requests**

Node.js uses a single-threaded server model to manage these incoming tasks through a specific architecture:

- **Single Threaded Server**: The main thread receives all I/O operations from clients.
    
- **The Thread Pool**: For tasks that are blocking or heavy (like file system access or database queries), the single thread offloads the work to the **Thread Pool**.
    
- **LibUV & Event Loop**: These components coordinate the "non-blocking" nature by registering callbacks and checking when the offloaded tasks are complete.
    
- **Database Interaction**: The Thread Pool communicates with the Database (DB) to retrieve data and sends the result back to the client once finished.
    

#### **Limitations & Solutions**

- **The Problem**: While the Thread Pool handles I/O well, it cannot efficiently handle highly **CPU-intensive tasks** (like heavy mathematical calculations or image processing).
    
- **The Solution**: For these specific limitations, **Multithreading** (Worker Threads) is used. Modern Node.js versions (v14+) now have native support for handling these multi-processing needs.
    

---

> _Note: This architecture ensures that the main thread remains free to handle new incoming requests, making the system "non-blocking"._

**Scenario: Real-world request in action (Login + Dashboard load)**

A user opens a laptop and enters `https://example.com/dashboard` in the browser. The goal is to access a personal dashboard after authentication.

The browser first parses the URL and realizes it needs to contact a server. Since it only has a domain name, it sends a DNS query to resolve `example.com` into an IP address. DNS responds with something like `139.59.132.134`.

Now the browser knows where the server lives. It establishes a TCP connection with that IP on port `443` (HTTPS). A handshake happens to ensure both sides are ready for reliable communication.

Once the connection is stable, the browser creates an HTTP request:

- Method: GET
    
- Path: /dashboard
    
- Headers: include authentication token (JWT/cookie), content type, etc.
    

This request is split into TCP packets and sent across the network.

On the server side, Node.js (running behind LibUV) receives these packets. LibUV reconstructs them into a complete request and passes it to the event loop.

The event loop decides how to handle it. Since this request needs authentication and database access, it triggers:

- auth validation
    
- database query for user dashboard data
    

If file I/O or heavy operations are needed, they are pushed to the thread pool so the main thread does not block.

Once the data is retrieved, Node builds the response. If the payload is large (like dashboard data with many components), it may use streams to send chunks instead of a single payload.

The response is structured:

- Status: 200 OK
    
- Headers: content-type, caching rules
    
- Body: HTML (SSR) or JSON (CSR API response)
    

The response travels back through TCP packets to the browser.

Finally:

- If SSR → browser directly renders HTML
    
- If CSR → frontend framework (React-like) builds UI using JSON
    

The user sees their dashboard loaded.

---

**Key takeaway:**  
This is not a single process. It is a chained system of multiple layers:  
DNS → TCP → HTTP → Node Event Loop → DB → Response → Browser Rendering

---

**Full System Graph (Obsidian-compatible)**

```mermaid
flowchart LR

%% ================= USER ACTION =================
A[User opens browser] --> B[Enter URL example.com/dashboard]

%% ================= DNS LAYER =================
B --> C[Parse URL]
C --> D[DNS Request sent]
D --> E[DNS resolves domain]
E --> F[IP Address returned 139.59.132.134]

%% ================= TCP LAYER =================
F --> G[TCP Connection request]
G --> H[TCP 3 way handshake]
H --> I[Secure HTTPS channel established port 443]

%% ================= HTTP REQUEST =================
I --> J[Create HTTP Request]
J --> J1[Method GET]
J --> J2[Path dashboard]
J --> J3[Headers auth token cookies]

J --> K[Request split into TCP packets]
K --> L[Sent over network]

%% ================= SERVER ENTRY =================
L --> M[Node.js receives packets via LibUV]
M --> N[Reconstruct full request]

%% ================= EVENT LOOP =================
N --> O[Event Loop receives request]
O --> P[Route to handler]

%% ================= AUTH + LOGIC =================
P --> Q[Auth Middleware verify token]

Q --> R{Authenticated?}
R -->|No| R1[Return 401 Unauthorized]
R -->|Yes| S[Controller executes logic]

%% ================= BACKGROUND TASKS =================
S --> T{Heavy operation needed?}

T -->|Yes| U[Thread Pool via LibUV]
U --> U1[File IO operations]
U --> U2[Crypto hashing]
U --> U3[Background tasks]
U1 --> V
U2 --> V
U3 --> V

T -->|No| V[Direct execution]

%% ================= DATABASE =================
V --> W[Database query]
W --> X[User data fetched]

%% ================= RESPONSE BUILD =================
X --> Y[Build response]
Y --> Y1[Status 200 OK]
Y --> Y2[Headers content type caching]
Y --> Y3[Body HTML or JSON]

%% ================= STREAMING =================
Y --> Z{Large response?}
Z -->|Yes| Z1[Stream chunks via LibUV]
Z -->|No| Z2[Single payload response]

Z1 --> AA[TCP packets sent back]
Z2 --> AA

%% ================= CLIENT RENDER =================
AA --> AB[Browser receives response]

AB --> AC{SSR or CSR?}

AC -->|SSR| AD[Render HTML directly]
AC -->|CSR| AE[React style UI build from JSON]

AD --> AF[Dashboard displayed]
AE --> AF
```

---

If you understand this graph properly, you basically understand:

- how a real backend system executes
    
- where latency comes from
    
- where scaling issues happen
    
- where bottlenecks form
    

# Module 6: Node.js REST API development and server architecture

## Building server with node.js

A computer can act as a server. When you run and serve your code from your own device, it is called a local server.

---

1. **Initializing a Node.js project**

Steps:

- Open project folder in VS Code
    
- Open terminal
    
- Run command:
    

```bash
npm init
```

This creates a `package.json` file.

---

**package.json (basic structure)**

```json
{
  "name": "learning-node",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}
```

---

**Important fields in package.json**

**main**

- Entry point of the application
    
- Default file Node runs when the project starts
    
- Usually `index.js`
    

**scripts**

- Shortcut commands for running tasks
    
- Example: testing, starting server, build scripts
    
- You can run them using:
    

```bash
npm run test
```

---

**Key idea**

- `package.json` is the configuration file of a Node.js project
    
- It stores metadata + scripts + dependencies
    
- Every Node project depends on it

Understood. Keeping structure and numbering intact, only refining clarity.

---

**2. Install TypeScript**

```bash
npm i -D typescript
```

**-D (dev dependency)**

- Used only during development/build time
    
- Not shipped to production
    
- TypeScript runs at compile time, so runtime doesn’t need it
    

**Initialize TypeScript config**

```bash
npx tsc --init
```

---

**3. Inside tsconfig.json**

```ts
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {

    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",

    // Environment Settings
    "module": "nodenext",
    "target": "esnext",
    "types": [],

    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}
```

**What tsconfig is**

- Core configuration file for TypeScript compiler
    
- Controls:
    
    - How code is compiled
        
    - Output structure
        
    - Type checking strictness
        
    - Module system behavior
        

---

**4. Required tweaks**

- Uncomment:
    

```json
"rootDir": "./src",
"outDir": "./dist",
```

- Change:
    

```json
"module": "esnext"
```

- Why:
    
    - Cleaner modern ES module system
        
    - Better compatibility with modern Node.js + tooling
        
    - Avoids extra Node-specific module resolution complexity
        

---

```json
"types": ["node"]
```

- Why:
    
    - Enables Node.js built-in type definitions
        
    - Gives TypeScript awareness of `process`, `fs`, `path`, etc.
        

Install required package:

```bash
npm i -D @types/node
```

---

**5. Project structure**

Since `rootDir` is `./src`, all source code goes there.

Create structure:

```
learning-node/
├─ src/
│  └─ server.ts
├─ package-lock.json
├─ package.json
└─ tsconfig.json
```

- `src/` → TypeScript source code
    
- `dist/` → compiled JavaScript output (generated later)
    
- `server.ts` → entry backend file
- 
**6. Inside server.ts — Creating a basic Node.js server**

Node.js works on an event-driven architecture:

- Event occurs
    
- EventEmitter emits event
    
- Listener detects event
    
- Callback executes
    

In HTTP servers:

- Incoming request = event
    
- Server callback = event listener
    
- Response logic = callback execution
    

---

**Basic HTTP server setup**

```ts
import { createServer, IncomingMessage, Server, ServerResponse } from "http";

const server: Server = createServer(
  (req: IncomingMessage, res: ServerResponse) => {
    console.log(req);
  }
);

server.listen(5000, () => {
  console.log("server is running on port: 5000");
});
```

---

**What is happening here**

- `createServer(...)`
    
    - Creates an HTTP server
        
    - Internally registers a request event listener
        
- `(req, res) => {}`
    
    - This is the **event callback**
        
    - Runs every time a request hits the server
        
- `req: IncomingMessage`
    
    - Contains request data (URL, method, headers, etc.)
        
- `res: ServerResponse`
    
    - Used to send response back to client
        

---

**Event-driven flow in this code**

- Browser sends request → event triggered
    
- Node HTTP server detects request event
    
- `createServer` callback runs
    
- `req` is processed
    
- `res` sends output back (currently not used yet)
    

---

**Server startup**

```ts
server.listen(5000, () => {
  console.log("server is running on port: 5000");
});
```

- Starts listening on port `5000`
    
- Keeps process alive
    
- Logs confirmation when server is ready
    

---

**Key idea**

- Node HTTP server is just a wrapper over event system
    
- Every request is an event
    
- Your callback is the listener reacting to that event

**7. Running TypeScript server in development**

To execute TypeScript directly without manual compilation, install a runtime helper:

```bash
npm i tsx
```

---

**What is `tsx`**

- Runs TypeScript files directly in Node.js
    
- No need to manually compile (`tsc`)
    
- Supports watch mode (auto-reload on file changes)
    
- Common in modern Node.js dev workflows
    

---

**Add dev script in package.json**

```json
"scripts": {
  "dev": "tsx watch ./src/server.ts",
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

---

**What this script does**

- `tsx watch`
    
    - Watches file changes
        
    - Automatically restarts server on update
        
- `./src/server.ts`
    
    - Entry point of backend
        

---

**Run development server**

```bash
npm run dev
```

---

**Result**

- Server starts immediately
    
- Runs TypeScript without manual build step
    
- Auto-restarts on file changes
    
- Faster development loop compared to `tsc + node dist` workflow

Understood. Same flow, same structure, only fixing English + completing gaps + improving clarity.

---

## Route Handling with HTTP Requests & Responses

Here, we're gonna **handle HTTP requests and responses using Node.js built-in HTTP module**

go to your web browser and search `localhost: _your port number (eg: 5000)_` and then check console

```js
server is running on port: 5000
<ref *2> IncomingMessage {
  _events: {
    close: undefined,
    error: undefined,
    data: undefined,
    end: undefined,
    readable: undefined
  },
  _readableState: ReadableState {
    highWaterMark: 16384,
    buffer: [],
    bufferIndex: 0,
    length: 0,
    pipes: [],
    awaitDrainWriters: null,
    [Symbol(kState)]: 1315084
  },
  _maxListeners: undefined,
  socket: <ref *1> Socket {
```

... a huge JSON (what is this?)

👉 This is the full **IncomingMessage object**, which is Node.js internal representation of an incoming HTTP request stream. It contains request data, metadata, and low-level network information.

---

Let's check some stuff

> [!As we saw before]  
> A request is a message sent from the client to the server. It is not just a simple “call”, but a structured object with three main parts:
> 
> 1. **Method**
>     
> 
> This defines what action the client wants to perform.
> 
> - `GET` → fetch data
>     
> - `POST` → send new data
>     
> - `PUT` → replace existing data
>     
> - `PATCH` → partially update data
>     
> - `DELETE` → remove data
>     
> 
> The method defines intent, not implementation.

---

Let's look at the method in the big JSON we got

```json
{
  url: '/',
  method: 'GET', // it's a GET method
  statusCode: null,
  statusMessage: null,
}
```

We can log it in the console if we want

```ts
import { createServer, IncomingMessage, Server } from "http";

const server: Server = createServer((req: IncomingMessage, res) => {
  console.log(req.method);
  console.log(req.url);
});
```

```txt
server is running on port: 5000
GET
/
```

---

```ts
import { createServer, IncomingMessage, Server } from "http";

const server: Server = createServer((req: IncomingMessage, res) => {
  const method = req.method;
  const url = req.url;

  if (url == '/' && method == 'GET') {
    console.log("This is root route");
  }
});
```

```txt
server is running on port: 5000
This is root route
```

Now, what if we want to see the response in the browser

Let's look at the docs: [https://nodejs.org/api/http.html#responsewriteheadstatuscode-statusmessage-headers](https://nodejs.org/api/http.html#responsewriteheadstatuscode-statusmessage-headers)

```ts
response.writeHead(statusCode[, statusMessage][, headers])
```

```ts
const body = 'hello world';
response
  .writeHead(200, {
    'Content-Length': Buffer.byteLength(body),
    'Content-Type': 'text/plain',
  })
  .end(body);
```

Applying to my code

```ts
import { createServer, IncomingMessage, Server } from "http";

const server: Server = createServer((req: IncomingMessage, res) => {
  const method = req.method;
  const url = req.url;

  if (url == "/" && method == "GET") {
    res
      .writeHead(200, {
        "content-type": "text/plain",
      })
      .end("This is root route");
  }
});
```

```txt
server is running on port: 5000
```

![Server Response](../media/Mission%202-1778442194754.png)

## Now, what if we want to send the response as a JSON

```ts
import { createServer, IncomingMessage, Server } from "http";

const server: Server = createServer((req: IncomingMessage, res) => {
  const method = req.method;
  const url = req.url;

  // trying to send json
  if (url == "/" && method == "GET") {
    res
      .writeHead(200, {
        "content-type": "application/json", // json
      })
      .end({ mesage: "This is root route" });
  } else {
    res
      .writeHead(404, {
        "content-type": "text/plain",
      })
      .end("Route not found");
  }
});

server.listen(5000, () => {
  console.log("server is running on port: 5000");
});
```

> server crashes... why ?

Because `res.end()` cannot directly send JavaScript objects.

It only accepts:

- string
    
- Buffer
    

So the object must be converted into a string first.

---

Fix:

```ts
if (url == "/" && method == "GET") {
  res
    .writeHead(200, {
      "content-type": "application/json", // json
    })
    .end(JSON.stringify({ message: "This is root route" })); // stringified
}
```

---

```
%% in the browser %%

{
  "message": "This is root route"
}
```

---

## Now let's set route to another url (eg: products)

```ts
if (url == "/" && method == "GET") {
  res
    .writeHead(200, {
      "content-type": "application/json", // json
    })
    .end(JSON.stringify({ message: "This is root route" }));
} else if (url?.startsWith("/products")) {
  res
    .writeHead(200, {
      "content-type": "application/json", // json
    })
    .end(JSON.stringify({ message: "This is products route" }));
}
```

---

**startsWith:**  
Checks if a string begins with a specific substring.

So:

- `/products`
    
- `/products/1`
    
- `/products?sort=asc`
    

All match this condition.

---

**Why didn’t we mention methods in the product route?**

Because this check only cares about the URL path, not the action.

Right now:

- You are only matching routes structurally (`/`, `/products`)
    
- Not enforcing HTTP intent (`GET`, `POST`, etc.)
    

So `/products` works for any method (GET, POST, etc.) unless you explicitly restrict it.

If you want strict routing, you should do:

```ts
if (url === "/products" && method === "GET")
```

Otherwise:

- URL-based routing = path matching only
    
- Method-based routing = action control (REST behavior)

## Better file organization

As we can see, our previous `server.ts` has too many responsibilities in a single file.  
We'll face problems during debugging because:

- routing logic
    
- response handling
    
- controller logic
    
- business logic
    

... everything is mixed together.

This becomes harder to:

- debug
    
- scale
    
- maintain
    
- reuse
    

So, we have to distribute the tasks among separate files.

- `route.ts` → handles route matching
    
- `controller.ts` → handles request/response logic
    
- `service.ts` → handles business logic and data processing
    

```mermaid
flowchart LR

A[Client] --request--> B[route.ts]--> C[controller.ts] --> D[service.ts]
D --> C
C --response--> A
```

---

**What each layer does**

- `route.ts`
    
    - decides which route should run
        
    - acts like traffic control
        
- `controller.ts`
    
    - receives request from route
        
    - processes request/response handling
        
    - calls services if needed
        
- `service.ts`
    
    - contains core business logic
        
    - database operations
        
    - calculations
        
    - reusable backend logic
        

---

## 1. In `server.ts`

first we're gonna handle the routing logic in a separate file (`route.ts`)

```ts
import { createServer, IncomingMessage, Server } from "http";

const server: Server = createServer((req: IncomingMessage, res) => {
  const method = req.method;
  const url = req.url;

  // routing logic
  if (url == "/" && method == "GET") {
    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "This is root route" }));
  } else if (url?.startsWith("/products")) {
    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "This is product route" }));
  } else {
    res
      .writeHead(404, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "Route not found" }));
  }
});

server.listen(5000, () => {
  console.log("server is running on port: 5000");
});
```

---

## 2. Create `route.ts`

```ts
import type { IncomingMessage, ServerResponse } from "node:http";

const routeHandler = (req: IncomingMessage, res: ServerResponse) => {
  const method = req.method;
  const url = req.url;

  if (url == "/" && method == "GET") {
    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "This is root route" }));
  } else if (url?.startsWith("/products")) {
    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "This is product route" }));
  } else {
    res
      .writeHead(404, {
        "content-type": "application/json",
      })
      .end(JSON.stringify({ message: "Route not found" }));
  }
};

export default routeHandler;
```

Took the whole routing logic from `server.ts` and pasted it in `route.ts` inside a function named `routeHandler`, then called it inside `server.ts`.

`server.ts`

```ts
import { createServer, IncomingMessage, Server } from "http";
import routeHandler from "./route";

const server: Server = createServer((req: IncomingMessage, res) => {
  routeHandler(req, res);
});

server.listen(5000, () => {
  console.log("server is running on port: 5000");
});
```

Now:

- `server.ts` only creates the server
    
- `route.ts` handles route matching
    

Cleaner separation of responsibility.

---

## 3. Now, we can see, we're handling `/products` route inside the `route.ts` file

We need another abstraction layer for it.

Instead of handling everything inside routes, we move product-related response logic into a controller.

Open `Controller/product.controller.ts`

```ts
import type { ServerResponse } from "node:http";
import products from "../database/db.json";

export const productsController = (
  res: ServerResponse
) => {
  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Retrieved product data successfully",
        data: {
          products,
        },
      })
    );
};
```

---

Now the controller:

- handles response logic
    
- sends formatted response
    
- manages product-related request handling
    

This keeps `route.ts` cleaner and focused only on route matching.

## Reading files with fs module

We defined a product database inside `database/db.json`

```js
[
  {
    "id": 1,
    "name": "Earphone",
    "price": 250
  },
  {
    "id": 2,
    "name": "Wireless Mouse",
    "price": 150
  },
  {
    "id": 3,
    "name": "Keyboard",
    "price": 300
  },
  {
    "id": 4,
    "name": "Webcam",
    "price": 400
  },
  {
    "id": 5,
    "name": "USB-C Hub",
    "price": 200
  }
]
```

Now, we're gonna see how we can send this data to the client as a response via `products.controller.ts`

---

### Concept

There's a global object in Node.js called `process`, and a method under it called `cwd()`.

`cwd()` means:

- current working directory
    

It returns the absolute path of the currently running project.

Let's see a demo

```ts
export const readProducts = () => {
  console.log(process.cwd());
};
```

```txt
server is running on port: 5000
/home/username/Desktop/learning Node.js
```

---

Now, let's access and join the `db.json` path using `process.cwd()` and the `path` module.

`path.join()` safely combines file paths together in a cross-platform way.

```ts
import path from "path";

export const readProducts = () => {
  const filePath = path.join(process.cwd(), "./src/database/db.json");
  console.log(filePath);
};
```

```txt
server is running on port: 5000
/home/username/Desktop/learning Node.js/src/database/db.json

%% file accessed %%
```

---

Now, we're gonna read the data from the `db.json` file using the `fs` module

Doc: [https://nodejs.org/api/fs.html#fsreadfilesyncpath-options](https://nodejs.org/api/fs.html#fsreadfilesyncpath-options)

```ts
// from docs
import { readFileSync } from 'node:fs';

// macOS, Linux, and Windows
readFileSync('<directory>');
// => [Error: EISDIR: illegal operation on a directory, read <directory>]

// FreeBSD
readFileSync('<directory>'); // => <data>
```

implemented in `products.service.ts`

```ts
import fs from "fs";
import path from "path";

export const readProducts = () => {
  const filePath = path.join(process.cwd(), "./src/database/db.json");
  const products = fs.readFileSync(filePath);

  console.log(products);
};
```

```txt
server is running on port: 5000
<Buffer 5b 0a 20 20 7b 0a 20 20 20 20 22 69 64 22 3a 20 31 2c 0a 20 20 20 20 22 6e 61 6d 65 22 3a 20 22 45 61 72 70 68 6f 6e 65 22 2c 0a 20 20 20 20 22 70 72 ... 273 more bytes>
```

It's supposed to return the products... why are we getting this?

Because `fs.readFileSync()` returns a **Buffer** by default.

A Buffer is raw binary data stored in memory.

So we have to convert it into a readable string.

---

```ts
import fs from "fs";
import path from "path";

export const readProducts = () => {
  const filePath = path.join(process.cwd(), "./src/database/db.json");
  const products = fs.readFileSync(filePath);

  console.log(products.toString()); // fixed
};
```

or

```ts
import fs from "fs";
import path from "path";

export const readProducts = () => {
  const filePath = path.join(process.cwd(), "./src/database/db.json");

  const products = fs.readFileSync(filePath, "utf-8");

  console.log(products);
};
```

```txt
server is running on port: 5000
[
  {
    "id": 1,
    "name": "Earphone",
    "price": 250
  },
  {
    "id": 2,
    "name": "Wireless Mouse",
    "price": 150
  },
  {
    "id": 3,
    "name": "Keyboard",
    "price": 300
  },
  {
    "id": 4,
    "name": "Webcam",
    "price": 400
  },
  {
    "id": 5,
    "name": "USB-C Hub",
    "price": 200
  }
]
```

---

Now, finally we'll return actual JavaScript data instead of raw JSON string.

`products.service.ts`

```ts
import fs from "fs";
import path from "path";

export const readProducts = () => {
  const filePath = path.join(process.cwd(), "./src/database/db.json");

  const products = fs.readFileSync(filePath, "utf-8");

  return JSON.parse(products);
};
```

`products.controller.ts`

```ts
const products = readProducts();
```

---

We've done separation of concerns properly now.

- `service.ts`
    
    - handles file reading + data processing
        
- `controller.ts`
    
    - handles HTTP response logic
        

This is important because:

- business logic stays reusable
    
- controller stays cleaner
    
- debugging becomes easier
    
- code becomes scalable and maintainable
    

Now our backend layers have proper responsibility separation.

## Building GET method operation

Now, we're gonna build dynamic route handling for fetching a single product using its id.

Previously:

- `/products` → returned all products
    

Now:

- `/products/1` → should return only the product with id `1`
    

This is one of the most common REST API patterns.

---

Think like this:

We want to go to path `/products/1`

Here:

- `products` → route name
    
- `1` → dynamic product id
    

We can extract the id from the URL by splitting the string.

```mermaid
flowchart LR
A["/products/1"] --split('/')--> B["['', 'products', '1']"]
B --index: 2--> C["'1'"]
```

---

**products.controller.ts**

```ts
// got the id number from the url
const urlParts = url?.split("/");

// if route is /products/:id then extract the id, otherwise return null
const id =
  urlParts && urlParts[1] == "products"
    ? Number(urlParts[2])
    : null;

console.log(id);
```

---

Now, we got the id.

It's time to fetch a single product using that id.

> e.g: `/products/1` will return the product with `id: 1`

```ts
else if (method == "GET" && id != null) {
  const products = readProducts();

  // get the product that matches the requested id
  const product = products.find((p: Product) => p.id == id);

  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Fetched product successfully",
        data: product,
      }),
    );
}
```

---

**Final code**

```ts
import type { IncomingMessage, ServerResponse } from "node:http";
import { readProducts } from "../services/products.service";
import type { Product } from "../types/product.type";

export const productsController = (
  req: IncomingMessage,
  res: ServerResponse,
) => {
  const method = req.method;
  const url = req.url;

  // got the id number from the url
  const urlParts = url?.split("/");

  // if route is /products/:id then extract the id, otherwise return null
  const id =
    urlParts && urlParts[1] == "products"
      ? Number(urlParts[2])
      : null;

  // fetch all products
  if (url == "/products" && method == "GET") {
    const products = readProducts();

    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(
        JSON.stringify({
          message: "Retrieved product data successfully",
          data: products,
        }),
      );
  }

  // fetch single product
  else if (method == "GET" && id != null) {
    const products = readProducts();

    // find product by id
    const product = products.find((p: Product) => p.id == id);

    res
      .writeHead(200, {
        "content-type": "application/json",
      })
      .end(
        JSON.stringify({
          message: "Fetched product successfully",
          data: product,
        }),
      );
  }
};
```

## Parsing request body

we did set up basic routing, now we're gonna handle request data properly so we can send and receive actual content between client and server.

let's do a POST method to send a request to the server.

> [!clarification]  
> **In same device**  
> Client: My browser  
> Server: Node.js code  
> so, POST method just sends request from Browser to the console

`product.controller.ts`

```ts
else if (method == 'POST' && url == '/products'){

  const body = req.body;
  // Property 'body' does not exist on type 'IncomingMessage'.ts(2339)
  // IncomingMessage has no built-in body property in Node.js HTTP module

  res.writeHead(200,{
    "content-type":"application/json"
  }).end(
    JSON.stringify({
      message: "Post method message.",
    })
  )
}
```

the client sends a request to the server in chunks, so it doesn't arrive as a ready-made `body` object. Node.js gives us a stream instead, so we have to manually collect and parse it.

first, let's log the request.

```js
      chunkedEncoding: false,
      shouldKeepAlive: true,
	 // ... remaining internal request metadata
    },
    [Symbol(lastWriteQueueSize)]: 0,
    [Symbol(timeout)]: null,
    [Symbol(kBuffer)]: null,
    [Symbol(kBufferCb)]: null,
    [Symbol(kBufferGen)]: null,
    [Symbol(shapeMode)]: true,
    [Symbol(kCapture)]: false,
    [Symbol(kSetNoDelay)]: true,
    [Symbol(kSetKeepAlive)]: false,
    [Symbol(kSetKeepAliveInitialDelay)]: 0,
    [Symbol(kBytesRead)]: 0,
    [Symbol(kBytesWritten)]: 0
  },
  _consuming: false,
  _dumped: false,
  [Symbol(shapeMode)]: true,
  [Symbol(kCapture)]: false,
  [Symbol(kHeaders)]: {
    host: 'localhost:5000',
    connection: 'keep-alive',
    'cache-control': 'max-age=0',
    'sec-ch-ua': '"Google Chrome";v="147", "Not.A/Brand";v="8", "Chromium";v="147"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Linux"',
    'upgrade-insecure-requests': '1',
    user-agent: 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36',
    accept: 'text/html,application/xhtml+xml',
    'accept-encoding': 'gzip, deflate, br, zstd',
    'accept-language': 'en-US,en;q=0.9'
  },
  [Symbol(kHeadersCount)]: 30
}
```

this shows a full request object. there is no `body` yet because HTTP request bodies arrive as a stream.

node gives us an `on` method on the request object. we use it to listen to events and collect data.

> Adds a listener function to an event. Each time that event happens, the listener runs.

**Open file `utils/parseBody.ts`**

```ts
import type { IncomingMessage } from "node:http";

export const parseBody = (req: IncomingMessage): Promise<any> => {

  return new Promise((resolve, reject) => {
    
    let body = "";

    req.on("data", (chunk) => {
      body += chunk;
    });

    req.on("end", () => {
      try {
        resolve(JSON.parse(body));
      } catch (error) {
        reject(error);
      }
    });
  });
};
```

since this returns a promise, we need `await` to get the final value. that also means the function calling it must be `async`.

> [!NOTE] Promise flow  
> A promise has three states:  
> pending → initial state while async work is running  
> fulfilled → operation completed successfully, `resolve()` called  
> rejected → operation failed, `reject()` called

`products.controller.ts`

```ts
else if (method == "POST" && url == "/products") {

  const body = await parseBody(req);
  console.log(body);

  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Post method message.",
      }),
    );
}
```

Now, usually requests are sent from frontend applications. since there is no frontend here, we use tools like Postman or Thunder Client to manually send HTTP requests.

> Postman / Thunder Client are API testing tools used to send HTTP requests without building a frontend.

I'm using Thunder Client.

Go to Thunder Client → enter `http://localhost:5000/products` → send a GET request.

Now switch method to POST.

Select POST → Body → JSON → enter data.

Now check the console:

```
server is running on port: 5000

{
  "id": 6,
  "name": "Banana",
  "price": 5
}
```

## Creating clean POST API

Now, as we sent a POST request, let's store it in the DB.

open `controller/product.controller.ts`

```ts
// this portion handles the POST method
else if (method == "POST" && url == "/products") {

  const body = await parseBody(req);
  console.log(body);

  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product created successfully.",
      }),
    );
}
```

---

### 1. Create new product object

```ts
else if (method == "POST" && url == "/products") {

  const body = await parseBody(req);

  // new product, id is auto-generated
  const newProduct = {
    id: Date.now(),
    ...body
  };

  console.log(newProduct);

  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product created successfully.",
      }),
    );
}
```

```
server is running on port: 5000
{ id: 1779038895412, name: 'Banana', price: 5 }
```

---

### 2. Insert product into DB (array-based DB)

The DB here is just an array stored in a file.

So the flow is:  
read file → get array → push new product → save back

`product.controller.ts`

```ts
const products = readProducts();

const newProduct = {
  id: Date.now(),
  ...body
};

products.push(newProduct);
console.log(products);
```

```
[
  { "id": 1, "name": "Apple", "price": 2 },
  { "id": 2, "name": "Banana", "price": 1 },
  { "id": 3, "name": "Orange", "price": 3 },
  { "id": 4, "name": "Mango", "price": 5 },
  {
    "id": 1779041033419,
    "name": "Banana",
    "price": 5
  }
]
```

At this point, the product is only updated in memory, not in the actual DB file.

---

### 3. Persist data to file (DB write service)

We need a service function that rewrites the DB file every time data changes.

`services/product.service.ts`

```ts
export const insertProduct = (payload: any) => {
// Payload = the JSON body inside the POST request (the actual data you send to the server).
  fs.writeFileSync(filePath, JSON.stringify(payload));
};
```

---

### Important behavior

Writing to a file here is destructive overwrite, not append.

So the correct flow is:  
old array + new product → combined array → write full array again

```mermaid
flowchart LR
A[Old data] --> C[Combined data]
B[New product] --> C
C --> D[Write to file]
D --> E[db.json updated]
```

---

### 4. Final controller integration

```ts
products.push(newProduct);
insertProduct(products);
```

---

### Result after POST request

```json
[
  { "id": 1, "name": "Apple", "price": 2 },
  { "id": 2, "name": "Banana", "price": 1 },
  { "id": 3, "name": "Orange", "price": 3 },
  { "id": 4, "name": "Mango", "price": 5 },
  { "id": 5, "name": "Grapes", "price": 4 },
  { "id": 6, "name": "Pineapple", "price": 6 },
  { "id": 7, "name": "Watermelon", "price": 7 },
  {
    "id": 1779042052811,
    "name": "Tangerine",
    "price": 5
  }
]
```

`db.json` is now rewritten with the updated array including the new product.

## PUT method implementation

PUT is used to fully update or replace an existing resource on the server with new data.  
Usually, you send the complete updated object, not just the changed fields.

---

### 1. Finding product index using id

`product.controller.ts`

```ts
else if (method == "PUT" && id != null) {

  const products: Product[] = readProducts();

  const index = products.findIndex((p) => p.id == id);

  console.log(products[index]);
  console.log(index);
}
```

```id="j5mxzr"
server is running on port: 5000

{ id: 3, name: 'Orange', price: 3 }

index: 2
```

We found the target product. now we can update it.

---

### 2. Updating the product in memory

```ts
else if (method == "PUT" && id != null) {

  const products: Product[] = readProducts();

  const index = products.findIndex((p) => p.id == id);

  const body = await parseBody(req);

  products[index] = {
    id: products[index]?.id,
    ...body,
  };

  console.log(products);
}
```

---

### 3. Check existing data first

Send a GET request.

```json
{
  "message": "Retrieved product data successfully",
  "data": [
    {
      "id": 1,
      "name": "Apple",
      "price": 2
    },
    {
      "id": 2,
      "name": "Banana",
      "price": 1
    },
    {
      "id": 3,
      "name": "Orange",
      "price": 3
    },
    {
      "id": 4,
      "name": "Mango",
      "price": 5
    }
  ]
}
```

Let's update the product with `id: 3`

Thunder Client → search `/products/3` → PUT → Body → type updated data → Send

```js
server is running on port: 5000

[
  { id: 1, name: 'Apple', price: 2 },
  { id: 2, name: 'Banana', price: 1 },

  // updated product
  { id: 3, name: 'Tangerine', price: 12 },

  { id: 4, name: 'Mango', price: 5 },
  { id: 5, name: 'Grapes', price: 4 },
  { id: 6, name: 'Pineapple', price: 6 },
  { id: 7, name: 'Watermelon', price: 7 },
  { id: 8, name: 'Banana', price: 5 },
  { id: 9, name: 'Tangerine', price: 12 }
]
```

The data is updated in memory, but if we check `db.json`, it is still unchanged.

So, after updating the array, we also have to rewrite the DB file.

---

### 4. Persist updated data into DB

```ts
else if (method == "PUT" && id != null) {

  const products: Product[] = readProducts();

  const index = products.findIndex((p) => p.id == id);

  const body = await parseBody(req);

  products[index] = {
    id: products[index]?.id,
    ...body,
  };

  // rewrite DB file
  insertProduct(products);

  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product updated successfully.",
        data: products,
      }),
    );
}
```

**This time `db.json` will also be updated.**

> [!CAUTION]  
> Remember to handle edge cases like:
> 
> - `id < 0`
>     
> - invalid id types (`abc`, `null`, `undefined`)
>     
> - product not found
>     
> - empty request body
>     
> - missing fields
>     
> - duplicate data
>     
> - invalid JSON body
>     
> 
> Backend APIs should always validate data before processing requests.
## DELETE method implementation

DELETE is used to remove a resource from the server.

Flow:  
find product → remove from array → rewrite DB → send response

```ts
else if (method == "DELETE" && id != null) {

  // 1. get product index
  const products: Product[] = readProducts();

  const index = products.findIndex(
    (p: Product) => p.id == id
  );

  // 2. remove product from array
  products.splice(index, 1);

  // 3. send response
  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product deleted successfully",
        data: products,
      }),
    );

  // 4. rewrite DB
  insertProduct(products);
}
```

---

### How `splice()` works

```ts
array.splice(startIndex, deleteCount)
```

Example:

```ts
products.splice(index, 1);
```

Meaning:

- start deleting from `index`
    
- delete `1` item
    

---

### Example

Before delete:

```json
[
  { "id": 1, "name": "Apple", "price": 2 },
  { "id": 2, "name": "Banana", "price": 1 },
  { "id": 3, "name": "Orange", "price": 3 }
]
```

Request:

```http
DELETE /products/2
```

After delete:

```json
[
  { "id": 1, "name": "Apple", "price": 2 },
  { "id": 3, "name": "Orange", "price": 3 }
]
```

`db.json` will also be rewritten without the deleted product.

> [!CAUTION]  
> Handle edge cases:
> 
> - invalid id
>     
> - `id < 0`
>     
> - product not found
>     
> - `findIndex()` returning `-1`
>     
> 
> If `splice(-1, 1)` runs accidentally, it will delete the last item of the array.

## Edge case handling

---

1. **Getting a single product by id:** What if that product doesn't exist?
    

```ts
if (product == null) {
  res
    .writeHead(404, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product not found.",
        data: null,
      }),
    );
}
```

---

2. **Updating / deleting product:** What if index is invalid?
    

```ts
if (index < 0) {
  res
    .writeHead(404, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product doesn't exist",
        data: null,
      }),
    );
}
```

---

### Important edge case logic

- `find()` → returns `null` / `undefined` if not found
    
- `findIndex()` → returns `-1` if not found
    

So always check:

- `if (product == null)`
    
- `if (index < 0)`
    

before doing:

- update (`products[index] = ...`)
    
- delete (`splice(index, 1)`)
    

Otherwise you'll silently corrupt data (like deleting last element when index = -1).


## Optimizing `sendResponse`

We're repeating the same response pattern too much, which goes against the DRY principle (Don’t Repeat Yourself).

---

Instead of writing this every time:

```ts
res
  .writeHead(200, {
    "content-type": "application/json",
  })
  .end(
    JSON.stringify({
      message: "Product deleted successfully",
      data: products,
    }),
  );
```

---

We create a reusable helper function: `sendResponse`

File: `utils/sendResponse.ts`

---

### 1. Initial idea (incorrect version)

```ts
export const sendResponse = () => {
  res
    .writeHead(200, {
      "content-type": "application/json",
    })
    .end(
      JSON.stringify({
        message: "Product deleted successfully",
        data: products,
      }),
    );
};
```

This doesn’t work because:

- `res` is not in scope
    
- `products` is not in scope
    
- message/status are hardcoded
    
- not reusable
    

---

### 2. Correct generic implementation

We pass everything as parameters so the function becomes reusable.

```ts
import type { ServerResponse } from "node:http";

export const sendResponse = (
  res: ServerResponse,
  success: boolean,
  statusCode: number,
  message: string,
  data?: any,
) => {
  const response = {
    success,
    message,
    data,
  };

  res
    .writeHead(statusCode, {
      "content-type": "application/json",
    })
    .end(JSON.stringify(response));
};
```

---

### Why this works better

- reusable across GET / POST / PUT / DELETE
    
- consistent response structure
    
- avoids duplication
    
- easier to maintain (change format once, updates everywhere)
    

---

### 3. Usage example

```ts
try {
  sendResponse(
    res,
    true,
    200,
    "Products retrieved successfully",
    products,
  );
} catch (err) {
  sendResponse(
    res,
    false,
    500,
    "Something went wrong",
  );
}
```

---

### Result

Now every controller uses a single standardized response format instead of repeating `writeHead + JSON.stringify + end` everywhere.

## Working with Environment-base configuration

%% what is the use of .env %%

1. Install dotenv
   ```
   npm i dotenv
   ```
2. Open file `.env` in the root and let's put our port in it
   `.env`
   ```ts
   POST = 5000
   ```
3. create file `./src/config/index.ts`
   Get the path of the .env file here
   ```ts
	import dotenv from "dotenv";
	import path from "node:path";
	
	dotenv.config({
	  path: path.resolve(process.cwd(), ".env"),
	});
   ```
4. Got the path, now get the data from it (e.g : PORT)

	```ts
	import dotenv from "dotenv";
import path from "node:path";

dotenv.config({
  path: path.resolve(process.cwd(), ".env"),
});


const config = {
    port:process.env.PORT,
}

export default config
	```

Now, replace every single explicit port number with `config.port`
**Data hidden successfully**

```ts
import { createServer, IncomingMessage, Server } from "http";
import routeHandler from "./routes/route";
import config from "./config";

const server: Server = createServer((req: IncomingMessage, res) => {
    routeHandler(req,res);
});

server.listen(config.port, () => {
  console.log("server is running on port:",config.port);
});
```

#  Module 7: Express js server architecture and Database integration

## Create server with express js and ts

1. Create folder --> initialize --> get ts dependencies --> get config
   ```bash
   npm init --y 
   npm i -D typescript
   npx tsc --init
   ```
2. Make some edits in tsconfig.json
   ```js
   "rootDir": "./src",
    "outDir": "./dist",
    
    "module": "esnext",
    "target": "esnext",
    "types": ["node"],
    
    // "jsx": "react-jsx",
   ```

3. As mentioned in tsconfig.json, create folders `./src/server.ts`

	**Now, we start coding in express.js**

	Doc: https://expressjs.com/en/


**First code:** Just go with the flow (doc + vscode suggestions)

```js
import express from 'express';
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

Include this is the `package.json/scripts`

```js
"dev":"tsx watch ./src/server.ts",
```

## Understanding express request and response 

The types in express are defined differently than node.js

Check the file : `node_modules/@types/express`

```js
import express, { type Application, type Request, type Response } from 'express';
const app: Application = express();
const port = 3000;

// alot simpler than node.js
app.get('/', (req: Request, res: Response) => {
  res.status(200).json({
    "message":"Express server",
    "author":"Mahmud"
  })
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

## Setting Up Postgres with Neon Serverless Cloud

Look at this code

```ts
import express, { type Application, type Request, type Response } from "express";

const app: Application = express();
const port = 3000;

app.use(express.json());

app.get("/", (req: Request, res: Response) => {
  res.status(200).json({
    message: "Express server",
    author: "Mahmud",
  });
});

app.post("/", async (req: Request, res: Response) => {
  const body = req.body;

  res.status(201).json({
    message: "created",
    data: body,
    password: 12345,
  });
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

Now, the client can see the password, which is not correct.

So we should destruct the whole object and avoid sending the password in the response.

```ts
import express, { type Application, type Request, type Response } from "express";

const app: Application = express();
const port = 3000;

app.use(express.json()); // parses incoming JSON request body into req.body

app.get("/", (req: Request, res: Response) => {
  res.status(200).json({
    message: "Express server",
    author: "Mahmud",
  });
});

app.post("/", async (req: Request, res: Response) => {
  const { name, email, password } = req.body; // destructured

  res.status(201).json({
    message: "created",
    data: { name, email },
  });
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

**Response:**

```json
{"message":"created","data":{"name":"Mahmud","email":"abdullahmahmud01798@gmail.com"}}
```

Password is not sent in the response now.

Now, let's move to the main part: instead of storing the response in a local JSON file (like we did earlier using `writeFileSync` in Node.js), we will store it using a cloud service called NeonDB.

NeonDB is a serverless PostgreSQL database platform that lets you create and manage Postgres databases in the cloud without handling server setup or maintenance.

Link:

NeonDB console → create project → connect → get the connection string

Now we need a bridge between our project and NeonDB → PostgreSQL driver

```bash
npm install pg
```

`server.ts`

```ts
import express, {
  type Application,
  type Request,
  type Response,
} from "express";
import { Pool } from "pg"; // PostgreSQL client

const app: Application = express();
const port = 3000;

app.use(express.json());

const pool = new Pool({
  // paste connection string from NeonDB here
  connectionString:
    "postgresql://neondb_owner:npg_ZItFPXK28EqU@ep-wandering-shape-aoxuq2q5-pooler.c-2.ap-southeast-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require",
});

app.get("/", (req: Request, res: Response) => {
  res.status(200).json({
    message: "Express server",
    author: "Mahmud",
  });
});

app.post("/", async (req: Request, res: Response) => {
  const { name, email, password } = req.body; // destructured

  res.status(201).json({
    message: "created",
    data: { name, email },
  });
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

The Express server is now correctly handling request data by using `express.json()` to parse incoming JSON payloads and destructuring sensitive fields to prevent exposing private information like passwords in API responses.

Instead of storing data locally (e.g., using `writeFileSync`), the project is moving toward a production-grade approach by integrating a cloud database solution, NeonDB, which is based on PostgreSQL.

With the addition of the `pg` library and a configured connection pool, the backend is now prepared to communicate with NeonDB securely and efficiently. This setup enables scalable, serverless database operations while keeping the application structure clean and maintainable.

Overall, the system transitions from a basic local storage model to a cloud-backed architecture with proper API data handling and improved security practices.

## Explore SQL datatype

SQL data types define the kind of data a column can store in a database table. They ensure data consistency and help the database understand how to store, process, and validate values.

---

### 1. Numeric Types

Used for numbers.

- `INT` / `INTEGER` → whole numbers (e.g., 1, 100, -5)
    
- `SMALLINT` → smaller range integers
    
- `BIGINT` → very large integers
    
- `DECIMAL(p, s)` / `NUMERIC(p, s)` → exact decimal values (e.g., 10.25)
    
- `FLOAT` / `REAL` / `DOUBLE PRECISION` → approximate decimal numbers
    

---

### 2. String (Text) Types

Used for text data.

- `CHAR(n)` → fixed-length string
    
- `VARCHAR(n)` → variable-length string (most commonly used)
    
- `TEXT` → large or unlimited text
    

---

### 3. Boolean Type

- `BOOLEAN` → stores `TRUE` or `FALSE`
    

---

### 4. Date & Time Types

Used for storing time-related values.

- `DATE` → date only (YYYY-MM-DD)
    
- `TIME` → time only
    
- `TIMESTAMP` → date + time
    
- `TIMESTAMPTZ` → timestamp with timezone (common in PostgreSQL/NeonDB)
    

---

### 5. Binary Types

Used for raw data like files or images.

- `BYTEA` (PostgreSQL) → binary data storage
    

---

### 6. JSON Types (important for modern apps)

- `JSON` → stores JSON as text
    
- `JSONB` → optimized binary JSON (faster querying, preferred in PostgreSQL/NeonDB)
    

---

### Example Table

|id (SERIAL)|student_id (INTEGER)|name (VARCHAR(50))|dob (DATE)|is_enrolled (BOOLEAN)|
|---|---|---|---|---|
|1|1023|Emma|2001-06-12|true|
|2|2045|Liam|2000-09-30|false|

---

We'll learn the other data types gradually: UUID, ARRAY, ENUM, INTERVAL, JSONB, SERIAL, BIGSERIAL, MONEY, SMALLINT.

## Executing Pool and Creating Table

Previously we created a `Pool` using the `pg` library to connect our Express application with the PostgreSQL database (NeonDB). This pool acts as a bridge between our server and the database, allowing us to run SQL queries efficiently without opening a new connection every time.

Now we will use this pool to execute queries and create a table in the database.

```ts
const pool = new Pool({
  // pass connectionString from NeonDB here
  connectionString:
    "postgresql://neondb_owner:npg_ZItFPXK28EqU@ep-wandering-shape-aoxuq2q5-pooler.c-2.ap-southeast-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require",
});

const initDB = async () => {
  try {
    await pool.query(
      // we'll form the table here
      // await: because the connection between project and DB takes time (fixed comment)
    );
  } catch (error) {
    console.log(error);
  }
};
```

We don't need to memorize anything.  
We'll read the docs: [https://neon.com/docs/data-types/array](https://neon.com/docs/data-types/array)

```ts
const initDB = async () => {
  try {
    await pool.query(
      // PostgreSQL
      // create table only if the table doesn't exist, don't create it again if it already exists
      `CREATE TABLE IF NOT EXISTS users (  
          id SERIAL PRIMARY KEY,
          name VARCHAR(20),
          email VARCHAR(50) NOT NULL,
          password VARCHAR(20) NOT NULL,
          is_active BOOLEAN DEFAULT true,
          age INT,

          created_at TIMESTAMP DEFAULT NOW(),
          updated_at TIMESTAMP DEFAULT NOW()
        );
      `,
    );
  } catch (error) {
    console.log(error);
  }
};

initDB(); // function called
```

Database created. Now go to NeonDB project dashboard → tables.

![Pasted-image-20260522201947.png](../media/Pasted-image-20260522201947.png)

We can see a table called `users`, and the full table structure has been created successfully.

## Creating Our First User with POST Method

Previously, we connected our Express server with NeonDB and created a `users` table using PostgreSQL queries.

Now we're going to insert real user data into the database using the `POST` method.

Go to the client (`Postman`) and send a data request using the `POST` method. First, we have to destructure the incoming data from `req.body`.

Doc: [W3Schools SQL INSERT INTO](https://www.w3schools.com/sql/sql_insert.asp?utm_source=chatgpt.com)

`server.ts`

This code receives user data from the client and inserts it into the `users` table using an SQL query.  
`pool.query()` is used to execute PostgreSQL commands from the backend.  
`$1, $2, $3, $4` are placeholders that safely insert dynamic values and help prevent SQL injection attacks.  
`RETURNING *` returns the newly inserted row after the insert operation is completed.

```ts
app.post("/", async (req: Request, res: Response) => {
  const { name, age, email, password } = req.body; // destructured

  // $1, $2, $3, $4 are placeholders for dynamic values
  const result = await pool.query(
    `
    INSERT INTO users (name, age, email, password)
    VALUES ($1, $2, $3, $4) RETURNING * 
    `,
    [name, age, email, password]
    // * = all columns
  );

  res.status(201).json({
    message: "created",
    data: { name, age, email },
  });
});
```

Now send a request using the `POST` method.  
The data will be stored in the database table.

![Pasted-image-20260522222021.png](../media/Pasted-image-20260522222021.png)

### Precaution

The sequence of values inside the array:

```ts
[name, age, email, password]
```

must always match the sequence of table columns:

```sql
(name, age, email, password)
```

Otherwise, the wrong data may be inserted into the wrong columns.

**Issue:** We can enter duplicate values multiple times ( e.g.: 3 user with the same email address )

**Solve**

```ts
email VARCHAR(50) NOT NULL UNIQUE,
```


We wrap the database operation inside a `try-catch` block to handle runtime and database errors safely instead of crashing the server.

This version also adds basic input validation and handles PostgreSQL-specific errors like duplicate entries.

```ts
app.post("/", async (req: Request, res: Response) => {
  try {
    const { name, age, email, password } = req.body;

    // basic validation
    if (!name || !email || !password) {
      return res.status(400).json({
        message: "Name, email and password are required",
      });
    }

    const result = await pool.query(
      `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
      [name, age, email, password],
    );

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    // PostgreSQL unique violation error (duplicate email)
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
});
```

### Key Points

- `try` → runs database insert safely
    
- `catch` → handles unexpected failures
    
- Validation prevents empty required fields from entering DB
    
- `23505` → PostgreSQL error code for **unique constraint violation** (e.g., duplicate email)
    
- `RETURNING *` → returns the newly inserted user row
    
- `result.rows[0]` → fetches the inserted record from PostgreSQL response
## Getting All Users and Single User with Params

Doc: [https://www.w3schools.com/postgresql/postgresql_fetch_data.php](https://www.w3schools.com/postgresql/postgresql_fetch_data.php)

---

### Create User (POST)

```ts
app.post("/api/users", async (req: Request, res: Response) => {
  try {
    const { name, age, email, password } = req.body;

    // basic validation
    if (!name || !email || !password) {
      return res.status(400).json({
        message: "Name, email and password are required",
      });
    }

    const result = await pool.query(
      `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
      [name, age, email, password],
    );

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    // duplicate email
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
      error,
      data: null,
    });
  }
});
```

---

### Get All Users (GET)

```ts
app.get("/api/users", async (req: Request, res: Response) => {
  try {
    const result = await pool.query(`
      SELECT * FROM users
    `);

    res.status(200).json({
      success: true,
      message: "Users retrieved successfully",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error,
      data: null,
    });
  }
});
```

---

### Get Single User (GET by ID)

```ts
app.get("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `
      SELECT * FROM users
      WHERE id = $1
      `,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        message: "User not found",
      });
    }

    res.json({
      message: "User found",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      message: error.message,
    });
  }
});
```

---

### Update User (PUT)

```ts
app.put("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  const { name, age, email, password, is_active } = req.body;

  try {
    const result = await pool.query(
      `
      UPDATE users
      SET name = $1,
          age = $2,
          email = $3,
          password = $4,
          is_active = $5
      WHERE id = $6
      RETURNING *
      `,
      [name, age, email, password, is_active, id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found",
      });
    }

    res.status(200).json({
      success: true,
      message: "User updated successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error,
    });
  }
});
```

---

### Partial Update Fix (COALESCE)

Problem: If only some fields are sent, others become `undefined` → update breaks.

Solution: `COALESCE`

```ts
const result = await pool.query(
  `
  UPDATE users
  SET 
    name = COALESCE($1, name),
    age = COALESCE($2, age),
    email = COALESCE($3, email),
    password = COALESCE($4, password),
    is_active = COALESCE($5, is_active)
  WHERE id = $6
  RETURNING *
  `,
  [name, age, email, password, is_active, id],
);
```

Meaning: if new value is `null/undefined`, keep old value.

---

### Delete User (DELETE)

```ts
app.delete("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `
      DELETE FROM users
      WHERE id = $1
      RETURNING *
      `,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found",
      });
    }

    res.status(200).json({
      success: true,
      message: "User deleted successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
});
```

## Environment based configuration

Create .env

```env
CONNECTIONSTRING = 'connectionString here'
PORT = 3000
```

```bash
npm i dotenv
```

create `/src/config/index.ts`
```ts
import dotenv from 'dotenv'
import path from 'path'

dotenv.config({
    path: path.join(process.cwd(),".env")
})

const config = {
    connectionString: process.env.CONNECTIONSTRING as string,
    port: process.env.PORT
};

export default config;
```


# Module 8: Advanced backend structuring and user authentication

## Software design pattern

Software design patterns are standard ways to structure backend code. They help keep code clean, scalable, and easier to debug. Instead of dumping everything in one file, logic is separated into layers.

Main idea:

- Separate responsibilities
    
- Reuse logic
    
- Make scaling easier
    

---

## 1. MVC (Model - View - Controller)

MVC is a layered architecture pattern where responsibilities are split into three parts.

### What’s inside each layer

#### Model

- Handles database structure and queries
    
- Defines schema (table structure)
    
- Communicates directly with DB
    

#### Controller

- Handles request and response logic
    
- Validates input
    
- Calls services/models
    

#### View

- UI layer (not used in pure backend APIs)
    
- In backend APIs, "view" is usually JSON response
    

Extra parts:

- Routes → maps endpoints to controllers
    
- Interfaces → defines TypeScript structure (types)
    
- Services → business logic layer (optional but common)
    

---

### MVC Flow

```mermaid
flowchart LR
    %% Style Definitions
    classDef clientClass fill:#43A047,stroke:#2E7D32,stroke-width:2px,color:#fff;
    classDef codeClass fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff;
    classDef dbClass fill:#E0F7FA,stroke:#00ACC1,stroke-width:2px,color:#006064;

    %% Nodes
    Client((Client)):::clientClass
    Route(route.ts):::codeClass
    Controller(controller.ts):::codeClass
    Service(service.ts):::codeClass
    DB[("Database")]:::dbClass

    %% Request Flow
    Client -- "req" --> Route
    Route -- "req" --> Controller
    Controller -- "req" --> Service
    Service -- "req" --> DB

    %% Response Flow
    DB --> Service
    Service -- "res" --> Controller
    Controller -- "{ success, message, data }" --> Client
```

---

## 2. Modular Pattern

Modular architecture splits the project by feature instead of layers.

Each feature (Student, Admin, etc.) has its own complete internal structure.

### Example Project Structure (Modular)

```
Student
    student.interface.ts   // defines types (name, email, etc.)
    student.routes.ts     // defines API endpoints (/create, /get)
    student.model.ts      // database schema + queries
    student.controller.ts // request/response handler
    student.service.ts    // business logic (core rules)

Admin
    admin.interface.ts
    admin.routes.ts
    admin.model.ts
    admin.controller.ts
    admin.service.ts
```

### What each file does

- **interface.ts** → defines data shape (TypeScript types)
    
- **routes.ts** → connects endpoints to controllers
    
- **model.ts** → DB schema + raw queries
    
- **controller.ts** → handles req/res logic
    
- **service.ts** → business logic (core rules, reusable logic)
    

---

## 3. MVC Project Structure Example

```
src
│
├── models
│     user.model.ts        // DB schema + queries
│
├── controllers
│     user.controller.ts   // request handling
│
├── routes
│     user.routes.ts       // API endpoints
│
├── services
│     user.service.ts      // business logic
│
├── interfaces
│     user.interface.ts    // type definitions
│
└── app.ts
```

---

## Modular vs MVC

### Modular (Feature-based)

- Groups everything by feature (student/admin)
    
- Easier scaling for large apps
    
- Cleaner when many modules exist
    

### MVC (Layer-based)

- Groups by responsibility (model/controller/service)
    
- Easier for beginners
    
- Better for small/medium apps
    

---

### Conclusion

- Small projects → MVC is simpler and faster to manage
    
- Large scalable systems → Modular is better
    
- Real-world backend systems → mostly **Modular + Service layer hybrid**
    
---

### DRY, Fat Model / Thin Controller

### Short Intro

- **DRY (Don’t Repeat Yourself)** → avoid duplicating logic
    
- **Fat Model** → model handles most logic (DB + business rules)
    
- **Thin Controller** → controller only handles request/response, nothing heavy
    

Goal:

- Controllers stay clean
    
- Logic stays reusable and centralized
    

---

### Summary

Good backend design is about separating concerns:

- Routes → entry point
    
- Controller → request handler
    
- Service → business logic
    
- Model → database layer
    
- Interface → type safety
    

This structure makes code scalable, maintainable, and production-ready.


## Cleaning up and creating app.ts

---

### Initial state

```
src/
├─ server.ts
└─ (everything inside server.ts: routes + DB + config mixed)
```

`server.ts`

```ts
import express, {
  type Application,
  type Request,
  type Response,
} from "express";
import { Pool } from "pg";

const app: Application = express();
const port = 3000;

app.use(express.json());

// DB connection directly inside server file
const pool = new Pool({
  connectionString: "postgresql://....",
});

// routes (all mixed in one file)
app.get("/");
app.post("/api/users");
app.get("/api/users");
app.get("/api/users/:id");
app.put("/api/users/:id");
app.delete("/api/users/:id");

app.listen(port);
```

---

## Final state

### What is done

- Separated configuration (port, env variables)
    
- Separated database connection
    
- Moved Express app to `app.ts`
    
- Kept server startup in `server.ts`
    
- Clean architecture (config → db → app → server)
    
- Removed tight coupling between DB and routes
    
- Improved scalability and maintainability
    

---

### File structure

```
src/
├─ config/
│  └─ index.ts
├─ db/
│  └─ index.ts
├─ app.ts
└─ server.ts
```

---

### File responsibilities

- **config/index.ts** → environment variables (port, DB url, secrets)
    
- **db/index.ts** → PostgreSQL pool + DB connection
    
- **app.ts** → Express app + middleware + routes
    
- **server.ts** → starts server + initializes DB
    

---

## `app.ts`

```ts
import express, {
  type Application,
  type Request,
  type Response,
} from "express";

import config from "./config";
import { pool } from "./db";

const app: Application = express();
const port = config.port;

// middleware
app.use(express.json());
app.use(express.text());

// home route
app.get("/", (req: Request, res: Response) => {
  res.status(200).json({
    message: "Express server running",
    author: "Mahmud",
  });
});

// create user
app.post("/api/users", async (req: Request, res: Response) => {
  try {
    const { name, age, email, password } = req.body;

    if (!name || !email || !password) {
      return res.status(400).json({
        message: "Name, email and password are required",
      });
    }

    const result = await pool.query(
      `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
      [name, age, email, password],
    );

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
});

// get all users
app.get("/api/users", async (req: Request, res: Response) => {
  try {
    const result = await pool.query(`SELECT * FROM users`);

    res.status(200).json({
      success: true,
      message: "Users retrieved successfully.",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      data: null,
    });
  }
});

// get single user
app.get("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `SELECT * FROM users WHERE id = $1`,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        message: "User not found",
      });
    }

    res.json({
      message: "User found",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      message: error.message,
    });
  }
});

// update user
app.put("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;
  const { name, age, email, password, is_active } = req.body;

  try {
    const result = await pool.query(
      `
      UPDATE users
      SET 
        name = COALESCE($1, name),
        age = COALESCE($2, age),
        email = COALESCE($3, email),
        password = COALESCE($4, password),
        is_active = COALESCE($5, is_active),
        updated_at = NOW()
      WHERE id = $6
      RETURNING *
      `,
      [name, age, email, password, is_active, id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found",
      });
    }

    res.status(200).json({
      success: true,
      message: "User updated successfully.",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
});

// delete user
app.delete("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `DELETE FROM users WHERE id = $1 RETURNING *`,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found.",
      });
    }

    res.status(200).json({
      success: true,
      message: "User deleted successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
});

export default app;
```

---

## `server.ts`

```ts
import app from "./app";
import config from "./config";
import { initDB } from "./db";

const main = async () => {
  await initDB();

  app.listen(config.port, () => {
    console.log(`Server running on port ${config.port}`);
  });
};

main();
```

### db/index.ts

```ts
import { Pool } from "pg";
import config from "../config";

// postgres pool
export const pool = new Pool({
  // from neoDB
  connectionString: config.connectionString,
});

// create table
export const initDB = async () => {
  try {
    await pool.query(
      // postgreSQL
      `CREATE TABLE IF NOT EXISTS users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(20),

        age INT,

        email VARCHAR(50) NOT NULL UNIQUE,

        password TEXT NOT NULL,

        is_active BOOLEAN DEFAULT true,

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      );
    `,
    );

    console.log("Database initialized successfully.");
  } catch (error) {
    console.log(error);
  }
};
```

---

## Key improvement

- server.ts → only starts server
    
- app.ts → only API logic
    
- db.ts → only database logic
    
- config → only environment values
    

This is the base structure used in real production backend systems.

## Implementing Modular Pattern

---

## After implementing modular pattern

```text
src/
├─ modules/
│  └─ users/
│     ├─ users.routes.ts
│     ├─ users.controller.ts
│     ├─ users.service.ts
│     └─ users.interface.ts
│     
├─ db/
├─ config/
├─ app.ts
└─ server.ts
```

---

## 1. Handling routes

### What’s done

- Routes are moved out of `app.ts`
    
- `app.ts` only mounts feature routers
    
- All `/api/users` endpoints are grouped under one router
    
- Clean separation between app setup and business logic routing
    

---

### `app.ts`

```ts
app.use('/api/users', userRouter); // routing handled in users.routes.ts
```

---

### `users.routes.ts`

```ts
import { Router, type Request, type Response } from "express";
import { pool } from "../../db";

const router = Router();

router.post("/", async (req: Request, res: Response) => {
  try {
    const { name, age, email, password } = req.body;

    if (!name || !email || !password) {
      return res.status(400).json({
        message: "Name, email and password are required",
      });
    }

    const result = await pool.query(
      `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
      [name, age, email, password],
    );

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
});

// get all users
router.get("/", async (req: Request, res: Response) => {
  try {
    const result = await pool.query(`SELECT * FROM users`);

    res.status(200).json({
      success: true,
      message: "Users retrieved successfully.",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error,
      data: null,
    });
  }
});

// get single user
router.get("/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `SELECT * FROM users WHERE id = $1`,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        message: "User not found",
      });
    }

    res.json({
      message: "User found",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      message: error.message,
    });
  }
});

export const userRouter = router;
```

---

## 2. Handling HTTP methods (Controller layer)

### What’s done

- Routes only define endpoints
    
- Controller handles request/response logic
    
- Business logic still partially inside controller
    
- Better separation than routes-only structure
    

---

### `users.routes.ts`

```ts
router.post("/", userController.createUser);
router.get("/", userController.getAllUsers);
router.get("/:id", userController.getSingleUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

export const userRouter = router;
```

---

### `users.controller.ts`

```ts
import type { Request, Response } from "express";
import { pool } from "../../db";

const createUser = async (req: Request, res: Response) => {
  try {
    const { name, age, email, password } = req.body;

    if (!name || !email || !password) {
      return res.status(400).json({
        message: "Name, email and password are required",
      });
    }

    const result = await pool.query(
      `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
      [name, age, email, password],
    );

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
};

const getAllUsers = async (req: Request, res: Response) => {
  try {
    const result = await pool.query(`SELECT * FROM users`);

    res.status(200).json({
      success: true,
      message: "Users retrieved successfully.",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error,
    });
  }
};

const getSingleUser = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `SELECT * FROM users WHERE id = $1`,
      [id],
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        message: "User not found",
      });
    }

    res.json({
      message: "User found",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      message: error.message,
    });
  }
};

export const userController = {
  createUser,
  getAllUsers,
  getSingleUser,
};
```

---

## 3. Handling DB services (Service layer)

### What’s done

- Controller no longer directly writes SQL logic (or minimized)
    
- Database logic moved to service layer
    
- Service handles all PostgreSQL queries
    
- Controller only calls service functions
    
- Cleaner separation: Controller → Service → DB
    

---

### `users.controller.ts`

```ts
import { userService } from "./users.service";
```

```ts
const createUser = async (req: Request, res: Response) => {
  try {
    const result = await userService.createUserIntoDB(req.body);

    res.status(201).json({
      message: "User created successfully",
      data: result.rows[0],
    });
  } catch (error: any) {
    if (error.code === "23505") {
      return res.status(409).json({
        message: "Email already exists",
      });
    }

    res.status(500).json({
      message: error.message,
    });
  }
};
```

---

### `users.service.ts`

```ts
import { pool } from "../../db";

const createUserIntoDB = async (payload: any) => {
  const { name, age, email, password } = payload;

  const result = await pool.query(
    `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
    `,
    [name, age, email, password],
  );

  return result;
};

const getAllUsersFromDB = async () => {
  return await pool.query(`SELECT * FROM users`);
};

const getSingleUserFromDB = async (id: any) => {
  return await pool.query(
    `SELECT * FROM users WHERE id = $1`,
    [id],
  );
};

const updateUserInDB = async (id: any, payload: any) => {
  const { name, age, email, password, is_active } = payload;

  return await pool.query(
    `
      UPDATE users
      SET 
        name = COALESCE($1, name),
        age = COALESCE($2, age),
        email = COALESCE($3, email),
        password = COALESCE($4, password),
        is_active = COALESCE($5, is_active),
        updated_at = NOW()
      WHERE id = $6
      RETURNING *
    `,
    [name, age, email, password, is_active, id],
  );
};

const deleteUserFromDB = async (id: any) => {
  return await pool.query(
    `
      DELETE FROM users
      WHERE id = $1
      RETURNING *
    `,
    [id],
  );
};

export const userService = {
  createUserIntoDB,
  getAllUsersFromDB,
  getSingleUserFromDB,
  updateUserInDB,
  deleteUserFromDB,
};
```

---

## Summary

- Routes → define endpoints only
    
- Controller → handles request/response
    
- Service → handles DB logic
    
- DB → pure connection layer
    

This is the real-world backend structure used in production systems for scalability and maintainability.

## Managing the User Profile with SQL Reference

To extend the system, we introduce a **profile table** and connect it with the `users` table using a relational (foreign key) relationship in PostgreSQL (NeonDB).

This enables a **one-to-one relationship** where each user has one profile.

---

### `db/index.ts`

```ts
import { Pool } from "pg";
import config from "../config";

// PostgreSQL pool
export const pool = new Pool({
  // from NeonDB
  connectionString: config.connectionString,
});

// initialize database tables
export const initDB = async () => {
  try {
    // users table
    await pool.query(
      `
      CREATE TABLE IF NOT EXISTS users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(20),

        age INT,

        email VARCHAR(50) NOT NULL UNIQUE,

        password TEXT NOT NULL,

        is_active BOOLEAN DEFAULT true,

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      );
      `,
    );

    // profiles table (linked with users)
    await pool.query(
      `
      CREATE TABLE IF NOT EXISTS profiles (
        id SERIAL PRIMARY KEY,

        user_id INT UNIQUE REFERENCES users(id) ON DELETE CASCADE,

        bio TEXT,
        address TEXT,
        phone VARCHAR(15),
        gender VARCHAR(10),

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      );
      `,
    );

    console.log("Database connected successfully.");
  } catch (error) {
    console.log(error);
  }
};
```

---

## Key Concepts (SQL Relationship)

### 1. Foreign Key (`user_id`)

- Links `profiles.user_id` → `users.id`
    
- Ensures every profile belongs to a valid user
    

---

### 2. `UNIQUE` constraint

- Ensures **one profile per user**
    
- Prevents multiple profiles for the same user
    

---

### 3. `ON DELETE CASCADE`

- If a user is deleted → their profile is automatically deleted
    
- Maintains data consistency (no orphan profiles)
    

---

## Relationship Summary

- `users` = main entity
    
- `profiles` = extended user data
    
- Relationship type = **one-to-one**
    
- Enforced via `FOREIGN KEY + UNIQUE`
    

---

## Outcome

This setup normalizes the database:

- Core authentication data stays in `users`
    
- Extended personal information stays in `profiles`
    
- Data integrity is enforced at database level (not just backend logic)

## Creating user profiles

### 1. `app.ts`

```ts
app.use("/api/profiles", profileRoute)
```

### 2. `profiles.route.ts`

```ts
import { Router } from "express";
import { profileContoller } from "./profiles.controller";

const router = Router();

router.post("/",profileContoller.createProfile); // create profile
router.get("/",profileContoller.getAllProfiles) // get all profiles
router.get("/:id",profileContoller.getSingleProfile) // get single profile
router.put("/:id",profileContoller.updateProfile); // update profile
router.delete("/:id",profileContoller.deleteProfile); // delete profile

export const profileRoute = router; 
```

### 3. `profiles.controller.ts`

```ts
import type { Request, Response } from "express";
import { profileService } from "./profiles.service";

const createProfile = async (req: Request, res: Response) => {
  const { user_id, bio, address, phone, gender } = req.body;

  try {
    const result = await profileService.createProfileIntoDB(req.body);
    res.status(201).json({
      success: true,
      message: "Profile created successfully.",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const getAllProfiles = async (req: Request, res: Response) => {
  try {
    const result = await profileService.getAllProfilesFromDB();
    res.status(200).json({
      success: true,
      message: "Fetched profiles successfully.",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const getSingleProfile = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await profileService.getSingleProfileFromDB(id);
    res.status(200).json({
      success: true,
      message: "Fetched profile successfully.",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const updateProfile = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await profileService.updateProfileInDB(id, req.body);
    res.status(201).json({
      success: true,
      message: "Updated profile successfully.",
      data: result.rows[0],
    });

    if (result.rows.length === 0) {
      res.status(404).json({
        success: false,
        message: "Profile not found",
        data: null,
      });
    }
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const deleteProfile = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await profileService.deleteProfileFromDB(id);

    if (result.rows.length === 0) {
      res.status(404).json({
        success: false,
        message: "Profile not found",
        data: null,
      });
    }

    res.status(200).json({
      success: true,
      message: "Deleted profile successfully.",
      data: result.rows[0],
    });

  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error
    })
  }
};

export const profileContoller = {
  createProfile,
  getAllProfiles,
  getSingleProfile,
  updateProfile,
  deleteProfile
};
```

### 4. `profiles.service.ts`

```ts
import { pool } from "../../db";
import type { Profile } from "./profiles.interface";

const createProfileIntoDB = async (payload: Profile) => {
  const { user_id, bio, address, phone, gender } = payload;

  // check if user exists
  const user = await pool.query(
    `
        SELECT * FROM users
        WHERE id = $1
    `,
    [user_id],
  );

  if (user.rows.length === 0) {
    throw new Error("User does not exist !");
  }

  const result = await pool.query(
    `
        INSERT INTO profiles(user_id, bio, address, phone, gender)
        VALUES ($1,$2,$3,$4,$5)
        RETURNING *
    `,
    [user_id, bio, address, phone, gender],
  );

  return result;
};

const getAllProfilesFromDB = async () => {
  const result = await pool.query(
    `
        SELECT * FROM profiles;
    `,
  );

  return result;
};

const getSingleProfileFromDB = async (id: any) => {
  const result = await pool.query(
    `
        SELECT * FROM profiles
        WHERE id = $1
    `,
    [id],
  );

  return result;
};

const updateProfileInDB = async (id: any, payload: Profile) => {
  const { user_id, bio, address, phone, gender } = payload;

  // check if profile exists
  const profile = await pool.query(
    `
        SELECT * FROM profiles
        WHERE id = $1
    `,
    [id],
  );

  if (profile.rows.length === 0) {
    throw new Error("Profile not found.");
  }

  const result = await pool.query(
    `
        UPDATE profiles
        SET 
          user_id = COALESCE($1, user_id),
          bio = COALESCE($2, bio),
          address = COALESCE($3, address),
          phone = COALESCE($4, phone),
          gender = COALESCE($5, gender)

        WHERE id = $6
        RETURNING *
    `,
    [user_id, bio, address, phone, gender, id],
  );

  return result;
};

const deleteProfileFromDB = async (id: any) => {
  // check if profile exists
  const profile = await pool.query(
    `
        SELECT * FROM profiles
        WHERE id = $1
    `,
    [id],
  );

  if (profile.rows.length === 0) {
    throw new Error("Profile not found.");
  }

  const result = pool.query(
    `
        DELETE FROM profiles
        WHERE id = $1
        RETURNING *
    `,
    [id],
  );

  return result;
};

export const profileService = {
  createProfileIntoDB,
  getAllProfilesFromDB,
  getSingleProfileFromDB,
  updateProfileInDB,
  deleteProfileFromDB
};
```

## Authentication and Authorization

Authentication and authorization are core parts of backend security systems.

- **Authentication** → verifies _who the user is_
    
- **Authorization** → verifies _what the user can access_
    

Example:

- Login system → authentication
    
- Admin dashboard permission → authorization
    

---

### Authentication

Authentication is the process of verifying user identity.

Example:

- Email + password login
    
- OTP verification
    
- Google login
    

---

### Core Types of Authentication

1. Session-based (Stateful)
    
2. JWT-based (Stateless)
    

---

### Session-based Authentication (Stateful)

- The server stores user session data on its side
    
- The client (browser) receives a session ID stored in cookies
    
- On every request → the cookie is sent → server verifies the session
    
- Called **stateful** because the server stores session information
    
- Requires server-side storage
    
- Harder to scale in large distributed systems
    

#### Flow

```mermaid
flowchart LR
Client --> Cookie
Cookie --> Server
Server --> SessionDB
SessionDB --> Server
Server --> Client
```

---

### JWT-Based Authentication (Stateless)

JWT = JSON Web Token

- After login, the server generates a JWT token
    
- The token is stored on the client side
    
- On every request → token is sent → server verifies it
    
- Called **stateless** because the server stores no session data
    
- Faster and easier to scale
    
- Common in modern REST APIs and mobile apps
    

#### Flow

```mermaid
flowchart LR
Client --> JWT
JWT --> Server
Server --> Client
```

---

### Session vs JWT

|Feature|Session-based|JWT-based|
|---|---|---|
|Storage|Server-side|Client-side|
|Stateful|Yes|No|
|Scalability|Lower|Higher|
|Speed|Slightly slower|Faster|
|Best for|Traditional web apps|Modern APIs / mobile apps|

---

### Authorization

Authorization determines what an authenticated user is allowed to do.

Example:

- Admin can delete users
    
- Normal user cannot access admin routes
    

---

### Core Types of Authorization

1. Role-Based Access Control (RBAC)
    
2. Permission-Based Access
    
3. Attribute-Based Access Control (ABAC)
    
4. Policy-Based Access Control (PBAC)
    

---

### Role-Based Access Control (RBAC)

Access is controlled based on user roles.

#### Example

- Admin → full access
    
- User → limited access
    
- Agent → special permissions
    

#### Common in

- Dashboards
    
- CMS systems
    
- Admin panels
    

---

### Permission-Based Access (Fine-Grained Access)

Permissions are assigned individually.

#### Example

- User 1 → read + write
    
- User 2 → read only
    

#### Benefit

More flexible than RBAC because permissions are controlled at a detailed level.

---

### Attribute-Based Access Control (ABAC)

Access depends on multiple attributes and conditions.

#### Example

Only allow access if:

- role = admin
    
- AND location = Bangladesh
    
- AND time = office hours
    

#### Common attributes

- User role
    
- Location
    
- Device
    
- Time
    
- Department
    

---

### Policy-Based Access Control (PBAC)

Access is controlled using defined business policies.

#### Example

- Only premium users can access this route
    
- Only verified users can withdraw money
    

#### Common in

- Banking systems
    
- SaaS applications
    
- Subscription systems
    

---

### Conclusion

- Authentication → verifies identity
    
- Authorization → controls permissions
    

Modern backend systems commonly use:

- JWT for authentication
    
- RBAC or Permission-based systems for authorization
    

As systems grow larger, ABAC and PBAC provide more flexible and scalable access control mechanisms.

## Introduction with Bcrypt and Hashing Password

**Doc:** [bcrypt npm package documentation](https://www.npmjs.com/package/bcrypt?activeTab=readme&utm_source=chatgpt.com)

Previously, we were storing passwords as plain text in the database, which is a major security risk.

If the database gets leaked or hacked, every user's password becomes exposed instantly.

To solve this, we use **hashing**.

Hashing converts the original password into an encrypted irreversible string.

Example:

```txt
password123
↓
$2b$10$NlY6rEpXvGF4/SJbeys1sOKrzmwjMoql2lbxmBIrkPUkHOQ3hfYvK
```

Even if someone gets access to the database, they cannot directly recover the original password.

---

### Install bcrypt

```bash
npm i bcrypt
```

---

### `users.service.ts`

```ts
const createUserIntoDB = async (payload: User) => {
  const { name, age, email, password } = payload;

  // salt rounds:
  // defines how many times the password hashing process runs
  // higher = more secure but slower

  const hashedPassword = await bcrypt.hash(password, 10);

  // save hashed password in DB
  const result = await pool.query(
    `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
    [name, age, email, hashedPassword],
  );

  return result;
};
```

---

### Response

```js
{
  "message": "User created successfully",
  "data": {
    "id": 2,
    "name": "Test user - 2",
    "age": 90,
    "email": "example02@gmail.com",
    "password": "$2b$10$NlY6rEpXvGF4/SJbeys1sOKrzmwjMoql2lbxmBIrkPUkHOQ3hfYvK",
    "is_active": true,
    "created_at": "2026-05-26T10:25:44.185Z",
    "updated_at": "2026-05-26T10:25:44.185Z"
  }
}
```

The password is now hashed before being stored in the database.

---

### Removing password from response

We want the hashed password to be stored in the database, but we should never send it back to the client in API responses.

---

### Updated code

```ts
const createUserIntoDB = async (payload: User) => {
  const { name, age, email, password } = payload;

  const hashedPassword = await bcrypt.hash(password, 10);

  const result = await pool.query(
    `
      INSERT INTO users (name, age, email, password)
      VALUES ($1, $2, $3, $4)
      RETURNING *
      `,
    [name, age, email, hashedPassword],
  );

  // remove password before sending response
  delete result.rows[0].password;

  return result;
};
```

---

### Why this matters

Even hashed passwords should not be exposed in API responses because:

- It leaks sensitive authentication-related data
    
- Attackers may attempt brute-force attacks offline
    
- It is considered bad backend security practice
    

---

### Outcome

- Passwords are no longer stored as plain text
    
- User passwords are securely hashed using bcrypt
    
- Password field is removed from API responses
    
- Backend authentication security is now significantly improved

## Introduction to JWT (JSON Web Token)

JWT stands for **JSON Web Token**.

JWT is a digital token used to identify and verify a user.

It is commonly used in authentication systems to securely transfer information between the **client** and the **server**.

JWT-based authentication is widely used in modern applications because it is:

- Faster
    
- Scalable
    
- Stateless
    
- Suitable for APIs and distributed systems
    

---

### Structure of JWT

A JWT token has 3 parts separated by dots (`.`)

```txt
xxxxx.yyyyy.zzzzz
```

These 3 parts are:

1. Header
    
2. Payload
    
3. Signature
    

---

### JWT Header

The header contains metadata about the token.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

- `alg` → which algorithm is used to generate the token
    
- `typ` → token type (`JWT`)
    

---

### JWT Payload

The payload contains the actual data or information we want to store inside the token.

Example:

```json
{
  "userId": 1,
  "email": "mahmud@gmail.com",
  "role": "admin"
}
```

This data is usually called **claims**.

Common payload data:

- user id
    
- email
    
- role
    
- permissions
    

---

### JWT Signature

The signature is used to verify whether the token is valid or modified.

It is created using:

- Header
    
- Payload
    
- Secret key
    

The server combines all of them using an algorithm and generates a unique signature.

```txt
Signature = Header + Payload + SecretKey
```

If someone changes the token data, the signature becomes invalid.

---

## How JWT Authentication Works

### Step-by-step flow

1. User sends email and password to the server
    
2. Server checks:
    
    - user exists or not
        
    - password matches or not
        
3. If valid:
    
    - server creates a payload
        
    - generates a JWT token
        
4. Server sends the token to the client
    
5. Client stores the token
    
6. Client sends the token in future requests
    
7. Server verifies the token and identifies the user
    

---

### Authentication flow

```mermaid
sequenceDiagram
    participant Client
    participant Server
    participant DB

    Client->>Server: Login (email + password)
    Server->>DB: Verify user
    DB-->>Server: User found

    Server->>Server: Generate JWT

    Server-->>Client: Send token

    Client->>Server: Request with token
    Server->>Server: Verify token
    Server-->>Client: Response
```

---

## JWT Token Generation

To generate a JWT token:

- create a payload
    
- use `jwt.sign()`
    
- provide a secret key
    
- provide expiration time
    

Example idea:

```ts
jwt.sign(payload, secret, {
  expiresIn: "1d"
});
```

---

## Expiration Time

JWT tokens usually have an expiration time.

Example:

- 1 hour
    
- 1 day
    
- 7 days
    

After expiration:

- user must login again
    
- a new token is generated
    

This improves security.

---

## Session vs JWT

Previously we learned two authentication systems:

1. Session-based authentication
    
2. JWT-based authentication
    

JWT-based authentication is more popular in modern backend systems because:

- server does not store session data
    
- easier to scale
    
- faster for APIs
    
- works well with mobile and frontend frameworks
    

---

## Authentication Module

Authentication mainly handles:

- login
    
- token generation
    
- token verification
    
- protected routes
    

To implement authentication properly, we usually create a separate module:

```txt
auth/
├─ auth.routes.ts
├─ auth.controller.ts
├─ auth.service.ts
├─ auth.utils.ts
```

This keeps authentication logic separated from user management logic.

### 1. `auth.service.ts`

```ts
import { pool } from "../../db";
import bcrypt from "bcrypt";

// 1. Check if user exists (done)
// 2. Match password (done)
// 3. Generate token

const loginUserIntoDB = async (payload: {
  email: string;
  password: string;
}) => {
  const { email, password } = payload;

  const userData = await pool.query(
    `
            SELECT * FROM users
            WHERE email = $1
        `,
    [email],
  );

  const user = userData.rows[0];

  if (!user) {
    throw new Error("Invalid credentials !");
  }

  const matchPassword = await bcrypt.compare(password, user.password);

  if (!matchPassword) {
    throw new Error("Invalid credentials !");
  }

  // Generating JWT

  // here
};

export const authService = {
    loginUserIntoDB
}
```

Now, we have to generate a JWT

#### 1.1 Generating JWT

**Doc:** [jsonwebtoken npm package documentation](https://www.npmjs.com/package/jsonwebtoken?utm_source=chatgpt.com)

Previously, we implemented password hashing using `bcrypt`.

Now we will generate a JWT token after successful login.

The token will help the server identify authenticated users in future requests.

---

### Install jsonwebtoken

```bash
npm i jsonwebtoken
```

---

### `auth.service.ts`

```ts
import { pool } from "../../db";
import bcrypt from "bcrypt";
import jwt from "jsonwebtoken";

// ... login logic ...

// JWT payload
const jwtPayload = {
  id: user.id,
  name: user.name,
  email: user.email,
  is_active: user.is_active,
};

// generate token
const token = jwt.sign(jwtPayload, "sudanirfua420", {
  expiresIn: "2d",
});

return token;
```

---

### Understanding the code

#### JWT Payload

```ts
const jwtPayload = {A
  id: user.id,
  name: user.name,
  email: user.email,
  is_active: user.is_active,
};
```

This payload contains the user information that will be stored inside the token.

Usually we store:

- user id
    
- email
    
- role
    
- permissions
    

We should avoid storing sensitive data like:

- password
    
- OTP
    
- secret information
    

---

#### `jwt.sign()`

```ts
jwt.sign(payload, secret, options)
```

- `payload` → data inside token
    
- `secret` → secret key used for signature generation
    
- `options` → additional settings like expiration time
    

---

#### Expiration

```ts
expiresIn: "2d"
```

The token will expire after 2 days.

After expiration:

- token becomes invalid
    
- user must login again
    

---

### Important

```ts
"sudanirfua420"
```

This secret key should never be hardcoded.

Move it into environment variables or config files.

Example:

```env
JWT_SECRET=sudanirfua420
```

Then use:

```ts
config.jwt_secret
```

---

### `auth.controller.ts`

```ts
const userLogin = async (req: Request, res: Response) => {
  const { email, password } = req.body;

  try {
    const result = await authService.loginUserIntoDB(req.body);

    res.status(200).json({
      success: true,
      message: "User logged in successfully",
      data: result,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};
```

---

### Response

After successful login, the API returns a JWT token.

Example:

```txt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

This token contains:

- Header
    
- Payload
    
- Signature
    

---

### JWT Flow

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: email + password

    Server->>Server: Verify credentials
    Server->>Server: Generate JWT

    Server-->>Client: Return token

    Client->>Server: Send token in future requests
    Server->>Server: Verify token
```

---

### Outcome

- User can now login using JWT authentication
    
- Server generates a token after successful login
    
- Token contains user identity information
    
- Authentication becomes stateless
    
- Backend is now ready for protected routes and authorization systems

# Module 9: Authorization and Middleware (Express)

we learned about **authentication and authorization**, and implemented authentication. Now we will implement **authorization**.

Before that, middleware needs to be clear because authorization depends heavily on it.

---

### What is Middleware?

Middleware is a **middle layer (middle man)** between request and response.

It behaves like a translator / tour guide:

- Client sends request
    
- Server processes it in multiple layers
    
- Middleware sits in between and processes/filters requests before they reach the controller
    

---

### Request Flow in Express

#### Basic flow idea

```mermaid
flowchart LR
Client --> Server --> Middleware --> Route --> Controller --> Service --> DB --> Response
```

---

#### Clean backend layered flow

```mermaid
flowchart LR
Client --> app.ts --> route.ts --> controller.ts --> service.ts --> DB
DB --> service.ts --> controller.ts --> Client
```

---

### Built-in Middleware

Express provides built-in middleware.

Examples:

- `express.json()` → parses JSON body
    
- `express.text()` → parses text input
    

#### Flow

```mermaid
flowchart LR
Client --> Request
Request --> express_json
express_json --> Routes
Routes --> Controller
```

What it does:

- preprocess request
    
- convert raw data into usable format
    
- forward to route handler
    

---

### Custom Middleware

Custom middleware is written by developers.

Used for:

- authentication checks
    
- authorization checks
    
- validation
    
- logging
    
- request blocking
    

---

### Middleware Position in Architecture

Middleware sits between route and controller:

```mermaid
flowchart LR
Route --> Middleware --> Controller --> Service --> DB
```

---

### How Middleware Works

Middleware receives:

- `req`
    
- `res`
    
- `next()`
    

#### Execution flow

```mermaid
flowchart LR
Request --> Middleware --> Decision{Call next ? }
Decision -- yes --> Controller
Decision -- no --> Blocked_Request
```

If `next()` is not called:

- request stops
    
- stays pending / blocked
    

---

### Multiple Middleware Chain

Multiple middleware can run sequentially.

```mermaid
flowchart LR
Request --> M1 --> M2 --> M3 --> Controller
```

Each middleware:

- validates something
    
- modifies request if needed
    
- calls `next()` to continue
    

If any middleware blocks:

- chain stops immediately
    

---

### Use Cases of Middleware

#### Authorization (main use)

```mermaid
flowchart LR
Client --> AuthMiddleware --> AdminCheck --> Controller
```

#### Logging system

```mermaid
flowchart LR
Request --> LoggerMiddleware --> Controller
LoggerMiddleware --> LogDB[(Logs DB)]
```

---

### Why Middleware Comes Before Controller

Placement:

```mermaid
flowchart LR
Route --> Middleware --> Controller
```

Reason:

- block unauthorized users early
    
- prevent invalid requests from reaching business logic
    
- reduce unnecessary DB load
    
- improve security and structure
    

---

### Conclusion

Middleware is a **core architectural layer in Express**.

It acts as:

- filter
    
- validator
    
- gatekeeper
    
- logger
    
- authorization handler
    

It is essential for:

- authentication flow control
    
- authorization enforcement
    
- request validation
    
- system security and scalability

---

## Implementing a logger middleware

**Doc:** [Express middleware documentation](https://expressjs.com/en/guide/using-middleware/?utm_source=chatgpt.com)

Here, we're going to log every request's:

- time
    
- method
    
- URL
    

This is useful for:

- tracking API activity
    
- debugging
    
- monitoring requests
    

```mermaid
flowchart LR
Client --> Middleware --> Route
Middleware --> log.txt
```

`app.ts`

```ts
// don't memorize, get em from docs
app.use((req, res, next) => {

  const log = `Url: ${req.url}, Method: ${req.method}, Time: ${Date.now()}\n`;

  fs.appendFile("log.txt", log, (err) => { // this'll append every request log in a file named `log.txt`
    console.log(err);
  });
  
  next(); // pass request to the next middleware / route
});
```

`log.txt`

```txt
Url: /users, Method: GET, Time: 1779982987411
Url: /users/, Method: GET, Time: 1779982988947
Url: /api/users, Method: GET, Time: 1779983034398
Url: /api/users/1, Method: GET, Time: 1779983053737
```

Without calling `next()`, the request will stay pending and won't reach the route/controller.

---
### clean up

`./src/middlewares/logger.ts` 

```ts
import type { NextFunction, Request, Response } from "express";
import fs from "fs";

const logger = (req: Request, res: Response, next: NextFunction) => {
  const log = `Url: ${req.url}, Method: ${req.method}, Time: ${Date.now()}\n`;

  fs.appendFile("log.txt", log, (err) => {
    console.log(err);
  });

  next();
};

export default logger;
```

`app.ts`

```ts
app.use(logger);
```
## Creating auth middleware

`middlewares/auth.ts`

```ts
import type { NextFunction, Request, Response } from "express";

const auth = (req:Request, res:Response, next:NextFunction) => {
    console.log("This is protected route.");
}

export default auth;
```

We didn't call `next()` here.

Suppose only an authorized person can access all user data, so let's use this middleware on `getAllUsers`.

`users.routes.ts`

```ts
router.get("/", auth, userController.getAllUsers);
```

When we send a request, the server doesn't return a response unless we call `next()`.

So, we can put the `next()` function inside a condition block if the person is authorized.

---

Wait. Check this out first.

```ts
import type { NextFunction, Request, Response } from "express";

const auth = (req: Request, res: Response, next: NextFunction) => {
  console.log(req.headers);
  next();
};

export default auth;
```

```js
{
  host: 'localhost:3000',
  'user-agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:150.0) Gecko/20100101 Firefox/150.0',
  accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'accept-language': 'en-US,en;q=0.9',
  'accept-encoding': 'gzip, deflate, br, zstd',
  connection: 'keep-alive',
  'upgrade-insecure-requests': '1',
  'sec-fetch-dest': 'document',
  'sec-fetch-mode': 'navigate',
  'sec-fetch-site': 'none',
  'sec-fetch-user': '?1',
  'if-none-match': 'W/"404-KEJh5O/4y0Xv2nz/AVoWsXDbNZE"',
  priority: 'u=0, i'
}
```

We can also send our JWT token through headers.

When a user sends a request to the `/users` route, we'll check for the token inside the headers.

1. Get the token  
    ![Pasted-image-20260528222316.png](../media/Pasted-image-20260528222316.png)
    
2. Go to client (Postman) and do as shown below  
    ![Pasted-image-20260528222639.png](../media/Pasted-image-20260528222639.png)
    

Set:

- header key → `authorization`
    
- value → JWT token
    

Then send the request.

**Console**

```js
{
  authorization: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NiwiYmF0Y2hIbyI6R3V5Z2giLCJlbWFpbCI6bWpWbXFDTWlsbSIsZ2VjdXJpdHl6SW1hbGUiOiJzYwFld30jR21zU0Tg1MzU3TjU3ZTNjZldjZjU3X2FpUfn3ET3QlVDdzcnB2bzBvOy4LJeU',
  'content-type': 'application/json',
  'user-agent': 'PostmanRuntime/7.39.1',
  accept: '*/*',
  'cache-control': 'no-cache',
  'postman-token': 'f8791e38-585d-40d0-a31a-8ce0a457fd33',
  host: 'localhost:3000',
  'accept-encoding': 'gzip, deflate, br',
  connection: 'keep-alive',
  'content-length': '61'
}
```

### `middlewares/auth.ts`

```ts
import type { NextFunction, Request, Response } from "express";

const auth = (req: Request, res: Response, next: NextFunction) => {
  const token = req.headers.authorization;

  if (!token) {
    res.status(401).json({
      success: false,
      message: "Unauthorized access !!",
    });
  } else {
    next();
  }
};

export default auth;
```

Now, the middleware checks whether the request contains a token inside the headers.

- If token exists → request moves to the controller using `next()`
    
- If token does not exist → server returns `401 Unauthorized`
    

```mermaid
flowchart LR
Client --> AuthMiddleware

AuthMiddleware -->|Token Exists| Controller
AuthMiddleware -->|No Token| Unauthorized
```

## Auth middleware validation

Wait, we checked if the token exists, but what if:

**Someone tries to enter with a fake token.**

> It doesn't check whether the token is valid, it only checks if it exists.

Now, we're going to validate it.

### 1. Decode the JWT

**Doc:** [jsonwebtoken verify documentation](https://www.npmjs.com/package/jsonwebtoken?utm_source=chatgpt.com#:~:text=var%20decoded%20%3D%20jwt%2Everify%28token%2C%20%27shhhhh%27%29%3B)

`middlewares/auth.ts`

```ts
const decodedToken = jwt.verify(token as string, config.secretKey);
console.log(decodedToken);
```

```json
{
  id: 6,
  name: "testUser_forAuth",
  email: "testUser@gmail.com",
  is_active: true,
  iat: 1780036212,
  exp: 1780209012
}
```

It returns the user's information from the payload, including the email.

We need that email to validate the user from the database.

### 2. Check DB for that email

```ts
const userData = await pool.query(
  `
    SELECT * FROM users
    WHERE email = $1
  `,
  [decodedToken.email]
)

const user = userData.rows[0];
console.log(user)
```

Now, if the user exists in the database, they can access the route.

### 3. Validation

```ts
// validation logics
if (userData.rows.length === 0) {
  res.status(404).json({
    success: false,
    message: "User not found",
  });
}

const user = userData.rows[0];

if (!user.is_active) {
  res.status(403).json({
    success: false,
    message: "Forbidden",
  });
}
```

Now the middleware validates:

- whether the token is real
    
- whether the user exists
    
- whether the account is active
    

Only after passing all validations should the request move to the controller using `next()`.

```mermaid
flowchart LR
Client --> TokenValidation
TokenValidation --> DecodeJWT
DecodeJWT --> CheckDB
CheckDB --> ActiveCheck

ActiveCheck -->|Valid User| Controller
ActiveCheck -->|Invalid / Blocked| Forbidden
```

## Setting the namespace

Now, since we got:

- JWT decoded
    
- DB checked
    
- activity status checked
    

we can safely attach the user data to the request object.

`auth.ts`

```ts
const user = userData.rows[0];

if (!user.is_active) {
  res.status(403).json({
    success: false,
    message: "Forbidden",
  });
}

req.user = decodedToken; // Property 'user' does not exist on type 'Request<ParamsDictionary, any, any, ParsedQs, Record<string, any>>'.

next();
```

Since `user` does not exist in Express’s native `Request` type, we extend the type using **namespace augmentation**.

**Doc:** [https://dev.to/kwabenberko/extend-express-s-request-object-with-typescript-declaration-merging-1nn5](https://dev.to/kwabenberko/extend-express-s-request-object-with-typescript-declaration-merging-1nn5)

### Create file `middlewares/index.d.ts`

```ts
import type { JwtPayload } from "jsonwebtoken";

declare global {
  namespace Express {
    interface Request {
      user: JwtPayload;
    }
  }
}
```

After validating the JWT and checking the user from the database, we attach the decoded user information to `req.user`.

This allows the next middleware/controller to directly access the authenticated user's data without validating the token again.

##  ## RBAC (Role-Based Access Control) System Implementation

RBAC (Role-Based Access Control) allows us to control access to routes based on a user's role. Instead of giving every user the same permissions, we can define which roles are allowed to perform specific actions.

### 1. Resolving role field

```ts
export interface User {
  name: string;
  age: number;
  email: string;
  password: string;
  is_active?: boolean;
  role: "user" | "agent" | "admin";
}
```

`db/index.ts`

```ts
// create table
export const initDB = async () => {
  try {
    await pool.query(
      // added role column in db
      `CREATE TABLE IF NOT EXISTS users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(20),

        age INT,

        role VARCHAR(10) DEFAULT 'user',

        email VARCHAR(50) NOT NULL UNIQUE,

        password TEXT NOT NULL,

        is_active BOOLEAN DEFAULT true,

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      );
    `,
    );
```

`users.service.ts`

```ts
const createUserIntoDB = async (payload: User) => {
  const { name, age, email, password, role } = payload;

  // Hash password with salt = 10
  const hashedPassword = await bcrypt.hash(password, 10);

  const result = await pool.query(
    `
      INSERT INTO users (name, age, email, password, role)
      VALUES ($1, $2, $3, $4, COALESCE($5, 'user'))
      RETURNING *
      `,
    [name, age, email, hashedPassword, role],
  );

  return result;
};
```

Updated other functions too.

```ts
const jwtPayload = {
  id: user.id,
  name: user.name,
  email: user.email,
  role: user.role,
  is_active: user.is_active,
};
```

Added some users to the database.

![Pasted-image-20260530175931.png](../media/Pasted-image-20260530175931.png)

### 2. Passing the role into the `auth()` function

Now we're going to make the `auth()` function in `middlewares/auth.ts` accept roles as parameters and validate access based on those roles.

```json
{
  "id": 5,
  "name": "Abdullah Al Mahmud",
  "email": "mahmudAdmin@gmail.com",
  "role": "admin",
  "is_active": true,
  "iat": 1780143060,
  "exp": 1780315860
}
```

When a valid request is made with a proper token, this data is available from the decoded JWT payload.

We can see the user's role here, and we're going to use it for authorization.

`users.routes.ts`

```ts
router.get("/", auth("admin", "agent"), userController.getAllUsers);
// only admin and agent can access this route
```

`auth.ts`

```ts
const auth = (...roles: any[]) => { // we'll define a type `Role` later

  // ...... other codes

  // validation logic:
  // 1. Check if any roles were provided
  // 2. Check whether the user's role matches one of the allowed roles
  // 3. If not, return forbidden

  if (roles.length && !roles.includes(user.role)) {
    res.status(403).json({
      success: false,
      message: "Forbidden",
    });
  }
}
```

Let's make a GET request using an admin account.

> Go to Postman (client) → log in with an admin email and password → get the token → put the token in the headers → send a GET request.

![Pasted-image-20260530184559.png](../media/Pasted-image-20260530184559.png)

Now let's try the same request with a regular `user`.

![Pasted-image-20260530184949.png](../media/Pasted-image-20260530184949.png)

The request is blocked because the user's role is not included in the allowed roles list.

**Good to have:** We should avoid hardcoding role strings and use proper type safety.

```ts
export const UserRoles = {
  admin: "admin",
  agent: "agent",
  user: "user",
} as const;

export type Role = "admin" | "agent" | "user";
```

This gives us:

- autocomplete support
    
- compile-time type checking
    
- fewer spelling mistakes
    
- centralized role management for the entire project

## Access Token + Refresh Token (Video Flow Explanation)

This system is used to balance **security and smooth user experience**. The idea is simple: one token is short-lived, the other is long-lived, so the user doesn’t have to log in repeatedly.

---

### Full Login + Access Flow

```mermaid
flowchart LR
A[User Login] --> B[Server verifies email & password]
B --> C[Server generates two tokens]
C --> D1[Access Token - short time]
C --> D2[Refresh Token - long time]

D1 --> E[User sends requests using Access Token]
E --> F{Token valid?}

F -- Yes --> G[Access granted]
F -- No --> H[Access Token expired]

H --> I[System uses Refresh Token automatically]
I --> J[Server verifies Refresh Token]
J --> K[New Access Token generated]
K --> E
```

---

### How It Works (Step by Step)

#### 1. Login

- User enters email and password
    
- Server checks if they are correct
    
- If correct → server gives two tokens
    

---

#### 2. Token Types

- **Access Token**
    
    - Short lifespan
        
    - Used for normal requests
        
    - Expires quickly for security
        
- **Refresh Token**
    
    - Long lifespan
        
    - Used only when access token expires
        
    - Helps user stay logged in
        

---

#### 3. Normal Request Flow

- User tries to access protected data
    
- Access token is sent with the request
    
- Server checks it:
    
    - If valid → data is returned
        
    - If expired → request is rejected
        

---

#### 4. When Token Expires

- Instead of asking user to log in again:
    
    - System automatically uses refresh token
        
- Server checks refresh token validity
    
- If valid:
    
    - New access token is issued
        
    - User continues without interruption
        

---

### Behind-the-Scenes Flow

```mermaid
sequenceDiagram
User->>Server: Login request
Server-->>User: Access token + Refresh token

User->>Server: Request data (Access token)
Server-->>User: Data OR expired response

User->>Server: Send refresh token
Server-->>User: New access token
User->>Server: Retry request
```

---

### Core Idea

- Access token = short permission pass
    
- Refresh token = long-term renewal key
    
- System keeps user logged in without repeated login
    
- Security stays strong because short-lived tokens reduce risk
    

---

### Why This Matters

- Improves user experience (no frequent login)
    
- Keeps system secure (short-lived access control)
    
- Prevents long-term abuse even if tokens are stolen

### Implementation

`config/index.ts`

```ts
refreshKey: process.env.REFRESH_SECRET as string
```

---

`auth.service.ts`

```ts
const refreshToken = jwt.sign(jwtPayload, config.refreshKey, {
  expiresIn: "7d",
});
```

---

Now, we're gonna send it in cookies

`auth.controller.ts`

```ts
const { refreshToken } = result;

res.cookie("refreshToken", refreshToken, {
  secure: false, // in production => true
  httpOnly: true,
  sameSite: "lax",
});
```

---

Now, let's do a user login

- it should give us an access token and refresh token in response and a refresh token in cookie
    

---

![Pasted-image-20260530224828.png](../media/Pasted-image-20260530224828.png)

**Response**

```json
{
  "success": true,
  "message": "User logged in successfully",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

## Secure request token API

Here we're going to do authentication token handling using refresh flow.

1. Define a controller function that'll console the cookies
    

`auth.controller.ts`

```ts
const refreshToken = (req: Request, res: Response) => {
  console.log(req.cookies);
};
```

`auth.route.ts`

```ts
router.post("/refresh-token", authController.refreshToken);
```

So when we do a login and then POST on `/auth/refresh-token`, it should log the refresh token in the console.

![image-2026531207649.png](../media/image-2026531207649.png)

But it logs `undefined`

This happens because Express does not parse cookies by default. Without a middleware, `req.cookies` is not populated, so the server cannot read incoming cookie headers.

For this we have to download a package called cookie-parser and use the express middleware

```bash
npm i cookie-parser
```

`app.ts`

```ts
import cookieParser from "cookie-parser";
app.use(cookieParser());
```

```json
{
  refreshToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

Got it

---

2. Now we're going to create a function for refreshing the access token so that the user can stay authenticated without logging in again.
    

`auth.service.ts`

```ts
const refreshAccessToken = async (token: string) => {
  if (!token) {
    throw new Error("Unauthorized !!");
  }

  // decode token
  const decodedToken = jwt.verify(
    token as string,
    config.refreshKey, // remember to use refreshKey, not secretKey
  ) as JwtPayload;

  // match token email with DB
  const userData = await pool.query(
    `
      SELECT * FROM users
      WHERE email = $1
    `,
    [decodedToken.email],
  );

  // validation logic
  if (userData.rows.length === 0) {
    throw new Error("User not found !!");
  }

  const user = userData.rows[0];

  if (!user.is_active) {
    throw new Error("Forbidden !!");
  }

  const jwtPayload = {
    id: user.id,
    name: user.name,
    role: user.role,
    is_active: user.is_active,
    email: user.email,
  };

  const refreshedAccessToken = jwt.sign(jwtPayload, config.secretKey, {
    expiresIn: "1d",
  });

  return { refreshedAccessToken };
};
```

Now use this in `auth.controller.ts` for handling the refresh request.

```ts
const refreshToken = async (req: Request, res: Response) => {
  try {
    const result = await authService.refreshAccessToken(
      req.cookies.refreshToken
    );

    res.status(200).json({
      success: true,
      message: "Access token refreshed.",
      data: result,
    });
  } catch (error: any) {
    res.status(400).json({
      success: false,
      message: error.message,
    });
  }
};
```

### Clarification

> [!NOTE]  
> **JWT Access vs Refresh Token — What I Misunderstood**
> 
> **Initial Confusion**  
> I thought both functions were just token generators for login, so I assumed:
> 
> - login generates access + refresh tokens
>     
> - refresh function should also generate both again
>     
> 
> This led to confusion about why a second token generation step exists.
> 
> ---
> 
> **Correct Understanding**
> 
> **1. Login Phase (`loginUserIntoDB`)**  
> This runs only once during authentication.
> 
> It:
> 
> - verifies email and password
>     
> - generates:
>     
>     - **Access Token** → short-lived, used for API requests
>         
>     - **Refresh Token** → long-lived, used to get new access tokens
>         
> 
> Output:
> 
> - both tokens are issued at login
>     
> 
> ---
> 
> **2. Refresh Phase (`refreshAccessToken`)**  
> This runs after login when the access token expires.
> 
> It:
> 
> - receives a **refresh token**
>     
> - verifies it
>     
> - generates:
>     
>     - **new access token only**
>         
> 
> It does NOT generate a new refresh token.
> 
> ---
> 
> **Key Idea**
> 
> - **Access Token** → short-term API authorization
>     
> - **Refresh Token** → long-term session continuity
>     
> - refresh flow → issue new access token without re-login
>     
> 
> ---
> 
> **Why Two Tokens Exist**
> 
> Without refresh token:
> 
> - user must log in again every time access token expires
>     
> 
> With refresh token:
> 
> - user stays authenticated
>     
> - only access token is rotated
>     
> 
> ---
> 
> **Naming Mistake I Had**
> 
> I used:
> 
> - `generateFreshToken`
>     
> 
> Correct naming:
> 
> - `refreshAccessToken`
>     
> 
> Reason:
> 
> - nothing is “fresh”
>     
> - we are refreshing the **access token**, not creating a new auth system
>     
> 
> ---
> 
> **Mental Model**
> 
> |Token Type|Purpose|Lifetime|
> |---|---|---|
> |Access Token|API authorization|short|
> |Refresh Token|session continuity|long|
> 
> ---
> 
> **Final Understanding**
> 
> Login → issue both tokens once  
> Refresh → use refresh token to issue new access token only

## CORS

CORS (Cross-Origin Resource Sharing) is a browser security mechanism that controls whether a frontend running on one origin can access resources from a backend on a different origin. Browsers block cross-origin requests by default to prevent unauthorized data access. The server must explicitly allow them using CORS headers.

- **Origin = protocol + domain + port** (e.g., `http://localhost:5173` vs `http://localhost:3000`)
    
- **Problem:** browser blocks requests between different origins even if backend is working fine
    
- **Fix:** backend must explicitly allow the frontend origin using `Access-Control-Allow-Origin`
    
- **Express setup:**
    
    ```ts
    app.use(cors({ origin: "http://localhost:5173", credentials: true }))
    ```
    
- **Cookies case:** requires `credentials: true` on backend and `withCredentials: true` on frontend requests
    
- **Without CORS:** requests fail, cookies are not sent, refresh-token flow breaks
    

---

### Implementation

1. Install package
    

```bash
npm i cors
```

2. Configure in `app.ts`
    

```ts
app.use(cors({
  origin: "http://localhost:3000"
}));
```

---

## Global Error Handling Architecture

This section introduces centralized error handling so all backend errors are managed in one place instead of scattered try-catch blocks.

When sending a request with an invalid token, Express returns a default HTML error page:

![image-20265312252329.png](../media/image-20265312252329.png)

This is not useful for APIs. We need structured JSON responses instead.

Reference: [https://dev.to/shyamtala/global-error-handling-in-expressjs-best-practices-4957](https://dev.to/shyamtala/global-error-handling-in-expressjs-best-practices-4957)

### Global error middleware

```ts
// middlewares/globalErrorHandler.ts

import { Request, Response, NextFunction } from "express";

const globalErrorHandler = (
  err: any,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  console.error(err.stack);

  res.status(500).json({
    success: false,
    message: err.message || "Internal Server Error",
  });
};

export default globalErrorHandler;
```

### Register middleware

```ts
import globalErrorHandler from "./middlewares/globalErrorHandler";

// routes above ...

app.use(globalErrorHandler);
```

Important rule: the global error handler must be registered after all routes. Otherwise, Express will never reach it.

### Result

```json
{
  "success": false,
  "message": "jwt malformed"
}
```

Now errors are consistent and API-friendly instead of raw HTML responses.

## SendResponse Utility Creation

In our project, an average response usually looks like this:

```ts
res.status(200).json({
  success: true,
  message: "User deleted successfully",
  data: result.rows[0],
});
```

This pattern gets repeated across multiple controllers, which violates the **DRY (Don't Repeat Yourself)** principle.

So we're going to create a reusable utility function that standardizes API responses throughout the application.

Create a file: `utilities/sendResponse.ts`

```ts
import type { Response } from "express";

interface TResponse {
  statusCode: number;
  success?: boolean;
  message?: string;
  data?: any;
  error?: any;
  meta?: any;
}

const sendResponse = (res: Response, options: TResponse) => {
  const { statusCode, success, message, data, error, meta } = options;

  // Auto-determine `success` based on status code
  const resSuccess = success ?? (statusCode >= 200 && statusCode < 400);

  res.status(statusCode).json({
    success: resSuccess,
    ...(message !== undefined && { message }),
    ...(data !== undefined && { data }),
    ...(meta !== undefined && { meta }),
  });
};

export default sendResponse;
```

Now, instead of manually creating response objects in every controller, we can simply call `sendResponse()` and keep our codebase consistent.

---

### Refactoring

After creating the utility, refactor the entire codebase to use the reusable response structure.

Refactored files:

1. `src/utilities/sendResponse.ts`
    
2. `src/modules/users/users.controllers.ts`
    
3. `src/middlewares/globalErrorHandler.ts`
    
4. `src/app.ts`
    
5. `src/modules/auth/auth.controller.ts`
    
6. `src/middlewares/auth.ts`
    
7. `src/modules/profiles/profiles.controller.ts`
    

This removes duplicate response logic and centralizes the response format across the project.

---

### Benefits

- Consistent API response structure
    
- Better maintainability
    
- Less repetitive code
    
- Easier future modifications
    
- Cleaner controllers
    

---

It was a lot of work 😮‍💨

(For the AI agent, not for me.) 😏

## Module 9.5: How to Deploy Your Express + TypeScript App

### `package.json`

```json
"scripts": {
  "start": "node dist/server.js",
  "dev": "tsx watch ./src/server.ts",
  "build": "tsc",
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

---

## Deployment Steps

### 1. Install dependencies

```bash
npm install
```

---

### 2. Build TypeScript → JavaScript

```bash
npm run build
```

This generates the `dist/` folder containing compiled `.js` files.

---

### 3. Run locally (production mode)

```bash
npm start
```

This executes:

```bash
node dist/server.js
```

---

### 4. Install Vercel CLI globally

```bash
npm i -g vercel
```

---

### 5. Login to Vercel

```bash
vercel login
```

Follow browser authentication.

---

### 6. Deploy project

```bash
vercel --prod
```

During setup:

- Choose team
    
- Link project (or create new)
    
- Set project name (must be lowercase)
    
- Confirm settings
    

---

### 7. Output

After deployment, Vercel provides:

- Production URL
    
- Alias URL
    

Example:

```
https://your-project.vercel.app
```

---

## Required Deployment Checklist

- `dist/` exists (build must run before deploy)
    
- `start` script points to `dist/server.js`
    
- environment variables configured in Vercel dashboard
    
- CORS updated to production frontend URL
    
- cookies enabled if using auth (`credentials: true`)