


Remote Debugging Java Web Applications on Microsoft Azure Websites
==================================================================

Microsoft Azure websites supports running Java Web Applications. This blog describes the support for remote debugging your Java Web Applications on Microsoft Azure Websites.

**Prerequisites**
* Azure website with a Java Web Application with a web.config that enables JVM debug server.
* Enable websockets on your azure web site.
* JDWP compatible debugger like Eclipse/Netbeans.
* DebugSession Client application (your debugger will connect to this client application).


Enable ‘web sockets’ on your azure website
------------------------------------------


![alt IMAGEONE](https://github.com/Azure/azure-websites-java-remote-debugging/blob/master/images/enable_websockets.png)


Enable debugging in web.config
------------------------------

If you already have web.config in your azure website site\wwwroot, add the following line to the JAVA_OPTS environment variable.
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=127.0.0.1:%HTTP_PLATFORM_DEBUG_PORT%
As shown below:

```
<httpPlatform … >
	<environmentVariables>
	<environmentVariable name=”JAVA_OPTS” value=”-Djava.net.preferIPv4Stack=true -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=127.0.0.1:%HTTP_PLATFORM_DEBUG_PORT%”>
	<environmentVariables>
</httpPlatform>
```

NOTE: %HTTP_PLATFORM_DEBUG_PORT% is a special placeholder which will be replaced with a port internally generated by the HttpPlatformHandler module.

Restart your azure website after editing the web.config.

If you don’t have a web.config, you will need to create one. Please see references section for links to sample web.config.

DebugSession client application
-------------------------------
Download the DebugSession client application from https://github.com/Azure/azure-websites-java-remote-debugging/releases

On your local machine, run DebugSession.bat (DebugSession.sh on linux) as shown below.
```
C:\JDebug\bin>DebugSession.bat
usage: org.azure.waws.DebugSessionMain
 -a,--affinity <arg>   affinity cookie value (optional)
 -p,--port <arg>       local port (required)
 -s,--server <arg>     server (required)
 -t,--auto             restart client application automatically on disconnect/error.
 -u,--user <arg>       username (required iff password is specified)
 -w,--pass <arg>       password (optional)
```

Example:
*DebugSession.bat –p 8000 –s yoursite.scm.azurewebsites.net –u deploymentUserName –w deploymentPassword –t*


![alt IMAGETWO](https://github.com/Azure/azure-websites-java-remote-debugging/blob/master/images/DebugSession.png)


You could specify the Affinity cookie value to hit a specific instance of your site if your site is configured to run on multiple workers and you want to debug a specific instance.

Connect eclipse debugger to your DebugSession


![alt IMAGETHREE](https://github.com/Azure/azure-websites-java-remote-debugging/blob/master/images/eclipse_debugconfig.png)


Click “Remote Java Application”, enter the port on which you started DebugSession.bat and then click ‘Debug’.


![alt IMAGEFOUR](https://github.com/Azure/azure-websites-java-remote-debugging/blob/master/images/eclipse_remote_debug.png)




**References**
* [Getting Started with Java Web Applications on Azure Websites](http://azure.microsoft.com/en-us/documentation/articles/web-sites-java-get-started/)
* [Sample HelloWorld java web application with web.config](https://github.com/rramachand21/jaws/tree/master/HelloWorld)
* [Configuring custom web container on Azure websites](http://azure.microsoft.com/en-us/documentation/articles/web-sites-java-custom-upload/)

