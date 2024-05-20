
# HTTP Protocol: Semantics

## 1. Definition and Chronology
- **HTTP (Hypertext Transfer Protocol)** is an application-level protocol for distributed, collaborative, hypermedia information systems.
- It operates on the principle of **'request-response'** and is a **client-server protocol** initiated by the recipient (usually the web browser).
- HTTP is the foundation of any data exchange on the Web.
- HTTP versions:

| Version | Year | Description |
|---------|------|--------------|
| 0.9 | 1991 | Solely intended for transferring data over the Internet (HTML files) |
| 1.0 | 1996 | Allowed transfer of messages with headers describing content using MIME encoding |
| 1.1 | 1997 | - |
| SPDY | 2012 | Google's transitional sub-layer protocol before HTTP/2 |
| HTTP/2 | 2015 | Introduced multiplexing, header compression, and server push |
| HTTP/3 | 2018/2019 | Evolution of Google's QUIC protocol, published as Proposed Standard in 2022 (RFC 9114) |

- While the low-level encoding mechanisms have changed across versions, the high-level HTTP semantics (verbs, methods, headers) have remained unaffected.

## 2. HTTP Request
- Request consists of a **request line**, **headers**, and an optional **request body**.
- **Request line**: `{Method} {Resource} {HTTP Version}` (e.g., GET /hello.txt HTTP/1.1)
- **Methods**:
  - **GET**: Retrieve data from a resource (**safe**, **idempotent**, **cacheable**)
    - Can use "range request" with the Range header to request part of a resource
  - **HEAD**: Retrieve headers of a resource (**safe**, **idempotent**)  
  - **POST**: Send data to be processed (**not cacheable**)
    - Can create new resources, append data to existing ones
    - Server responds with 201 Created or 200 OK status
  - **PUT**: Create or replace the entire state of a resource (**idempotent**)
    - Server responds with 201 Created, 200 OK, or 204 No Content
  - **DELETE**: Remove the resource (**idempotent**)
    - Server responds with 202 Accepted, 204 No Content, or 200 OK
  - **OPTIONS**: Describe communication options for the resource
    - Can be applied to a specific resource or server (`*`)
    - Server includes headers indicating supported options (Allow)
  - **TRACE**: Loops back the received request (**risky**, disabled on some servers)
  - **CONNECT**: Establishes a tunnel to the server (used for proxies)
- **Request headers**:
  - **Host**: Domain name of server handling the request (required for HTTP/1.1+)
  - **User-Agent**: Client software and platform making the request
  - **Accept**: Media types the client can handle (for content negotiation) 
  - **Cookie**: Data previously stored by the server on the client
  - **Accept-Language**, **Accept-Encoding**, **Connection**, **Content-Type**, **Authorization**, etc.
- **Request body**: Optional data sent with POST, PUT requests.

## 3. HTTP Response
- Response consists of a **status line**, **headers**, and an optional **response body**.
- **Status line**: `{HTTP Version} {Status Code} {Reason Phrase}` (e.g., HTTP/1.1 200 OK)
- **Status codes**:

| Code | Category | Description |
|------|-----------|-------------|
| 1xx | Informational | Request received and being processed (100 Continue, 101 Switching Protocols) |
| 2xx | Success | Request successfully completed <br> (200 OK, 201 Created, 202 Accepted, 204 No Content) |
| 3xx | Redirection | Further action needed <br> (301 Moved Permanently, 302 Found, 304 Not Modified) | 
| 4xx | Client Error | Request error <br> (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 405 Method Not Allowed) |
| 5xx | Server Error | Server failed to complete request <br> (500 Internal Server Error, 501 Not Implemented, 503 Service Unavailable) |

- **Response headers**:
  - **Date**: Date and time the response was generated
  - **Server**: Information about the server software  
  - **Content-Type**: Media type of the response body
  - **Content-Length**: Size of the response body
  - **Cache-Control**: Caching directives for the response
  - **Expires**, **Last-Modified**, **Location**, **Set-Cookie**, **ETag**, **Content-Encoding**, **Allow**, **Connection**, etc.

## 4. Caching Mechanism
- **Client-side caching**: Clients store and reuse responses based on caching headers
- **Cache-Control directives**: 
  - `public` (can be cached by intermediaries), `private` (only client caching) 
  - `max-age` (maximum time in seconds the response is fresh)
  - `no-store` (response cannot be stored)
- **Server-side caching**: Servers can cache resources and respond with "304 Not Modified" status if resource is still fresh  
- **Conditional requests**: Clients use `ETag` or `Last-Modified` headers to verify if cached resource is still valid
- **Cache invalidation**: Servers use `ETag` or `Last-Modified` to indicate when a resource has changed

## 5. Content Negotiation
- Clients use **Accept** and **Accept-Language** headers to specify content preferences
- Servers select best representation based on client preferences and server capabilities
- **Content-Type** header specifies the format of the response  
- **Quality values (q)** indicate priority of content types or languages
- Language negotiation using Accept-Language headers
- **Transparent Content Negotiation (TCN)** allows intermediary proxies to negotiate content on behalf of clients and servers

## 6. Protocols, Tools and Websites
- **Telnet**: Protocol for executing commands on a remote machine
- **Live HTTP Header**, **Web-Sniffer**: View HTTP request and response headers
- **Postman**, **Fiddler**, **Burp**, **HTTPRequester**: Tools for analyzing and managing HTTP traffic
- **http.dev**: Website for learning HTTP

## 7. Note About HTTP Headers
There are various categories of HTTP headers:

- **General headers**: Apply to both requests and responses (Date, Connection, Via)
- **Request headers**: Contain information about the request (Host, User-Agent, Accept, Cookie, Referer)
- **Response headers**: Contain information about the response (Status, Location, Server, Cache-Control, Set-Cookie)
- **Entity headers**: Contain information about the body message (Content-Type, Content-Length, Content-Encoding)
- **Authentication headers**: Used for client or server credentials (Authorization, WWW-Authenticate) 
- **Caching headers**: Control caching behavior (Cache-Control, Expires, Age, ETag, If-Modified-Since)
- **CORS headers**: Enable cross-origin resource sharing (Access-Control-Allow-Origin, Access-Control-Request-Method)
- **Security headers**: Enhance security of HTTP communication (Content-Security-Policy, Strict-Transport-Security)
```