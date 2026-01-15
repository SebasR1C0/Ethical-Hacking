# Obfuscating attacks using encodings
## URL encoding
URL encoding is a standard mechanism used in HTTP to safely transmit special characters inside URLs.
Some security controls (e.g., WAFs) may incorrectly handle multiple decoding steps. In such cases, double URL encoding can be used to bypass filters if the input is decoded more than once by the backend.

For example:
Plain-text: <img src=x onerror=alert(1)>
One url encoding: %3Cimg%20src%3Dx%20onerror%3Dalert(1)%3E
DOuble url encoding: %253Cimg%2520src%253Dx%2520onerror%253Dalert(1)%253E

## HTML encoding
HTML encoding replaces special characters with character references to prevent them from being interpreted as HTML markup.

Character references follow this format:
&name;
&#decimal;
&#xhex;

Example:
&colon;  → :
&#58;    → :
&#x3a;  → :

Note: The prefix "x" indicates a hexadecimal value.

HTML encoding can be abused for obfuscation in XSS attacks because browsers automatically decode these references during HTML parsing.

Example:

Plain text:
<img src=x onerror=alert(1)>

Obfuscated payload:
<img src=x onerror="&#x61;lert(1)">
