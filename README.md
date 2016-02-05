# tomcat-aws

Build routine to deploy a new webapp using Tomcat on AWS

## References
* https://tomcat.apache.org/tomcat-3.2-doc/tomcat-apache-howto.html
* https://tomcat.apache.org/connectors-doc/webserver_howto/apache.html
* http://programmers.stackexchange.com/questions/34959/how-are-apache-http-server-and-apache-tomcat-related-if-at-all
* https://www3.ntu.edu.sg/home/ehchua/programming/howto/ApachePlusTomcat_HowTo.html
* http://tomcat.apache.org/connectors-doc/
* https://aws.amazon.com/amazon-linux-ami/faqs/
* https://www.wikivs.com/wiki/DEB_vs_RPM
* http://httpd.apache.org/docs/
* https://tomcat.apache.org/tomcat-3.2-doc/uguide/tomcat_ug.html
* http://www.excelsior-usa.com/articles/tomcat-amazon-ec2-basic.html
* http://in4ray.blogspot.com/2012/04/install-tomcat-7-on-amazon-linux.html
* http://tomcat.apache.org/whichversion.html
* http://coenraets.org/blog/2011/11/set-up-an-amazon-ec2-instance-with-tomcat-and-mysql-5-minutes-tutorial/
* https://tomcat.apache.org/tomcat-7.0-doc/appdev/installation.html
* https://github.com/bachmeb/tomcat-aws
* https://tomcat.apache.org/tomcat-7.0-doc/appdev/source.html
* https://tomcat.apache.org/tomcat-7.0-doc/appdev/processes.html
* http://cs.lmu.edu/~ray/notes/jw/

##### Create a new vm
https://aws.amazon.com/ec2/

##### Step 1: Choose an Amazon Machine Image (AMI)
	Amazon Linux AMI 2015.09.1 (HVM), SSD Volume Type - ami-f0091d91

##### Step 2: Choose an Instance Type
	Family: General purpose
	Type: t2.micro
	vCPUs: 1
	Memory: 1
	Instance Storage: EBS only
	EBS-Optimized: -
	Network Performance: Low to Moderate

##### Step 3: Configure Instance Details
	Number of instances: 	1
	Network: (default)
	Subnet: No preference (default subnet in any Availability Zone)
	EBS-optimized: No
	Monitoring: No
	Termination protection: YES <---- UPDATE THIS
	Shutdown behavior: Stop
	IAM role: None
	Tenancy: default
	Host ID: 
	Affinity: Off
	Kernel ID: Use default
	RAM disk ID: Use default
	User data: 
	Assign Public IP: Use subnet setting (Enable)
	Network interfaces: 
	Purchasing option: On demand

##### Step 4: Add Storage
	Volume Type: Root
	Device: /dev/xvda
	Snapshot: snap-ad8e61f8
	Size (GiB): 8
	Volume Type: General Purpose SSD (GP2)
	IOPS: 24 / 3000
	Delete on Termination: Yes
	Encrypted: Not Encrypted

##### Step 5: Tag Instance
	Key: Name
	Value:

##### Step 6: Configure Security Group 
*Allow ICMP, SSH, and TCP traffic from your IP address*

    Type	    Protocol	Port Range	Source
    All TCP	    TCP	        0 - 65535	your ip address/32
    SSH	        TCP	        22	        your ip address/32
    All ICMP	All	        N/A         	your ip address/32

##### Download the key pair and change the mode to 400
	chmod 400 pemfile.pem

##### Ping the IP address to confirm that ICMP traffic is allowed from your IP address
	ping [ec2.ipa.ddr.ess]

##### Connect via SSH
	ssh -i pemfile.pem ec2-user@[ec2.ipa.ddr.ess]

##### Switch to root 
	sudo su

##### Ask who am I?
	whoami
	
##### Update yum
	yum update

##### Create a user account for development
	useradd [your new account name]
    
##### Set the password for your development account
	passwd [your new account name]

##### Edit sudoers file
	vim /etc/sudoers

##### Add development account to sudoers file
	## ALLOW {MYACCOUNT} TO SUDO
	[your new account name] ALL=(ALL) ALL

##### Switch to development user
	su [your new account name]
    
##### Ask who am I?
	whoami
    
##### Check the value of the home directory environment variable
	echo $HOME

##### Go home
	cd $HOME

##### Check the Linux distro version
	cat /proc/version

##### Check the hostname of the ec2 instance
	hostname

##### Check the DNS domain name of the ec2 instance
	dnsdomainname

##### Check the internal IP address
	ifconfig

##### Check the external IP address
	wget http://ipinfo.io/ip -qO -

##### Show all of the environment variables
	declare

##### Look for JAVA in the list of environmental variables
	env | grep JAVA

##### Echo the current JAVA_HOME
	echo $JAVA_HOME

##### Ask where is Java?
	whereis java

##### Check the Java version
	java -version

##### See if the Java compiler is installed
	whereis javac
	javac -version

##### Search yum for openjdk
	yum search openjdk
	
##### Install the Open JDK version 1.7
	sudo yum install java-1.7.0-openjdk-devel.x86_64

##### Check the Java compiler version
	javac -version

##### Tell Linux to use the Java interpreter in the JDK 1.7
	sudo /usr/sbin/alternatives --config java

##### Read the symlinks in /usr/lib/jvm/
	ls -l /usr/lib/jvm/

##### Confirm that /usr/lib/jvm/java points to etc/alternatives/java_sdk
	ls -l /usr/lib/jvm/java

##### Confirm that /etc/alternatives/java_sdk points to /usr/lib/jvm/java-1.7.0-openjdk.x86_64
	ls -l /etc/alternatives/java_sdk

##### Confirm that /usr/lib/jvm/java-1.7.0-openjdk.x86_64 points to /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64
	ls -l /usr/lib/jvm/java-1.7.0-openjdk.x86_64

##### Confirm that /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64 is the installation directory for Java SDK 1.7
	ls -l /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64

##### Echo the $JAVA_HOME environment variable
	echo $JAVA_HOME

##### Set the JAVA_HOME environment variable to the Open JDK directory
	export JAVA_HOME='/usr/lib/jvm/java'

##### Echo the $JAVA_HOME environment variable
	echo $JAVA_HOME

##### List the available tomcat packages
    yum --enablerepo="*" list available | grep tomcat

##### Search yum for tomcat7
    yum search tomcat7

##### Install Tomcat
    yum install tomcat7
    yum install tomcat7-webapps
    yum install tomcat7-admin-webapps

##### Ask where is tomcat7?
    whereis tomcat7

##### Get a listing of the Catalina Base directory
    ls -l /usr/share/tomcat7/
    
    total 4
    drwxr-xr-x 2 root root   4096 Jan 31 18:48 bin
    lrwxrwxrwx 1 root tomcat   12 Jan 31 18:48 conf -> /etc/tomcat7
    lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 lib -> /usr/share/java/tomcat7
    lrwxrwxrwx 1 root tomcat   16 Jan 31 18:48 logs -> /var/log/tomcat7
    lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 temp -> /var/cache/tomcat7/temp
    lrwxrwxrwx 1 root tomcat   24 Jan 31 18:48 webapps -> /var/lib/tomcat7/webapps
    lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 work -> /var/cache/tomcat7/work

##### Start Tomcat service
    service tomcat7 status
    service tomcat7 start
    service tomcat7 status

##### Check what is listening on port 8080
    lsof -ni:8080

##### Install Lynx
    yum install lynx

##### Go to the Tomcat welcome page
    lynx localhost:8080
    
##### Read the localhost access log
    cat /var/log/tomcat7/localhost_*
    
##### Add tomcat7 to autostart
    chkconfig
    chkconfig tomcat7 on 

##### Look for tomcat in the output of the process status command to see where the tomcat executable is running from
    ps -ef | grep tomcat

##### Read the Tomcat config file
    cd /usr/share/tomcat7/
    ls -l
    cat conf/tomcat7.conf
    
##### tomcat7.conf is where the CATALINA_BASE and CATALINA_HOME environment variables are set
```
# Where your java installation lives
JAVA_HOME="/usr/lib/jvm/jre"

# Where your tomcat installation lives
CATALINA_BASE="/usr/share/tomcat7"
CATALINA_HOME="/usr/share/tomcat7"
JASPER_HOME="/usr/share/tomcat7"
CATALINA_TMPDIR="/var/cache/tomcat7/temp"

# You can pass some parameters to java here if you wish to
#JAVA_OPTS="-Xminf0.1 -Xmaxf0.3"

# Use JAVA_OPTS to set java.library.path for libtcnative.so
#JAVA_OPTS="-Djava.library.path=/usr/lib"

# What user should run tomcat
TOMCAT_USER="tomcat"

# You can change your tomcat locale here
#LANG="en_US"

# Run tomcat under the Java Security Manager
SECURITY_MANAGER="false"

# Maximum time to wait in seconds, before killing process
SHUTDOWN_WAIT="30"

# Maximum time to wait in seconds, after killing the tomcat process
KILL_SLEEP_WAIT="5" 

# Whether to annoy the user with "attempting to shut down" messages or not
SHUTDOWN_VERBOSE="false"

# Set the TOMCAT_PID location
CATALINA_PID="/var/run/tomcat7.pid"

# Connector port is 8080 for this tomcat instance
#CONNECTOR_PORT="8080"

# If you wish to further customize your tomcat environment,
# put your own definitions here
# (i.e. LD_LIBRARY_PATH for some jdbc drivers)
```

##### Read the Tomcat init file
    less /etc/init.d/tomcat7

##### Read the Tomcat launch script
    less /usr/sbin/tomcat7

##### Read the server.xml file
    cat /usr/share/tomcat7/conf/server.xml

```xml

<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- Note:  A "Server" is not itself a "Container", so you may not
     define subcomponents such as "Valves" at this level.
     Documentation at /docs/config/server.html
 -->
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!--Initialize Jasper prior to webapps are loaded. Documentation at /docs/jasper-howto.html -->
  <Listener className="org.apache.catalina.core.JasperListener" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <!-- A "Service" is a collection of one or more "Connectors" that share
       a single "Container" Note:  A "Service" is not itself a "Container",
       so you may not define subcomponents such as "Valves" at this level.
       Documentation at /docs/config/service.html
   -->
  <Service name="Catalina">

    <!--The connectors can use a shared executor, you can define one or more named thread pools-->
    <!--
    <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
        maxThreads="150" minSpareThreads="4"/>
    -->


    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL HTTP/1.1 Connector on port 8080
    -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    -->
    <!-- Define a SSL HTTP/1.1 Connector on port 8443
         This connector uses the BIO implementation that requires the JSSE
         style configuration. When using the APR/native implementation, the
         OpenSSL style configuration is required as described in the APR/native
         documentation -->
    <!--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" />
    -->

    <!-- Define an AJP 1.3 Connector on port 8009 -->
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />


    <!-- An Engine represents the entry point (within Catalina) that processes
         every request.  The Engine implementation for Tomcat stand alone
         analyzes the HTTP headers included with the request, and passes them
         on to the appropriate Host (virtual host).
         Documentation at /docs/config/engine.html -->

    <!-- You should set jvmRoute to support load-balancing via AJP ie :
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="jvm1">
    -->
    <Engine name="Catalina" defaultHost="localhost">

      <!--For clustering, please take a look at documentation at:
          /docs/cluster-howto.html  (simple how to)
          /docs/config/cluster.html (reference documentation) -->
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->

      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <!-- This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm.  -->
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log." suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
    </Engine>
  </Service>
</Server>
```

##### Read the web.xml file
    less /usr/share/tomcat7/conf/web.xml 

##### grep the contents of the passwd file to see that a user named tomcat has been created
    cat /etc/passwd |grep tomcat

##### Create a user account for development
    useradd {myaccount}
    
##### Set the password for your development account
    passwd {myaccount}

##### Edit sudoers file
    nano /etc/sudoers

##### Add development account to sudoers file
    ## ALLOW {MYACCOUNT} TO SUDO
    {myaccount} ALL=(ALL) ALL

##### Switch to development user
    su {myaccount}

##### Edit Tomcat Users XML file
    sudo nano /usr/share/tomcat7/conf/tomcat7-users.xml

##### Add users

```xml
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>

  <role rolename="manager-script" />
  <role rolename="admin-script" />
  <user username="abc" password="123" roles="manager-script,admin-script" />

</tomcat-users>
```

##### Create a Build Properties file in developer's home directory
    cd ~
    nano build.properties

##### Add Catalina home and manager username and password
    catalina.home=/usr/share/tomcat7
    app.name=hello
    manager.username=abc
    manager.password=123

##### Check environment variable for home directory
    echo $HOME

##### Set permissions on home folder
    ls -l $HOME
    chmod -R 775 $HOME
    ls -l $HOME

##### Install git
    yum install git
	
##### Create project folder
    mkdir -p ~/git/projectfolder

##### Initialize project repository
    cd ~/git/projectfolder
    git init

##### Create remote repository
    http://github.com/

##### Configure repository
    git config user.name {username}
    git config user.email {emailaddress@example.com}

##### Add remote origin to local repository
    git remote add origin http://github.com/{username}/{projectname}.git

##### Fetch from remote
    git fetch

##### Pull from remote
    git pull origin master

##### Check status
    git status
    
##### Push to remote
    git push --set-upstream origin master

##### Create Build XML File
    cd ~/git/projectfolder
    wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/build.xml.txt
    cp build.xml.txt build.xml
    nano build.xml

##### Edit the XML file
```xml
<project name="MY PROJECT NAME" default="compile" basedir="."><!-- UPDATE THIS! -->

<!-- ===================== Property Definitions =========================== -->

  <property file="${user.home}/build.properties"/>

<!-- ==================== File and Directory Names ======================== -->

  <property name="app.path"      value="/${app.name}"/>
  <property name="app.version"   value="0.1-dev"/>

  <property name="build.home"    value="${basedir}/build"/>
  <property name="dist.home"     value="${basedir}/dist"/>
  <property name="docs.home"     value="${basedir}/docs"/>
  <property name="src.home"      value="${basedir}/src"/>
  <property name="web.home"      value="${basedir}/web"/>

  <property name="manager.url"   value="http://localhost:8080/manager/text"/>
  
<!-- ==================== External Dependencies =========================== -->
<!-- Dummy external dependency -->
<!--
  <property name="foo.jar"
           value="/path/to/foo.jar"/>
-->
<!-- ==================== Compilation Classpath =========================== -->
  <path id="compile.classpath">
    <fileset dir="${catalina.home}/bin">
      <include name="*.jar"/>
    </fileset>
    <pathelement location="${catalina.home}/lib"/>
    <fileset dir="${catalina.home}/lib">
      <include name="*.jar"/>
    </fileset>
  </path>
<!-- ================== Custom Ant Task Definitions ======================= -->
  <taskdef resource="org/apache/catalina/ant/catalina.tasks"
           classpathref="compile.classpath"/>
<!--  ==================== Compilation Control Options ==================== -->
  <property name="compile.debug"       value="true"/>
  <property name="compile.deprecation" value="false"/>
  <property name="compile.optimize"    value="true"/>
<!-- ==================== All Target ====================================== -->
  <target name="all" depends="clean,compile"
   description="Clean build and dist directories, then compile"/>
<!-- ==================== Clean Target ==================================== -->
  <target name="clean"
   description="Delete old build and dist directories">
    <delete dir="${build.home}"/>
    <delete dir="${dist.home}"/>
  </target>
<!-- ==================== Compile Target ================================== -->
  <target name="compile" depends="prepare"
   description="Compile Java sources">

    <!-- Compile Java classes as necessary --><!-- ADD THE includeantruntime PROPERTY -->
    <mkdir    dir="${build.home}/WEB-INF/classes"/>
    <javac srcdir="${src.home}"
includeantruntime="false"
          destdir="${build.home}/WEB-INF/classes"
            debug="${compile.debug}"
      deprecation="${compile.deprecation}"
         optimize="${compile.optimize}">
        <classpath refid="compile.classpath"/>
    </javac>
    <!-- Copy application resources -->
    <copy  todir="${build.home}/WEB-INF/classes">
      <fileset dir="${src.home}" excludes="**/*.java"/>
    </copy>
  </target>
<!-- ==================== Dist Target ===================================== -->
  <target name="dist" depends="compile,javadoc"
   description="Create binary distribution">
    <!-- Copy documentation subdirectories -->
    <mkdir   dir="${dist.home}/docs"/>
    <copy    todir="${dist.home}/docs">
      <fileset dir="${docs.home}"/>
    </copy>
    <!-- Create application JAR file -->
    <jar jarfile="${dist.home}/${app.name}-${app.version}.war"
         basedir="${build.home}"/>
    <!-- Copy additional files to ${dist.home} as necessary -->
  </target>
<!-- ==================== Install Target ================================== -->
  <target name="install" depends="compile"
   description="Install application to servlet container">
    <deploy url="${manager.url}"
       username="${manager.username}"
       password="${manager.password}"
           path="${app.path}"
       localWar="file://${build.home}"/>
  </target>
<!-- ==================== Javadoc Target ================================== -->
  <target name="javadoc" depends="compile"
   description="Create Javadoc API documentation">
    <mkdir          dir="${dist.home}/docs/api"/>
    <javadoc sourcepath="${src.home}"
                destdir="${dist.home}/docs/api"
           packagenames="*">
      <classpath refid="compile.classpath"/>
    </javadoc>
  </target>
<!-- ====================== List Target =================================== -->
  <target name="list"
   description="List installed applications on servlet container">

    <list    url="${manager.url}"
        username="${manager.username}"
        password="${manager.password}"/>
  </target>
<!-- ==================== Prepare Target ================================== -->
  <target name="prepare">
    <!-- Create build directories as needed -->
    <mkdir  dir="${build.home}"/>
    <mkdir  dir="${build.home}/WEB-INF"/>
    <mkdir  dir="${build.home}/WEB-INF/classes"/>
    <!-- Copy static content of this web application -->
    <copy todir="${build.home}">
      <fileset dir="${web.home}"/>
    </copy>
    <!-- Copy external dependencies as required -->
    <!-- *** CUSTOMIZE HERE AS REQUIRED BY YOUR APPLICATION *** -->
    <mkdir  dir="${build.home}/WEB-INF/lib"/>

    <!-- Copy static files from external dependencies as needed -->
    <!-- *** CUSTOMIZE HERE AS REQUIRED BY YOUR APPLICATION *** -->
  </target>
<!-- ==================== Reload Target =================================== -->
  <target name="reload" depends="compile"
   description="Reload application on servlet container">
    <reload url="${manager.url}"
       username="${manager.username}"
       password="${manager.password}"
           path="${app.path}"/>
  </target>
<!-- ==================== Remove Target =================================== -->
  <target name="remove"
   description="Remove application on servlet container">
    <undeploy url="${manager.url}"
         username="${manager.username}"
         password="${manager.password}"
             path="${app.path}"/>
  </target>
</project>

```
##### Create Web XML File
    cd ~/git/projectfolder/web/WEB-INF
    wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/web.xml.txt
    cp web.xml.txt web.xml
    nano web.xml

##### Edit the XML file
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE web-app
    PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd">
<web-app>
    <display-name>Hi There</display-name>
    <servlet>
        <servlet-name>greeting</servlet-name>
        <servlet-class>com.example.words.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>greeting</servlet-name>
        <url-pattern>/there</url-pattern>
    </servlet-mapping>
</web-app>
```

##### Install ant
    yum install ant

##### List installed projects
    ant -v list

##### Prepare the project
    ant -v prepare

##### Compile the project
    ant -v compile

##### Install the project
    ant -v install

##### Reload the project
    ant -v reload

##### Remove the project
    ant -v remove

##### Clean the project build directory
    ant -v clean

##### Clean, prepare, and compile the project
    ant -v all

##### Create an index file
    cd ~/git/projectfolder/web/
    nano index.html

##### Edit the XML file
```xml
Hello <a href="./there">There</a>.
```
##### Make a Java class
    mkdir -p src/com/example/words
    nano -i src/com/example/words/HelloServlet.java
  
##### Code the class
```java
package com.example.cup;

import java.io.IOException;
import java.io.PrintWriter;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response)$
        PrintWriter out = response.getWriter();
        out.println("What's up?");
        out.close();
    }
    
}
```
##### Reinstall the project
    ant remove all install

##### Use a web browser to view the web application
    http://[ec2.ipa.ddr.ess]:8080/hello
