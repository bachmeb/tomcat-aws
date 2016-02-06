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
	:$

##### Add development account to sudoers file
	## ALLOW ME TO SUDO
	[your new account name] ALL=(ALL) ALL

##### Switch to development user
	su [your new account name]
    
##### Ask who am I?
	whoami
    
##### Check the value of the home directory environment variable
	echo $HOME

##### Go home
	cd ~

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
	sudo yum install java-1.7.0-openjdk-devel

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

##### List the available tomcat packages in yum
    yum --enablerepo="*" list available | grep tomcat

##### Search yum for tomcat7
    yum search tomcat7

##### Install Tomcat
    sudo yum install tomcat7 tomcat7-webapps tomcat7-admin-webapps

##### Ask where is tomcat7?
    whereis tomcat7

##### Get a listing of the Catalina Base directory
    ls -l /usr/share/tomcat7/
```    
total 4
drwxr-xr-x 2 root root   4096 Jan 31 18:48 bin
lrwxrwxrwx 1 root tomcat   12 Jan 31 18:48 conf -> /etc/tomcat7
lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 lib -> /usr/share/java/tomcat7
lrwxrwxrwx 1 root tomcat   16 Jan 31 18:48 logs -> /var/log/tomcat7
lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 temp -> /var/cache/tomcat7/temp
lrwxrwxrwx 1 root tomcat   24 Jan 31 18:48 webapps -> /var/lib/tomcat7/webapps
lrwxrwxrwx 1 root tomcat   23 Jan 31 18:48 work -> /var/cache/tomcat7/work
```

##### Check what is listening on port 8080
    sudo lsof -ni:8080
    
##### Start Tomcat service
    sudo service tomcat7 status
    sudo service tomcat7 start
    sudo service --status-all | grep tomcat

##### Grep the output of the process status command for tomcat
	ps -ef | grep tomcat

##### Check what is listening on port 8080
    sudo lsof -ni:8080

##### Install Lynx
    sudo yum install lynx

##### Go to the Tomcat welcome page
    lynx localhost:8080
```
Apache Tomcat/7.0.65
If you're seeing this, you've successfully installed Tomcat. Congratulations!
```
    
##### List the contents of the Tomcat logs directory
	sudo ls -l /usr/share/tomcat7/logs/
	
##### Read the catalina.out file
	sudo cat /usr/share/tomcat7/logs/catalina.out

##### Read the localhost access log
	sudo find /usr/share/tomcat7/logs/ -name localhost* -exec cat {} \;

##### Add tomcat7 to autostart
	chkconfig
	chkconfig | grep tomcat
	sudo chkconfig tomcat7 on 
	chkconfig | grep tomcat

##### Read the Tomcat config file
	less /usr/share/tomcat7/conf/tomcat7.conf
*This file is where the CATALINA_BASE and CATALINA_HOME environment variables are set*    
```
# System-wide configuration file for tomcat services
# This will be sourced by tomcat and any secondary service
# Values will be overridden by service-specific configuration
# files in /etc/sysconfig
#
# Use this one to change default values for all services
# Change the service specific ones to affect only one service
# (see, for instance, /etc/sysconfig/tomcat7)
#

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
```bash
#!/bin/bash
#
# tomcat      This shell script takes care of starting and stopping Tomcat
#
# chkconfig: - 80 20
#
### BEGIN INIT INFO
# Provides: tomcat
# Required-Start: $network $syslog
# Required-Stop: $network $syslog
# Default-Start:
# Default-Stop:
# Description: Release implementation for Servlet 3.0 and JSP 2.2
# Short-Description: start and stop tomcat
### END INIT INFO
#
# - originally written by Henri Gomez, Keith Irwin, and Nicolas Mailhot
# - heavily rewritten by Deepak Bhole and Jason Corley
#

## Source function library.
. /etc/rc.d/init.d/functions

NAME="$(basename $0)"

unset ISBOOT
if [ "${NAME:0:1}" = "S" -o "${NAME:0:1}" = "K" ]; then
    NAME="${NAME:3}"
    ISBOOT="1"
fi

# For SELinux we need to use 'runuser' not 'su'
if [ -x "/sbin/runuser" ]; then
    SU="/sbin/runuser -s /bin/sh"
else
    SU="/bin/su -s /bin/sh"
fi

# Get the tomcat config (use this for environment specific settings)
TOMCAT_CFG="/etc/tomcat7/tomcat7.conf"
if [ -r "$TOMCAT_CFG" ]; then
    . $TOMCAT_CFG
fi

# Get instance specific config file
if [ -r "/etc/sysconfig/${NAME}" ]; then
    . /etc/sysconfig/${NAME}
fi

# Define which connector port to use
CONNECTOR_PORT="${CONNECTOR_PORT:-8080}"

# Path to the tomcat launch script
TOMCAT_SCRIPT="/usr/sbin/tomcat7"

# Tomcat program name
TOMCAT_PROG="${NAME}"

# Define the tomcat username
TOMCAT_USER="${TOMCAT_USER:-tomcat}"

# Define the tomcat log file
TOMCAT_LOG="${TOMCAT_LOG:-${CATALINA_HOME}/logs/${NAME}-initd.log}"

# set kill timeout
KILL_SLEEP_WAIT="${KILL_SLEEP_WAIT:-5}"

RETVAL="0"

# Look for open ports, as the function name might imply
function findFreePorts() {
    local isSet1="false"
    local isSet2="false"
    local isSet3="false"
    local lower="8000"
    randomPort1="0"
    randomPort2="0"
    randomPort3="0"
    local -a listeners="( $(
                        netstat -ntl | \
                        awk '/^tcp/ {gsub("(.)*:", "", $4); print $4}'
                    ) )"
    while [ "$isSet1" = "false" ] || \
          [ "$isSet2" = "false" ] || \
          [ "$isSet3" = "false" ]; do
        let port="${lower}+${RANDOM:0:4}"
        if [ -z `expr " ${listeners[*]} " : ".*\( $port \).*"` ]; then
            if [ "$isSet1" = "false" ]; then
                export randomPort1="$port"
                isSet1="true"
            elif [ "$isSet2" = "false" ]; then
                export randomPort2="$port"
                isSet2="true"
            elif [ "$isSet3" = "false" ]; then
                export randomPort3="$port"
                isSet3="true"
            fi
        fi
    done
}

function makeHomeDir() {
    if [ ! -d "$CATALINA_HOME" ]; then
        echo "$CATALINA_HOME does not exist, creating"
        if [ ! -d "/usr/share/${NAME}" ]; then
            mkdir /usr/share/${NAME}
            cp -pLR /usr/share/tomcat/* /usr/share/${NAME}
        fi
        mkdir -p /var/log/${NAME} \
                 /var/cache/${NAME} \
                 /var/tmp/${NAME}
        ln -fs /var/cache/${NAME} ${CATALINA_HOME}/work
        ln -fs /var/tmp/${NAME} ${CATALINA_HOME}/temp
        cp -pLR /usr/share/${NAME}/bin $CATALINA_HOME
        cp -pLR /usr/share/${NAME}/conf $CATALINA_HOME
        ln -fs /usr/share/java/tomcat ${CATALINA_HOME}/lib
        ln -fs /usr/share/tomcat/webapps ${CATALINA_HOME}/webapps
        chown ${TOMCAT_USER}:${TOMCAT_USER} /var/log/${NAME}
    fi
}

function parseOptions() {
    options="export TOMCAT_NAME=${NAME};export TOMCAT_CFG=${TOMCAT_CFG};"
    options="$options $(
                 awk '!/^#/ && !/^$/ { ORS=" "; print "export ", $0, ";" }' \
                 $TOMCAT_CFG
             )"
    if [ -r "/etc/sysconfig/${NAME}" ]; then
        options="$options $(
                     awk '!/^#/ && !/^$/ { ORS=" ";
                                           print "export ", $0, ";" }' \
                     /etc/sysconfig/${NAME}
                 )"
    fi
    TOMCAT_SCRIPT="$options ${TOMCAT_SCRIPT}"
}

# See how we were called.
function start() {

   echo -n "Starting ${TOMCAT_PROG}: "
   if [ "$RETVAL" != "0" ]; then
     failure
     return
   fi
   if [ -f "/var/lock/subsys/${NAME}" ]; then
        if [ -s "/var/run/${NAME}.pid" ]; then
            read kpid < /var/run/${NAME}.pid
#           if checkpid $kpid 2>&1; then
            if [ -d "/proc/${kpid}" ]; then
                success
                return 0
            fi
        fi
    fi
    # fix permissions on the log and pid files
    export CATALINA_PID="/var/run/${NAME}.pid"
    touch $CATALINA_PID 2>&1 || RETVAL="4"
    if [ "$RETVAL" -eq "0" -a "$?" -eq "0" ]; then
      chown ${TOMCAT_USER}:${TOMCAT_USER} $CATALINA_PID
    fi
    [ "$RETVAL" -eq "0" ] && touch $TOMCAT_LOG 2>&1 || RETVAL="4"
    if [ "$RETVAL" -eq "0" -a "$?" -eq "0" ]; then
      chown ${TOMCAT_USER}:${TOMCAT_USER} $TOMCAT_LOG
    fi
    if [ "$CATALINA_HOME" != "/usr/share/tomcat7" -a "$RETVAL" -eq "0" ]; then
        # Create a tomcat directory if it doesn't exist
        makeHomeDir
        # If CATALINA_HOME doesn't exist modify port number so that
        # multiple instances don't interfere with each other
        findFreePorts
        sed -i -e "s/8005/${randomPort1}/g" -e "s/8080/${CONNECTOR_PORT}/g" \
            -e "s/8009/${randomPort2}/g" -e "s/8443/${randomPort3}/g" \
            ${CATALINA_HOME}/conf/server.xml
    fi
    parseOptions
    if [ "$RETVAL" -eq "0" -a "$SECURITY_MANAGER" = "true" ]; then
        $SU - $TOMCAT_USER -c "${TOMCAT_SCRIPT} start-security" \
            >> ${TOMCAT_LOG} 2>&1 || RETVAL="4"
    else

       [ "$RETVAL" -eq "0" ] && $SU - $TOMCAT_USER -c "${TOMCAT_SCRIPT} start" >> ${TOMCAT_LOG} 2>&1 || RETVAL="4"
    fi
    if [ "$RETVAL" -eq "0" ]; then
        success
        touch /var/lock/subsys/${NAME}
    else
        echo -n "Error code ${RETVAL}"
        failure
    fi
}

function stop() {
    #check to see if pid file is good. We only want to stop tomcat7 if
    #we started it from this init script
    running_pid=$(pgrep -f catalina)
    if [ -f /var/run/${NAME}.pid ]; then
        read kpid junk< /var/run/${NAME}.pid
        if [ -z "$kpid" ]; then
            echo -n "PID file empty"
            rm -f /var/lock/subsys/${NAME} /var/run/${NAME}.pid
            failure
            exit 4
        fi
        if [ -z "$running_pid" ]; then
            echo -n "no ${NAME} running, but pid file exists - cleaning up"
            rm -f /var/lock/subsys/${NAME} /var/run/${NAME}.pid
            success
            exit 0
        fi
        if [ -z "$(echo ${kpid} | fgrep -x "${running_pid}")" ]; then
            echo -n "PID file does not match pid of any running ${NAME}"
            failure
            rm -f /var/lock/subsys/${NAME} /var/run/${NAME}.pid
            exit 4
        fi
        parseOptions
        #stop tomcat
        echo -n "Stopping ${TOMCAT_PROG}: "
        $SU - $TOMCAT_USER -c "${TOMCAT_SCRIPT} stop" >> ${TOMCAT_LOG} 2>&1 || RETVAL="4"
        if [ "$RETVAL" -eq "4" ]; then
            sleep 1
            if [ "$SHUTDOWN_VERBOSE" = "true" ]; then
                echo "Failed to stop ${NAME} normally, sending a graceful kill."
            fi
            kill $kpid > /dev/null 2>&1
            sleep 1
        fi
        #wait for tomcat to really shutdown
        count=0
        until [ "$(ps --pid $kpid | grep -c $kpid)" -eq "0" ] || \
          [ "$count" -gt "$SHUTDOWN_WAIT" ]; do
            if [ "$SHUTDOWN_VERBOSE" = "true" ]; then
                echo "waiting for processes ${NAME} ($kpid) to exit"
            fi
            sleep 1
            let count="${count}+1"
        done
        if [ "$count" -gt "$SHUTDOWN_WAIT" ]; then
            if [ "$SHUTDOWN_VERBOSE" = "true" ]; then
                echo -n "Failed to stop ${NAME} ($kpid) gracefully after $SHUTDOWN_WAIT seconds, sending SIGKILL."
            fi
            warning
            kill -9 $kpid
            if [ "$SHUTDOWN_VERBOSE" = "true" ]; then
                echo "Waiting for ${NAME} ($kpid) to exit."
            fi
            count=0
            until [ "$(ps --pid $kpid | grep -c $kpid)" -eq "0" ] || \
              [ "$count" -gt "$KILL_SLEEP_WAIT" ]; do
                if [ "$SHUTDOWN_VERBOSE" = "true" ]; then
                    echo "waiting for ${NAME} ($kpid) to exit. It could be in the UNINTERRUPTIBLE state"
                fi
                sleep 1
                let count="${count}+1"
            done
        fi
        #check to make sure tomcat is gone
        if [ "$(ps --pid $kpid | grep -c $kpid)" -eq "0" ]; then
            rm -f /var/lock/subsys/${NAME} /var/run/${NAME}.pid
            RETVAL="0"
            success
        else
            echo -n "Unable to stop ${NAME} ($kpid)"
            RETVAL="4"
            failure
        fi
    else
        if [ -n "$running_pid" ]; then
            echo -n "${NAME} running, but no pid file"
            failure
            RETVAL="4"
        else
            success
        fi
    fi
    return $RETVAL
}

function usage()
{
   echo "Usage: $0 {start|stop|restart|condrestart|try-restart|reload|force-reload|status|version}"
   RETVAL="2"
}

function rh_status()
{
    status -p /var/run/${NAME}.pid ${NAME}
}

function rh_status_q()
{
    rh_status >/dev/null 2>&1
}

# See how we were called.
RETVAL="0"
case "$1" in
    start)
        rh_status_q && exit 0
        start
        ;;
    stop)
        stop
        ;;
    restart)
        stop
        start
        ;;
    condrestart|try-restart)
        if [ -s "/var/run/${NAME}.pid" ]; then
            stop
            start
        fi
        ;;
    reload)
        RETVAL="3"
        ;;
    force-reload)
        if [ -s "/var/run/${NAME}.pid" ]; then
            stop
            start
        fi
        ;;
    status)
        if [ -s "/var/run/${NAME}.pid" ]; then
            read kpid junk < /var/run/${NAME}.pid
            if [ -d "/proc/${kpid}" ]; then
                echo -n "${NAME} (pid ${kpid}) is running..."
                success
                RETVAL="0"
            else
# The pid file exists but the process is not running
                echo -n "PID file exists, but process is not running"
                warning
                RETVAL="1"
            fi
        else
            pid="$(/usr/bin/pgrep -d , -u ${TOMCAT_USER} -G ${TOMCAT_USER} java)"
            if [ -z "$pid" ]; then
                echo "${NAME} is stopped"
                success
                RETVAL="3"
            else
                echo "${NAME} (pid $pid) is running, but PID file is missing"
                success
                RETVAL="0"
            fi
        fi
        ;;
    version)
        ${TOMCAT_SCRIPT} version
        ;;
    *)
      usage
      ;;
esac

exit $RETVAL
```
##### Read the Tomcat launch script
    less /usr/sbin/tomcat7
```bash
#!/bin/bash

if [ -r /usr/share/java-utils/java-functions ]; then
  . /usr/share/java-utils/java-functions
else
  echo "Can't read Java functions library, aborting"
  exit 1
fi

# Get the tomcat config (use this for environment specific settings)
if [ -z "${TOMCAT_CFG}" ]; then
  TOMCAT_CFG="/etc/tomcat7/tomcat7.conf"
fi

if [ -r "$TOMCAT_CFG" ]; then
  . $TOMCAT_CFG
fi

# Only source the sysconfig file if TOMCAT_NAME or NAME is defined
# This prevents issues when defining NAME in the config file while
# still allowing the NAME environment variable to be set when executing
# this script
# By default the init script exports TOMCAT_NAME
if [ -n "${TOMCAT_NAME}" -a -r "/etc/sysconfig/${TOMCAT_NAME}" ]; then
    . /etc/sysconfig/${TOMCAT_NAME}
elif [ -n "${NAME}" -a -r "/etc/sysconfig/${NAME}" ]; then
    . /etc/sysconfig/${NAME}
fi

set_javacmd
# CLASSPATH munging
if [ -n "$JSSE_HOME" ]; then
  CLASSPATH="${CLASSPATH}:$(build-classpath jcert jnet jsse 2>/dev/null)"
fi
CLASSPATH="${CLASSPATH}:${CATALINA_HOME}/bin/bootstrap.jar"
CLASSPATH="${CLASSPATH}:${CATALINA_HOME}/bin/tomcat-juli.jar"
CLASSPATH="${CLASSPATH}:$(build-classpath commons-daemon 2>/dev/null)"

if [ "$1" = "start" ]; then
  ${JAVACMD} $JAVA_OPTS $CATALINA_OPTS \
    -classpath "$CLASSPATH" \
    -Dcatalina.base="$CATALINA_BASE" \
    -Dcatalina.home="$CATALINA_HOME" \
    -Djava.endorsed.dirs="$JAVA_ENDORSED_DIRS" \
    -Djava.io.tmpdir="$CATALINA_TMPDIR" \
    -Djava.util.logging.config.file="${CATALINA_BASE}/conf/logging.properties" \
    -Djava.util.logging.manager="org.apache.juli.ClassLoaderLogManager" \
    org.apache.catalina.startup.Bootstrap start \
    >> ${CATALINA_BASE}/logs/catalina.out 2>&1 &
    if [ ! -z "$CATALINA_PID" ]; then
      echo $! > $CATALINA_PID
    fi
elif [ "$1" = "start-security" ]; then
  ${JAVACMD} $JAVA_OPTS $CATALINA_OPTS \
    -classpath "$CLASSPATH" \
    -Dcatalina.base="$CATALINA_BASE" \
    -Dcatalina.home="$CATALINA_HOME" \
    -Djava.endorsed.dirs="$JAVA_ENDORSED_DIRS" \
    -Djava.io.tmpdir="$CATALINA_TMPDIR" \
    -Djava.security.manager \
    -Djava.security.policy=="${CATALINA_BASE}/conf/catalina.policy" \
    -Djava.util.logging.config.file="${CATALINA_BASE}/conf/logging.properties" \
    -Djava.util.logging.manager="org.apache.juli.ClassLoaderLogManager" \
    org.apache.catalina.startup.Bootstrap start \
    >> ${CATALINA_BASE}/logs/catalina.out 2>&1 &
    if [ ! -z "$CATALINA_PID" ]; then
      echo $! > $CATALINA_PID
    fi
elif [ "$1" = "stop" ]; then
  ${JAVACMD} $JAVA_OPTS \
    -classpath "$CLASSPATH" \
    -Dcatalina.base="$CATALINA_BASE" \
    -Dcatalina.home="$CATALINA_HOME" \
    -Djava.endorsed.dirs="$JAVA_ENDORSED_DIRS" \
    -Djava.io.tmpdir="$CATALINA_TMPDIR" \
    org.apache.catalina.startup.Bootstrap stop \
    >> ${CATALINA_BASE}/logs/catalina.out 2>&1
elif [ "$1" = "version" ]; then
  ${JAVACMD} -classpath ${CATALINA_HOME}/lib/catalina.jar \
    org.apache.catalina.util.ServerInfo
else
  echo "Usage: $0 {start|start-security|stop|version}"
  exit 1
fi
```

##### Read the server.xml file
    less /usr/share/tomcat7/conf/server.xml
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
```

```
##### grep the contents of the passwd file to see that a user named tomcat has been created
    cat /etc/passwd |grep tomcat
    
##### Add users to the Tomcat 7 users file
    sudo vim /usr/share/tomcat7/conf/tomcat-users.xml
```xml
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>

  <role rolename="manager-script" />
  <role rolename="admin-script" />
  <user username="abc" password="123" roles="manager-script,admin-script" />

</tomcat-users>
```
##### Restart the Tomcat service to pick up the changes to tomcat-users.xml
	sudo service tomcat7 restart
	
##### Check environment variable for home directory
	echo $HOME
    
##### Create a Build Properties file in your home directory
	cd ~
	pwd
	vim build.properties

##### Add Catalina home, app name, manager username and password to the build.properties file
*These values are used by the build.xml file*  
```
catalina.home=/usr/share/tomcat7
app.name=hello
manager.username=abc
manager.password=123
```

##### Install git
    sudo yum install git
	
##### Create project folder
    mkdir -p ~/git/tc-aws

##### Initialize project repository
    cd git/tc-aws
    git init

##### Create remote repository
    http://github.com/

##### Configure repository
    git config user.name {username}
    git config user.email {emailaddress@example.com}

##### Add remote origin to local repository
    git remote add origin http://github.com/{username}/{projectname}.git

##### Pull from remote
    git pull origin master

##### Check status
    git status
    
##### Push to remote
    git push --set-upstream origin master

##### Create Build XML File
    cd ~/git/tc-aws
    wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/build.xml.txt
    cp build.xml.txt build.xml
    nano build.xml
*Give the project a name in the project declaration*  
*Set the app.name in build.properties*  
*Set catalina.home in build.properties*  
*Add includeantruntime="false" to the javac declaration*  
```xml
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

<!--
     General purpose build script for web applications and web services,
     including enhanced support for deploying directly to a Tomcat
     based server.

     This build script assumes that the source code of your web application
     is organized into the following subdirectories underneath the source
     code directory from which you execute the build script:

        docs                 Static documentation files to be copied to
                             the "docs" subdirectory of your distribution.

        src                  Java source code (and associated resource files)
                             to be compiled to the "WEB-INF/classes"
                             subdirectory of your web application.

        web                  Static HTML, JSP, and other content (such as
                             image files), including the WEB-INF subdirectory
                             and its configuration file contents.
-->


<!-- A "project" describes a set of targets that may be requested
     when Ant is executed.  The "default" attribute defines the
     target which is executed if no specific target is requested,
     and the "basedir" attribute defines the current working directory
     from which Ant executes the requested task.  This is normally
     set to the current working directory.
-->

<project name="MY PROJECT NAME" default="compile" basedir="."><!-- UPDATE THIS! -->



<!-- ===================== Property Definitions =========================== -->


<!--

  Each of the following properties are used in the build script.
  Values for these properties are set by the first place they are
  defined, from the following list:

  * Definitions on the "ant" command line (ant -Dfoo=bar compile).

  * Definitions from a "build.properties" file in the top level
    source directory of this application.

  * Definitions from a "build.properties" file in the developer's
    home directory.

  * Default definitions in this build.xml file.

  You will note below that property values can be composed based on the
  contents of previously defined properties.  This is a powerful technique
  that helps you minimize the number of changes required when your development
  environment is modified.  Note that property composition is allowed within
  "build.properties" files as well as in the "build.xml" script.

-->

  <property file="build.properties"/>
  <property file="${user.home}/build.properties"/>


<!-- ==================== File and Directory Names ======================== -->


<!--

  These properties generally define file and directory names (or paths) that
  affect where the build process stores its outputs.

  app.name             Base name of this application, used to
                       construct filenames and directories.
                       Defaults to "myapp".

  app.path             Context path to which this application should be
                       deployed (defaults to "/" plus the value of the
                       "app.name" property).

  app.version          Version number of this iteration of the application.

  build.home           The directory into which the "prepare" and
                       "compile" targets will generate their output.
                       Defaults to "build".

  catalina.home        The directory in which you have installed
                       a binary distribution of Tomcat.  This will
                       be used by the "deploy" target.

  dist.home            The name of the base directory in which
                       distribution files are created.
                       Defaults to "dist".

  manager.password     The login password of a user that is assigned the
                       "manager-script" role (so that he or she can execute
                       commands via the "/manager" web application)

  manager.url          The URL of the "/manager" web application on the
                       Tomcat installation to which we will deploy web
                       applications and web services.

  manager.username     The login username of a user that is assigned the
                       "manager-script" role (so that he or she can execute
                       commands via the "/manager" web application)

-->

  <!-- <property name="catalina.home" value="../../../.."/>--> <!-- COMMENT OUT -->
  <!--<property name="app.name"      value="myapp"/>--><!-- SET IN BUILD PROPERTIES FILE -->
  
  <property name="app.path"      value="/${app.name}"/>
  <property name="app.version"   value="0.1-dev"/>
  
  <property name="build.home"    value="${basedir}/build"/>
  <property name="dist.home"     value="${basedir}/dist"/>
  <property name="docs.home"     value="${basedir}/docs"/>
  <property name="src.home"      value="${basedir}/src"/>
  <property name="web.home"      value="${basedir}/web"/>
  
  <property name="manager.url"   value="http://localhost:8080/manager/text"/>

<!-- ==================== External Dependencies =========================== -->


<!--

  Use property values to define the locations of external JAR files on which
  your application will depend.  In general, these values will be used for
  two purposes:
  * Inclusion on the classpath that is passed to the Javac compiler
  * Being copied into the "/WEB-INF/lib" directory during execution
    of the "deploy" target.

  Because we will automatically include all of the Java classes that Tomcat
  exposes to web applications, we will not need to explicitly list any of those
  dependencies.  You only need to worry about external dependencies for JAR
  files that you are going to include inside your "/WEB-INF/lib" directory.

-->

<!-- Dummy external dependency -->
<!--
  <property name="foo.jar"
           value="/path/to/foo.jar"/>
-->


<!-- ==================== Compilation Classpath =========================== -->

<!--

  Rather than relying on the CLASSPATH environment variable, Ant includes
  features that makes it easy to dynamically construct the classpath you
  need for each compilation.  The example below constructs the compile
  classpath to include the servlet.jar file, as well as the other components
  that Tomcat makes available to web applications automatically, plus anything
  that you explicitly added.

-->

  <path id="compile.classpath">

    <!-- Include all JAR files that will be included in /WEB-INF/lib -->
    <!-- *** CUSTOMIZE HERE AS REQUIRED BY YOUR APPLICATION *** -->
<!--
    <pathelement location="${foo.jar}"/>
-->

    <!-- Include all elements that Tomcat exposes to applications -->
    <fileset dir="${catalina.home}/bin">
      <include name="*.jar"/>
    </fileset>
    <pathelement location="${catalina.home}/lib"/>
    <fileset dir="${catalina.home}/lib">
      <include name="*.jar"/>
    </fileset>

  </path>



<!-- ================== Custom Ant Task Definitions ======================= -->


<!--

  These properties define custom tasks for the Ant build tool that interact
  with the "/manager" web application installed with Tomcat.  Before they
  can be successfully utilized, you must perform the following steps:

  - Copy the file "lib/catalina-ant.jar" from your Tomcat
    installation into the "lib" directory of your Ant installation.

  - Create a "build.properties" file in your application's top-level
    source directory (or your user login home directory) that defines
    appropriate values for the "manager.password", "manager.url", and
    "manager.username" properties described above.

  For more information about the Manager web application, and the functionality
  of these tasks, see <http://localhost:8080/tomcat-docs/manager-howto.html>.

-->

  <taskdef resource="org/apache/catalina/ant/catalina.tasks"
           classpathref="compile.classpath"/>


<!--  ==================== Compilation Control Options ==================== -->

<!--

  These properties control option settings on the Javac compiler when it
  is invoked using the <javac> task.

  compile.debug        Should compilation include the debug option?

  compile.deprecation  Should compilation include the deprecation option?

  compile.optimize     Should compilation include the optimize option?

-->

  <property name="compile.debug"       value="true"/>
  <property name="compile.deprecation" value="false"/>
  <property name="compile.optimize"    value="true"/>



<!-- ==================== All Target ====================================== -->

<!--

  The "all" target is a shortcut for running the "clean" target followed
  by the "compile" target, to force a complete recompile.

-->

  <target name="all" depends="clean,compile"
   description="Clean build and dist directories, then compile"/>



<!-- ==================== Clean Target ==================================== -->

<!--

  The "clean" target deletes any previous "build" and "dist" directory,
  so that you can be ensured the application can be built from scratch.

-->

  <target name="clean"
   description="Delete old build and dist directories">
    <delete dir="${build.home}"/>
    <delete dir="${dist.home}"/>
  </target>



<!-- ==================== Compile Target ================================== -->

<!--

  The "compile" target transforms source files (from your "src" directory)
  into object files in the appropriate location in the build directory.
  This example assumes that you will be including your classes in an
  unpacked directory hierarchy under "/WEB-INF/classes".

-->

  <target name="compile" depends="prepare"
   description="Compile Java sources">

    <!-- Compile Java classes as necessary -->
    <mkdir dir="${build.home}/WEB-INF/classes"/>
    
    <javac srcdir="${src.home}"
    	destdir="${build.home}/WEB-INF/classes"
    	debug="${compile.debug}"
    	deprecation="${compile.deprecation}"
      	optimize="${compile.optimize}"
      	includeantruntime="false"
        ><!-- ADD includeantruntime="false" -->
        <classpath refid="compile.classpath"/>
    </javac>

    <!-- Copy application resources -->
    <copy  todir="${build.home}/WEB-INF/classes">
      <fileset dir="${src.home}" excludes="**/*.java"/>
    </copy>

  </target>



<!-- ==================== Dist Target ===================================== -->


<!--

  The "dist" target creates a binary distribution of your application
  in a directory structure ready to be archived in a tar.gz or zip file.
  Note that this target depends on two others:

  * "compile" so that the entire web application (including external
    dependencies) will have been assembled

  * "javadoc" so that the application Javadocs will have been created

-->

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

<!--

  The "install" target tells the specified Tomcat installation to dynamically
  install this web application and make it available for execution.  It does
  *not* cause the existence of this web application to be remembered across
  Tomcat restarts; if you restart the server, you will need to re-install all
  this web application.

  If you have already installed this application, and simply want Tomcat to
  recognize that you have updated Java classes (or the web.xml file), use the
  "reload" target instead.

  NOTE:  This target will only succeed if it is run from the same server that
  Tomcat is running on.

  NOTE:  This is the logical opposite of the "remove" target.

-->

  <target name="install" depends="compile"
   description="Install application to servlet container">

    <deploy url="${manager.url}"
       username="${manager.username}"
       password="${manager.password}"
           path="${app.path}"
       localWar="file://${build.home}"/>

  </target>


<!-- ==================== Javadoc Target ================================== -->

<!--

  The "javadoc" target creates Javadoc API documentation for the Java
  classes included in your application.  Normally, this is only required
  when preparing a distribution release, but is available as a separate
  target in case the developer wants to create Javadocs independently.

-->

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

<!--

  The "list" target asks the specified Tomcat installation to list the
  currently running web applications, either loaded at startup time or
  installed dynamically.  It is useful to determine whether or not the
  application you are currently developing has been installed.

-->

  <target name="list"
   description="List installed applications on servlet container">

    <list    url="${manager.url}"
        username="${manager.username}"
        password="${manager.password}"/>

  </target>


<!-- ==================== Prepare Target ================================== -->

<!--

  The "prepare" target is used to create the "build" destination directory,
  and copy the static contents of your web application to it.  If you need
  to copy static files from external dependencies, you can customize the
  contents of this task.

  Normally, this task is executed indirectly when needed.

-->

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
<!--
    <copy todir="${build.home}/WEB-INF/lib" file="${foo.jar}"/>
-->

    <!-- Copy static files from external dependencies as needed -->
    <!-- *** CUSTOMIZE HERE AS REQUIRED BY YOUR APPLICATION *** -->

  </target>


<!-- ==================== Reload Target =================================== -->

<!--

  The "reload" signals the specified application Tomcat to shut itself down
  and reload. This can be useful when the web application context is not
  reloadable and you have updated classes or property files in the
  /WEB-INF/classes directory or when you have added or updated jar files in the
  /WEB-INF/lib directory.

  NOTE: The /WEB-INF/web.xml web application configuration file is not reread
  on a reload. If you have made changes to your web.xml file you must stop
  then start the web application.

-->

  <target name="reload" depends="compile"
   description="Reload application on servlet container">

    <reload url="${manager.url}"
       username="${manager.username}"
       password="${manager.password}"
           path="${app.path}"/>

  </target>


<!-- ==================== Remove Target =================================== -->

<!--

  The "remove" target tells the specified Tomcat installation to dynamically
  remove this web application from service.

  NOTE:  This is the logical opposite of the "install" target.

-->

  <target name="remove"
   description="Remove application on servlet container">

    <undeploy url="${manager.url}"
         username="${manager.username}"
         password="${manager.password}"
             path="${app.path}"/>

  </target>

</project>
```

##### Install ant
    sudo yum install ant

##### List installed projects
    ant -v list
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `list' is [list]
Complete build sequence is [list, prepare, compile, reload, install, remove, javadoc, clean, dist, all, ]

list:
     [list] OK - Listed applications for virtual host localhost
     [list] /manager:running:1:manager
     [list] /:running:0:ROOT
     [list] /sample:running:0:sample
     [list] /examples:running:0:examples
     [list] /host-manager:running:0:host-manager

BUILD SUCCESSFUL
Total time: 0 seconds

```
    
##### Make a web directory in the project folder
	mkdir web

##### Prepare the project
	ant -v prepare
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `prepare' is [prepare]
Complete build sequence is [prepare, compile, reload, install, remove, list, javadoc, clean, dist, all, ]

prepare:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
     [copy] No sources found.
     [copy]  added as  is outdated.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/lib because it already exists.

BUILD SUCCESSFUL
Total time: 0 seconds
```

##### Make a src directory in the project folder
	mkdir src
	
##### Compile the project
    ant -v compile
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `compile' is [prepare, compile]
Complete build sequence is [prepare, compile, reload, install, remove, list, javadoc, clean, dist, all, ]

prepare:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
     [copy] No sources found.
     [copy]  added as  is outdated.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/lib because it already exists.

compile:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
    [javac] No sources found.
     [copy] No sources found.
     [copy]  added as  is outdated.

BUILD SUCCESSFUL
Total time: 0 seconds
```

##### Recursively change the mode of all the files in the project directory
```
chmod -R 775 ~/git/tc-aws
```
##### Install the project
    ant -v install
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `install' is [prepare, compile, install]
Complete build sequence is [prepare, compile, install, reload, remove, list, javadoc, clean, dist, all, ]

prepare:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
     [copy] No sources found.
     [copy]  added as  is outdated.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/lib because it already exists.

compile:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
    [javac] No sources found.
     [copy] No sources found.
     [copy]  added as  is outdated.

install:
   [deploy] OK - Deployed application at context path /hello

BUILD SUCCESSFUL
Total time: 1 second
```

##### Reload the project
    ant -v reload
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `reload' is [prepare, compile, reload]
Complete build sequence is [prepare, compile, reload, install, remove, list, javadoc, clean, dist, all, ]

prepare:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF because it already exists.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
     [copy] No sources found.
     [copy]  added as  is outdated.
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/lib because it already exists.

compile:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
    [javac] No sources found.
     [copy] No sources found.
     [copy]  added as  is outdated.

reload:
   [reload] OK - Reloaded application at context path /hello

BUILD SUCCESSFUL
Total time: 0 seconds
```

##### Remove the project
    ant -v remove
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `remove' is [remove]
Complete build sequence is [remove, prepare, compile, reload, install, list, javadoc, clean, dist, all, ]

remove:
 [undeploy] OK - Undeployed application at context path /hello

BUILD SUCCESSFUL
Total time: 1 second
```

##### Clean the project build directory
    ant -v clean
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `clean' is [clean]
Complete build sequence is [clean, prepare, compile, reload, install, remove, list, javadoc, dist, all, ]

clean:
   [delete] Deleting directory /home/bachmeb/git/tc-aws/build
   [delete] Deleting directory /home/bachmeb/git/tc-aws/build/WEB-INF/classes
   [delete] Deleting directory /home/bachmeb/git/tc-aws/build/WEB-INF/lib
   [delete] Deleting directory /home/bachmeb/git/tc-aws/build/WEB-INF
   [delete] Deleting directory /home/bachmeb/git/tc-aws/build

BUILD SUCCESSFUL
Total time: 0 seconds
```

##### Clean, prepare, and compile the project
    ant -v all
```
Apache Ant(TM) version 1.8.3 compiled on February 25 2015
Trying the default build file: build.xml
Buildfile: /home/bachmeb/git/tc-aws/build.xml
Detected Java version: 1.7 in: /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.91.x86_64/jre
Detected OS: Linux
parsing buildfile /home/bachmeb/git/tc-aws/build.xml with URI = file:/home/bachmeb/git/tc-aws/build.xml
Project base dir set to: /home/bachmeb/git/tc-aws
parsing buildfile jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml with URI = jar:file:/usr/share/java/ant.jar!/org/apache/tools/ant/antlib.xml from a zip file
 [property] Loading /home/bachmeb/git/tc-aws/build.properties
 [property] Unable to find property file: /home/bachmeb/git/tc-aws/build.properties
 [property] Loading /home/bachmeb/build.properties
Trying to override old definition of datatype resources
Build sequence for target(s) `all' is [clean, prepare, compile, all]
Complete build sequence is [clean, prepare, compile, all, reload, install, remove, list, javadoc, dist, ]

clean:

prepare:
    [mkdir] Created dir: /home/bachmeb/git/tc-aws/build
    [mkdir] Created dir: /home/bachmeb/git/tc-aws/build/WEB-INF
    [mkdir] Created dir: /home/bachmeb/git/tc-aws/build/WEB-INF/classes
     [copy] No sources found.
     [copy]  omitted as /home/bachmeb/git/tc-aws/build is up to date.
    [mkdir] Created dir: /home/bachmeb/git/tc-aws/build/WEB-INF/lib

compile:
    [mkdir] Skipping /home/bachmeb/git/tc-aws/build/WEB-INF/classes because it already exists.
    [javac] No sources found.
     [copy] No sources found.
     [copy]  omitted as /home/bachmeb/git/tc-aws/build/WEB-INF/classes is up to date.

all:

BUILD SUCCESSFUL
Total time: 0 seconds
```

##### Create an index file
    cd web
    vim index.html
```xml
Hello <a href="./there">There</a>.
```
##### Make a Java class
    mkdir -p ~/git/tc-aws/src/com/example/greeting
    vim ~/git/tc-aws/src/com/example/greeting/HelloServlet.java
```java
package com.example.packname;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Simple servlet displays a message
 */
public class HelloServlet extends HttpServlet {

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse
            response) throws IOException {
        PrintWriter out = response.getWriter();
        out.println("HELLO!!!");
        out.close();
    }
}
```

##### Create a web.xml file
    mkdir -p ~/git/tc-aws/web/WEB-INF
    vim ~/git/tc-aws/web/WEB-INF/web.xml
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE web-app
    PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd">
<web-app>
    <display-name>Hi There</display-name>
    <servlet>
        <servlet-name>sayhi</servlet-name>
        <servlet-class>com.example.packname.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>sayhi</servlet-name>
        <url-pattern>/there</url-pattern>
    </servlet-mapping>
</web-app>
```
##### Reinstall the project
    cd ~/git/tc-aws
    ls
    ant all
    ant install

##### View the web app in Lynx
    lynx http://localhost:8080/hello
    
##### View the app in a web browser on your PC
    http://[ec2.ipa.ddr.ess]:8080/hello
