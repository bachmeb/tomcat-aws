# tomcat-aws

Build routine to deploy a new webapp using Tomcat on AWS

##### [References](/docs/references.md)

##### Create a new vm
https://aws.amazon.com/ec2/

##### Configure the security group to allow ICMP, SSH, and TCP traffic from your IP address
    Type	    Protocol	Port Range	Source
    All TCP	    TCP	        0 - 65535	your ip address/32
    SSH	        TCP	        22	        your ip address/32
    All ICMP	All	        N/A         your ip address/32

##### Download the key pair and change the mode to 400
    chmod 400 pemfile.pem

##### Ping the IP address to confirm that ICMP traffic is allowed from your IP address
    ping [ec2.ipa.ddr.ess]

##### Update the DNS record at the domain registrar to point the domain name to the new IP address
    ; A Records
    @ 600 IN A [000.000.000.000] <-- The EC2 IP address
    mail 600 IN A [000.000.000.000] <-- The EC2 IP address
    
    ; CNAME Records
    ftp 600 IN CNAME @
    www 600 IN CNAME @
    
    ; MX Records
    @ 600 IN MX 1 [mail.somedomain.whatever]

##### Connect via SSH
    ssh -i pemfile.pem ec2-user@[ec2.ipa.ddr.ess]

##### Switch to root 
    sudo su

##### Check the Linux distro version
    cat /proc/version

##### Edit the sysconfig network file
    nano /etc/sysconfig/network
    
##### Change the hostname and save the file
    HOSTNAME=[subddomain?].[somedomain.whatever]

##### Change the command prompt to show the full hostname
    export PS1='[\u@\H \W]\$'

##### Reboot
    reboot

##### Ping the domain name to confirm the DNS update.
    ping [somedomain.whatever]

##### Reconnect
    ssh -i [pem file] ec2-user@[somedomain.whatever]

##### Check the hostname of the ec2 instance
    hostname

##### Check the DNS domain name of the ec2 instance
    dnsdomainname

##### Switch to root
    sudo su

##### Stop Sendmail
    /etc/init.d/sendmail stop

##### Update yum
    yum update

##### List the available tomcat packages
    yum --enablerepo="*" list available | grep tomcat

##### Search yum for tomcat7
    yum search tomcat7

##### Install Tomcat
    yum install tomcat7
    yum install tomcat7-webapps

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
    
##### Add tomcat7 to autostart
    chkconfig
    chkconfig tomcat7 on 

##### Create a user account for development
    useradd {myaccount}

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

##### Set permissions on home folder
    ls -l /home/{myaccount}
    chmod -R 775 /home/{myaccount}
    ls -l /home/{myaccount}

##### Check environment variable for home directory
    echo $HOME

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
    git pull

##### Push to remote
    git push

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
    ant all; ant remove; ant install

##### Use a web browser to view the web application
    http://[ec2.ipa.ddr.ess]:8080/hello
