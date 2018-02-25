# tomcat-spring-ec2

```
sudo yum update 

wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.rpm" -O jdk-8u161-linux-x64.rpm

```
Then install the rpm easily with the command:

```
sudo rpm -ivh jdk-8u161-linux-x64.rpm
```

```
sudo nano /etc/profile

JAVA_HOME=/usr/java/jdk1.8.0_161/
export  JAVA_HOME
PATH=$JAVA_HOME/bin:$PATH
export PATH

```

```
wget http://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.tar.gz

tar zxpvf apache-tomcat-7.0.85.tar.gz

sudo mv apache-tomcat-7.0.85 /usr/share/

```
To configure Tomcat to launch automatically create a file called tomcat in the directory /etc/rc.d/init.d/ with the following contents:

```
#!/bin/sh
# Tomcat init script for Linux.
#
# chkconfig: 2345 96 14
# description: The Apache Tomcat servlet/JSP container.
JAVA_HOME=/usr/java/jdk1.8.0_161/
CATALINA_HOME=/usr/share/apache-tomcat-7.0.85
export JAVA_HOME CATALINA_HOME
exec $CATALINA_HOME/bin/catalina.sh $*
```
Next, execute the following commands to set the proper permissions for your init script and enable Tomcat for auto-launch:
```
chmod 755 /etc/rc.d/init.d/tomcat
chkconfig --level 2345 tomcat on
```
Tomcat should now be automatically launched whenever your server restarts.

Now we need to set up the Tomcat users. This will allow access to the Manger Console in the Tomcat interface. The users are configured in a file called tomcat-users.xml which is stored in the apache-tomcat-7.0.85/conf directory. Open this file using nano and edit the user permissions as below, changing the password as appropriate:
```
sudo nano /usr/share/apache-tomcat-7.0.85/conf/tomcat-users.xml

<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<role rolename="admin-gui"/>
<user username="tomcat" password="pass12345"
  roles="manager-gui,manager-status,admin-gui"/>
<user username="tomcattools" password="pass12345"/>

```
We have now configure all that needs to be configured. Go back to the EC2 console and reboot the instance by right clicking on the instance and selecting reboot. This should not take more that a few minutes.
