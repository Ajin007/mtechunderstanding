## what is webservice?
## what is servlet?
## Why json over SOAP?
## what is the advantage of using the xml than json?
## Quick setup for the servlet installation:
~~~
# 1: generate maven project ---> mvn archetype:generate
# 2: filter any on archtype from the list --->
      Commonly Available Maven Archetypes (Apache/Default Catalog) 
      maven-archetype-quickstart: Generates a simple, standard Maven project (Java JAR).
      maven-archetype-webapp: Creates a basic Web Application (WAR) structure.
      maven-archetype-archetype: Used to create a new, custom archetype.
      maven-archetype-plugin: Generates a template for building a Maven Plugin.
      maven-archetype-site: Creates a sample Maven site.
      maven-archetype-simple: A very simple Java project template.
      maven-archetype-j2ee-simple: A template for a basic J2EE application.
# 3: select sample project
# 4: select the version
# 5: select the group-ID:
    ---> Example :

      groupId    → City / Organization
    artifactId → Building name
    version    → Floor number
# 6: when select the verison the things to remember in the same:
    | Situation              | Version        |
    | ---------------------- | -------------- |
    | Learning / development | `1.0-SNAPSHOT` |
    | First stable release   | `1.0.0`        |
    | Bug fixes              | `1.0.1`        |
    | New features           | `1.1.0`        |
    | Major changes          | `2.0.0`        |
# 7: What is the difference between the groupId and package :
| Field       | Purpose                                      |
| ----------- | -------------------------------------------- |
| **groupId** | Identifies the project in Maven repositories |
| **package** | Java namespace for your source code          |
# 8: why WEB-INF file ?
In early web apps, people accidentally exposed:
.class files
configuration files
DB credentials
internal JSPs
So Java EE said:
“Anything critical must live in a folder the browser cannot touch.”
That folder is WEB-INF.

# 9: what is web.xml ?
web.xml is the deployment descriptor — it tells Tomcat how your app behaves.
It is the brain/config file of the web app.
Before annotations existed, everything was defined here:

Which servlet maps to which URL
Filters
Listeners
Welcome pages
Security rules
-----> Modern usage (@WebServlet("/hello")

# 10: 
~~~
## Simple project using the servlet
~~~
1. How servlet runs ?
Servlet  → runs INSIDE → Tomcat
WAR      → deployed TO → Tomcat
2. Tomcat:

Provides ServletContainer
Manages lifecycle (init → service → destroy)
Handles HTTP server

3. How to run the jar?
Embedded Tomcat ---> Spring boot
Embedded Jetty ---> Dropwizard
Embedded Undertow --> Micronaut
~~~
## Project creation
### POM.xml
do not forget to change the packaging to war if need to runusing the war file with seperate tomcat server
~~~
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>com.iamneo.ai</groupId>
  <artifactId>servletunderstanding</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>

    <!-- Jetty 11 uses jakarta.servlet 5 -->
    <jetty.version>11.0.17</jetty.version>
  </properties>

  <dependencies>
    <!-- Jetty Embedded Server -->
    <dependency>
      <groupId>org.eclipse.jetty</groupId>
      <artifactId>jetty-server</artifactId>
      <version>${jetty.version}</version>
    </dependency>

    <dependency>
      <groupId>org.eclipse.jetty</groupId>
      <artifactId>jetty-servlet</artifactId>
      <version>${jetty.version}</version>
    </dependency>

    <!-- Servlet API -->
    <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>5.0.0</version>
    </dependency>

    <!-- Tests -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>servletunderstanding</finalName>

    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.13.0</version>
      </plugin>

      <!-- Makes the JAR runnable: java -jar ... -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.5.3</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <createDependencyReducedPom>false</createDependencyReducedPom>
              <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                  <mainClass>com.iamneo.ai.App</mainClass>
                </transformer>
              </transformers>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>

</project>

~~~

### HelloServlet.java
~~~
package com.iamneo.ai;


import java.io.IOException;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
     resp.setContentType("text/plain");
     resp.getWriter().println("Servlet running in the jar mode");

    }

}

~~~
### App.java
~~~

package com.iamneo.ai;

import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.servlet.ServletContextHandler;
import org.eclipse.jetty.servlet.ServletHolder;

public class App {

    public static void main (String[] args) throws Exception{

        // server starts here
        Server server=new Server(8080);

        // first entry 
        ServletContextHandler context=new ServletContextHandler(ServletContextHandler.SESSIONS);
        context.setContextPath("/");
        context.addServlet(new ServletHolder(new HelloServlet()), "/hello");
        server.setHandler(context);
        server.start();
        System.out.println("server started at 8080");
        server.join();

    }

}

~~~
