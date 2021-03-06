JMS Remote Messaging Usecases on JBoss AS
=========================================

This demo shows the various ways of sending JMS messages to remote (JVM) HornetQ system running on JBoss AS a.k.a Wildfly

Prerequisites for the Demo
--------------------------
- Ensure that [Oracle JDK] (http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) is installed
- Set the JAVA_HOME environment variable pointing to the JDK installation
- Update the PATH environment variable to include the bin directory of the JDK installation
- If in a new command prompt window you are able to run the command: *javac -version* you are all set
- For building the packaged source code, download and install [Maven](http://maven.apache.org/download.cgi)
- Ensure that the PATH environment variable to include the bin directory of the Maven installation

Highlights of the Demo
----------------------

The install process will:
- Setup two standlaone instances of Wildfly server
- Start the servers : Server 1 and Server 2, with Server 2 started with a port offset of 100
- On Server 1, thru CLI scripts
 * Configure JMS Queues: JMSBridgeSourceQ, LocalServer1Q
 * JMS Bridge: simple-jms-brdige 
 * A pooled-connection-factory using outbound JCA adapter 
- On Server 2, thru CLI scripts
 * Configure JMS Queues: JMSBridgeTargetQ, LocalServer2Q

In this demo you will see how to send messages to a:
- Remote queue running inside JBoss AS using a simple local Java client
- Remote queue running inside JBoss AS from a process running on local JBoss AS
- Local queue which will then forward the message to a remote using a JMS bridge

Pre-Installation Setup
----------------------
 1. [Download RHQ Bundle Deployer 4.8.0] (http://search.maven.org/remotecontent?filepath=org/rhq/rhq-ant-bundle-common/4.8.0/rhq-ant-bundle-common-4.8.0.zip)
 2. Extract the contents of rhq-ant-bundle-common-4.8.0.zip to the root folder of the demo

Command-Line Installation
-------------------------
- Open up Linux terminal or Windows command prompt
- Navigate to the demo root folder. On Unix/Linux, run _chmod -R 755 *_ at the command prompt when in the demo root folder
- Configure runtime properties in the file deployment.properties, including path to the deployment folder
- Execute either install.sh or install.bat based on the OS
- Based on the bind address configured in the deployment properties verify the startup of the server(s)

Building the Source Code with Maven
-----------------------------------
- Navigate to the folder _jms-remote-messaging-usecases_ in the demo root folder
- Find the files __RemoteQueueInteractor.java__, __RemoteMDB.java__ and correct the IP address to the IP of your physical server
- Come back to folder _jms-remote-messaging-usecases_ at command propmt and run _mvn clean package_
- If everything goes well and the build is a SUCCESS, the included test case for simple-remote-messaging-client has already sent out a message to __LocalServer1Q__ on __Server 1__
- Now deploy the WAR file _simple-remote-messaging-webapp\target\simple-remote-messaging-webapp.war_ onto __Server 1__
- Assuming a successful WAR deployment and assuming the IP address of the server is 192.168.1.21 for example
 * [Test the JMS Bridge functionality here] (http://192.168.1.21:8080/simple-remote-messaging-webapp/JMSBridgeClient?message=Hello)
 * [Test the pooled-connection-factory functionality here] (http://192.168.1.21:8080/simple-remote-messaging-webapp/RemoteMessageSender?message=Hello)
