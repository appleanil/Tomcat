# Tomcat Interview Questions and Answers

## 1. Can you please explain Tomcat? Where are you using it?

### Answer

Apache Tomcat is an open-source Java web server and Servlet/Jakarta Servlet container developed by the Apache Software Foundation. It is used to deploy and run Java web applications.

Tomcat supports technologies such as:
- Java Servlets
- JavaServer Pages (JSP)
- WebSocket
- HTTP request handling

### Where do we use Tomcat in a project?

In a DevOps project, we use Tomcat to deploy Java web applications packaged as WAR files.

**Example workflow:**

1. Developers write Java application code.
2. Jenkins builds the application using Maven.
3. Maven generates a WAR file.
4. The WAR file is deployed to Tomcat.
5. Tomcat starts the application and serves HTTP requests.
6. Users access the application through a browser or load balancer.

### Useful commands

```bash
# Check Tomcat service status
sudo systemctl status tomcat

# Start Tomcat
sudo systemctl start tomcat

# Stop Tomcat
sudo systemctl stop tomcat

# Restart Tomcat
sudo systemctl restart tomcat

# Check Tomcat logs
sudo tail -f /var/log/tomcat/catalina.out
```

*Note: Service names and log locations depend on how Tomcat was installed.*

---

## 2. How do you run JAR, WAR, and EAR files? Explain in detail.

### JAR file

JAR stands for Java Archive. It contains compiled Java classes and related resources.

An executable JAR often contains a `Main-Class` entry in its manifest.

**Command:**

```bash
java -jar application.jar
```

If it is a Spring Boot executable JAR, it commonly starts an embedded Tomcat server when the application includes the appropriate dependency.

**Example:**

```bash
java -jar target/springboot-app.jar
```

### WAR file

WAR stands for Web Application Archive. It packages a Java web application, including its classes, dependencies, and web resources.

A WAR file can be deployed to a compatible Servlet container such as Tomcat.

**Method 1: Deploy through the webapps directory**

```bash
cp application.war /opt/tomcat/webapps/
```

Tomcat normally detects the WAR file and deploys it automatically when the Host's automatic deployment settings permit it.

Access the application at:

```text
http://<server-ip>:8080/application/
```

**Method 2: Deploy through the Tomcat Manager UI**

1. Open the Tomcat Manager application.
2. Authenticate with an authorized account.
3. Upload the WAR file through the deployment section.
4. Verify the application status.
5. Open the application URL.

The Manager application must be installed and properly configured, and its access restrictions must permit your connection.

### EAR file

EAR stands for Enterprise Archive. It packages enterprise Java applications, potentially containing multiple web modules, EJB modules, and shared libraries.

EAR deployment is commonly associated with full Jakarta EE application servers such as JBoss EAP or WildFly.

**Important:** Standard Apache Tomcat does not natively deploy EAR files. An EAR generally requires a compatible full application server.

### Quick comparison

| File type | Full form | Typical runtime |
|---|---|---|
| JAR | Java Archive | Java runtime; sometimes an embedded server |
| WAR | Web Application Archive | Tomcat or a compatible application server |
| EAR | Enterprise Archive | Compatible enterprise application server |

### Interview answer

I use JAR files to run executable Java applications, WAR files to deploy web applications to Tomcat, and EAR files to deploy enterprise applications to full application servers such as WildFly or JBoss EAP.

---

## 3. What is the difference between Tomcat and JBoss/WildFly?

Apache Tomcat is primarily a Servlet container, whereas WildFly and JBoss EAP provide a broader set of enterprise Java and Jakarta EE capabilities.

| Feature | Apache Tomcat | WildFly / JBoss EAP |
|---|---|---|
| Main purpose | Web applications and Servlet container | Full enterprise application server |
| Servlets and JSP | Supported | Supported |
| Enterprise features | Limited compared with a full application server | Broader enterprise APIs and services |
| EJB support | Not natively provided | Supported |
| Messaging services | Usually requires additional components | Enterprise messaging capabilities available, depending on configuration |
| Deployment | Commonly WAR files | WAR, EAR, and other supported deployments |
| Resource management | Primarily web-container features | Broader enterprise resource management |
| Complexity | Generally simpler | Generally more feature-rich |
| Typical use | Spring MVC and Java web applications | Enterprise applications requiring additional Jakarta EE services |

### When do we use Tomcat?

- Java web applications.
- Spring MVC applications.
- Spring Boot applications deployed as WAR files.
- Applications that need a lightweight Servlet container.

### When do we use WildFly or JBoss EAP?

- Enterprise applications requiring EJB.
- Applications requiring broader Jakarta EE APIs.
- Applications packaged as EAR files.
- Systems requiring application-server-managed enterprise services.

**Note:** JBoss EAP is Red Hat's supported enterprise application server product, while WildFly is its upstream community project.

---

## 4. Which version of Tomcat are you using in your project?

### Sample interview answer

In my project, I use Apache Tomcat **[insert your actual version]** to deploy and run Java web applications. I verify compatibility with the application's Java version and Servlet/Jakarta Servlet API requirements before deploying.

### How to check the Tomcat version

**Method 1: Using the version script**

```bash
/opt/tomcat/bin/version.sh
```

**Method 2: From the installation directory**

```bash
cd /opt/tomcat/bin
./version.sh
```

**Method 3: Check the startup logs**

```bash
sudo journalctl -u tomcat --no-pager | grep -i "Server version"
```

If Tomcat was installed manually, the startup logs may instead be available under the Tomcat `logs` directory.

### How to answer confidently

Do not guess the version during an interview. Check the actual environment and state the version you used, the Java version, and how the application was deployed.

---

## 5. Can you explain the Tomcat directory structure?

A typical Apache Tomcat installation contains the following directories:

```text
apache-tomcat/
├── bin/
├── conf/
├── lib/
├── logs/
├── temp/
├── webapps/
├── work/
└── NOTICE
```

### Directory explanation

| Directory | Purpose |
|---|---|
| `bin/` | Startup, shutdown, and administration scripts |
| `conf/` | Server configuration files |
| `lib/` | Libraries used by the Tomcat server |
| `logs/` | Server and application-related logs |
| `temp/` | Temporary files |
| `webapps/` | Applications deployed to Tomcat |
| `work/` | Generated files, including compiled JSP artifacts |
| `NOTICE` | Notices about included software and licensing |

### Important configuration files

**`server.xml`**

```text
conf/server.xml
```

Configures connectors, ports, Hosts, Engines, and other server components.

**`web.xml`**

```text
conf/web.xml
```

Provides default web application configuration.

**`tomcat-users.xml`**

```text
conf/tomcat-users.xml
```

Can define users and roles for supported Tomcat management applications.

**`context.xml`**

```text
conf/context.xml
```

Provides default context configuration.

### Example deployment

If you copy a file named `myapp.war` into:

```text
/opt/tomcat/webapps/
```

Tomcat can deploy the application under the context path:

```text
/myapp
```

The application URL would typically be:

```text
http://<server-ip>:8080/myapp/
```

This assumes the default HTTP connector port and a successful deployment.

---

## 6. What is the default port number of Tomcat?

The default HTTP connector port in a standard Tomcat configuration is **8080**.

Other common ports include:

| Port | Typical purpose |
|---|---|
| 8080 | HTTP connector |
| 8443 | HTTPS connector, if configured |
| 8005 | Traditional shutdown port in older/default configurations; may be disabled or changed |
| 8009 | AJP connector, if enabled and configured |

The actual ports depend on the Tomcat version and configuration.

### How to check the port

```bash
grep -n 'Connector' /opt/tomcat/conf/server.xml
```

Look for a connector such as:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

## 7. I installed Tomcat, but I cannot connect through the UI. How do you troubleshoot?

### Step 1: Check whether Tomcat is running

```bash
sudo systemctl status tomcat
```

If the service is stopped:

```bash
sudo systemctl start tomcat
```

### Step 2: Check the Tomcat logs

```bash
sudo journalctl -u tomcat -n 100 --no-pager
```

For a manual installation:

```bash
sudo tail -n 100 /opt/tomcat/logs/catalina.out
```

Look for Java errors, port conflicts, deployment failures, or configuration problems.

### Step 3: Check whether Tomcat is listening on port 8080

```bash
sudo ss -lntp | grep ':8080'
```

If no process is listening, check the Tomcat service, connector configuration, and startup logs.

### Step 4: Test locally on the server

```bash
curl -I http://localhost:8080/
```

If local access works but remote access fails, investigate networking and access controls.

### Step 5: Check the firewall

On Ubuntu with UFW:

```bash
sudo ufw status
```

If required and authorized:

```bash
sudo ufw allow 8080/tcp
```

On RHEL-based systems, check the configured firewalld rules:

```bash
sudo firewall-cmd --list-all
```

### Step 6: Check AWS Security Groups

If Tomcat runs on an EC2 instance, verify that the inbound rules permit TCP port 8080 from the intended client network.

For example:

| Type | Protocol | Port | Source |
|---|---|---|---|
| Custom TCP | TCP | 8080 | Your trusted client IP/CIDR |

Avoid opening port 8080 to `0.0.0.0/0` unnecessarily. For production, it is often better to expose HTTPS through a load balancer or reverse proxy.

### Step 7: Check the bind address

Inspect the connector in `server.xml`:

```bash
grep -n 'Connector' /opt/tomcat/conf/server.xml
```

A connector configured with a loopback-only address may not accept remote connections. Check the `address` attribute if one is present.

### Step 8: Verify the application URL

For the Tomcat root page:

```text
http://<server-ip>:8080/
```

For an application named `myapp`:

```text
http://<server-ip>:8080/myapp/
```

The Tomcat Manager UI is separate and may be disabled or restricted by default.

### Troubleshooting summary

| Symptom | What to check |
|---|---|
| Connection refused | Tomcat status, listening port, bind address |
| Connection timed out | Security group, firewall, routing, network ACL |
| HTTP 404 | URL, deployment status, application context path |
| HTTP 403 | Access restrictions, application permissions, Manager configuration |
| HTTP 500 | Application logs, Java exceptions, dependencies |
| Port already in use | Identify the process using the port |

### Interview answer

I would check the Tomcat service first, then inspect the logs and confirm that port 8080 is listening. Next, I would test with `curl` locally and verify the firewall, AWS Security Group, bind address, and application URL. I would use the error code and logs to identify the root cause.

---

## 8. How can you check whether Tomcat is running on port 8080?

### Method 1: Using `ss`

```bash
sudo ss -lntp | grep ':8080'
```

Example output:

```text
LISTEN 0 100 *:8080 *:* users:(("java",pid=1234,fd=45))
```

This indicates that a process is listening on port 8080.

### Method 2: Using `lsof`

```bash
sudo lsof -iTCP:8080 -sTCP:LISTEN
```

### Method 3: Using `curl`

```bash
curl -I http://localhost:8080/
```

A response such as `HTTP/1.1 200` indicates that an HTTP endpoint responded successfully. Other HTTP status codes may also indicate that the server is reachable.

### Method 4: Check the Service

```bash
sudo systemctl status tomcat
```

**Important:** A process listening on port 8080 is not automatically Tomcat. Verify the process name and PID if necessary.

---

## 9. How do you change the Tomcat port number, and why would you change it?

### Why change the port?

- Avoid conflicts with another application.
- Match organizational network standards.
- Run multiple Tomcat instances on the same host using different ports.
- Integrate with a reverse proxy or load balancer.
- Meet specific application or infrastructure requirements.

Changing the port alone does not improve security.

### Step 1: Open `server.xml`

For a manual installation:

```bash
sudo vi /opt/tomcat/conf/server.xml
```

For package installations, the file may be located elsewhere.

### Step 2: Find the HTTP Connector

Default configuration:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

### Step 3: Change the Port

For example, change `8080` to `9090`:

```xml
<Connector port="9090" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

### Step 4: Restart Tomcat

```bash
sudo systemctl restart tomcat
```

For a manual installation, use the appropriate service management or startup scripts.

### Step 5: Verify the New Port

```bash
sudo ss -lntp | grep ':9090'
curl -I http://localhost:9090/
```

Also update the applicable firewall, Security Group, reverse proxy, health checks, and monitoring configuration.

### Interview answer

I change the HTTP connector's `port` attribute in `conf/server.xml`, restart Tomcat, and verify the new listener with `ss` and `curl`. I also update any firewall, load balancer, or monitoring rules that depend on the previous port.

---

## 10. What is the difference between a web server and an application server?

### Web Server

A web server primarily handles HTTP requests and serves static content, such as HTML, CSS, JavaScript, and images. It can also reverse-proxy requests to backend applications.

**Examples:**
- Apache HTTP Server (`httpd`)
- Nginx

### Application Server

An application server provides a runtime for application code and may offer enterprise features such as transactions, messaging, dependency injection, and enterprise resource management.

**Examples:**
- WildFly
- JBoss EAP
- WebLogic
- WebSphere

Tomcat is primarily a web server and Servlet container rather than a full Jakarta EE application server.

### Comparison table

| Feature | Web server | Application server |
|---|---|---|
| Primary purpose | Serve HTTP content and proxy requests | Run application code and provide runtime services |
| Static content | Yes | Often supported |
| Dynamic application execution | Can proxy to application runtimes | Yes |
| Enterprise services | Usually limited | Often extensive |
| Example | Apache HTTP Server, Nginx | WildFly, JBoss EAP, WebLogic |

### Real-time DevOps example

A common architecture is:

```text
User Browser
     |
     v
Load Balancer
     |
     v
Apache HTTP Server / Nginx
     |
     v
Tomcat
     |
     v
Java Web Application
     |
     v
Database
```

Apache HTTP Server or Nginx can terminate TLS and reverse-proxy requests to Tomcat. Tomcat runs the Java web application, which communicates with the database.

This is a common architecture, not a mandatory requirement for every project.

### Interview answer

A web server primarily handles HTTP requests, serves static files, and can proxy requests. An application server provides an execution environment for application code and additional services. Tomcat occupies a middle ground because it handles HTTP and serves Java web applications through its Servlet container, but it does not provide all the capabilities of a full Jakarta EE application server.

---

## 11. What is the default directory of Apache HTTP Server (`httpd`)?

The default configuration and document-root locations depend on the Linux distribution.

| Item | RHEL / CentOS / Amazon Linux (common defaults) | Ubuntu / Debian (common defaults) |
|---|---|---|
| Main configuration | `/etc/httpd/conf/httpd.conf` | `/etc/apache2/apache2.conf` |
| Additional configuration | `/etc/httpd/conf.d/` | `/etc/apache2/conf-enabled/`, `/etc/apache2/sites-enabled/` |
| Default document root | `/var/www/html/` | `/var/www/html/` |
| Logs | `/var/log/httpd/` | `/var/log/apache2/` |
| Service name | `httpd` | `apache2` |

### Check the document root

On RHEL-based systems:

```bash
grep -i 'DocumentRoot' /etc/httpd/conf/httpd.conf
```

On Ubuntu/Debian:

```bash
grep -R 'DocumentRoot' /etc/apache2/sites-enabled/
```

### Check the service

RHEL-based systems:

```bash
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl restart httpd
```

Ubuntu/Debian:

```bash
sudo systemctl status apache2
sudo systemctl start apache2
sudo systemctl restart apache2
```

### Test the web server

Create a simple HTML page:

```bash
echo "Welcome to Apache HTTP Server" | sudo tee /var/www/html/index.html
```

Test locally:

```bash
curl http://localhost/
```

If the page is accessible locally but not remotely, check the firewall, network routing, cloud Security Group, and web server configuration.

---

# Quick Revision: Important Commands

| Task | Command |
|---|---|
| Check Tomcat service | `sudo systemctl status tomcat` |
| Start Tomcat | `sudo systemctl start tomcat` |
| Restart Tomcat | `sudo systemctl restart tomcat` |
| Check Tomcat logs | `sudo tail -f /opt/tomcat/logs/catalina.out` |
| Check listening port | `sudo ss -lntp \| grep ':8080'` |
| Test HTTP endpoint | `curl -I http://localhost:8080/` |
| Check Tomcat version | `/opt/tomcat/bin/version.sh` |
| Deploy WAR | `cp application.war /opt/tomcat/webapps/` |
| Check Apache service on RHEL | `sudo systemctl status httpd` |
| Check Apache service on Ubuntu | `sudo systemctl status apache2` |
| Check Apache document root | `grep -i DocumentRoot /etc/httpd/conf/httpd.conf` |

**Final interview tip:** For every troubleshooting question, explain the command, the expected output, the possible root cause, the fix, and how you verified the application afterward.
