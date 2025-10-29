# Differences
- In HTTP, it's possible to see credencials in clear-text

# URL 

<img width="850" height="201" alt="image" src="https://github.com/user-attachments/assets/c1a844f9-5b2e-4ee2-9a25-9d63b02f05e4" />

# HTTP FLOW 

<img width="870" height="380" alt="image" src="https://github.com/user-attachments/assets/66e2311c-a209-431d-9cd4-d9994e4e610d" />

# HTTPS FLOW 

<img width="861" height="645" alt="image" src="https://github.com/user-attachments/assets/6c2cc230-c157-44cb-bd01-0a4c418634da" />

# HTTP REQUEST

<img width="871" height="410" alt="image" src="https://github.com/user-attachments/assets/871091a1-4f43-4591-bd10-a1ee0d32854c" />

# HTTP Response

<img width="855" height="456" alt="image" src="https://github.com/user-attachments/assets/c17aaa5f-95f4-416d-83ac-b227bbac83a8" />

# HTTP Headers

## General Headers: 

Used in request and response HTTP. 
Content: 
- Date
- Connection

## Entity Headers
- Content-Type: Used to descrbe the rype of resource being transferred. Example: Content-Type: text/html
- Media-Type: Example: Media-Type: application/pdf
- Boundary: Acts as a marker to separate content when there is more than one in the same message. Example: boundary="b4e4fbd93540"
- Content-Length: Example: Conten-Length: 385
- Content-Encode: Example: Content-Encode: gzip

## Request Headers
- Host: Example: Host: www.inlanefreight.com
- User-Agent: Describe the client requesting resources. Example: User-Agent: curl/7.77.0
- Referer: Example: Referer: http://www.inlanefreight.com/
- Accept: Wich medai types the client can understand it. Example: Accept: */*
- Cookie: Example: Cookie: PHPSESSID=b4e4fbd93540
- Authorization: A metehod to identify client. Authorization: BASIC cGFzc3dvcmQK
- X-Forwarded-For: 10.10.11.2

## Response Header
- 	Server: Apache/2.2.14 (Win32)
- 	Set-Cookie: PHPSESSID=b4e4fbd93540
- 	WWW-Authenticate: BASIC realm="localhost"

## Security Headers
- Content-Security-Policy: Dictate the web page policy. Example: Content-Security-Policy: script-src 'self'
- Strict-Transport-Security: Deny to use HTTP. Example: Strict-Transport-Security: max-age=31536000
- Referrer-Policy:  Example: Referrer-Policy: origin

# HTTP Methods and Codes

## Request Models
- GET
- POST
- HEAD: Request Header
- PUT
- DELETE
- OPTIONS
- PATH

## Status Codes

- 1xx: Provides information and does not affect the proccessing of the request
- 2xx: Request succes. Example 200 OK: Successful request
- 3xx: Redirect
- 4xx: Improper request. Example 400 Bad Request - 403 Forbidden: This exist but it's hidden - 404 Not Found
- 5xx: Problem with the HTTP Server. Example: 500 Internal Server Error: Server cannot process the request

# TOOLS

## cURL
- curl -O: Download the web page
- curl -o: Download the web page and the save the output in a specific path
- curl -s: Show the ouput
- curl -k: Skip SSL certificate
- curl -v: See HTTP response and request
- curl -vvv: See more information about the conection
- curl -i: See the response header
- curl -I: See the HEAD request
- curl -A: Set up 'User-Agent'
- curl -u: provide credencial (user:passwd)
- curl -H: Set up header
- curl -X: User the method request: Example: curl -X POST
- curl -d: Add parameters
- curl -L: Can follow the redirection
- curl -b: Send a cookie
- curl | jq: para verlo en formato JSON

## Example
- Explotation a comunication with the back-end: curl -vvv http://admin:admin@94.237.57.211:33506/search.php?search=flag -H 'Basic YWRtaW46YWRtaW4='
- Explotation a POST parameter: curl -X POST -i http://94.237.57.1:43200/search.php -b 'PHPSESSID=1vt8afjj16uu864iveva4u -H 'Content-Type: application/json'
  
