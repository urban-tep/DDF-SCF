
Installing & Configuring the IT4I Processing Center
###################################################

Proxying and load balancing
===========================

Proxy for utep.it4i.cz with the load balancing functionality.

Dual installation of the Geoserver and PostgreSql instances on separate machines used for the load balancing and as a backup in case of failure.

Geoservers
==========

Standard installation and configuration of following software:

* Geoserver 2.10.2 http://geoserver.org/
* Geoserver WPS plugin
* Geoserver Monitoring plugin (Core, Hibernate)
* Geoserver Community module - WPS status in JDBC DB https://repo.boundlessgeo.com/main/org/geoserver/community/gs-wps-jdbc/2.10.2/
* Apache Tomcat 8.5 https://tomcat.apache.org/
* PostgreSql 9.5 https://www.postgresql.org/


Cluster Access
==============

*HPC as a Service Middleware*

Internal middleware API implementation to provide access to the Salomon cluster. This API is used in the WPS implementation to submit required computation to PBS scheduler of the cluster environment.

Internal Software
=================

UtepHandler
-----------

HTTP handler used as a proxy for the WPS services. It is used to parse the REMOTE_USER header from the HTTP request to authenticate the user in the processing center.

1. Configuration *UtepHandler.asxh*

	* haasUtil - connection string for the HPC as a Service Middleware database (@"Server=<serverName>;Database=<databaseName>;User Id=<login>;Password=<password>;")
	* handlerName - URI of the deployed handler used for geoserver requests ("utep.it4i.cz/UtepHandler/UtepHandler.ashx/geoserver")
	* geoserverName - URI of the geoserver instance ("<serverName>.it4i.cz:8081/geoserver")
	* proxyName - URI of the proxy used for geoserver requests ("utep.it4i.cz/geoserver")
  
2. Deployment
   
	Deploy the UtepHandler to a Web Server (IIS)
   
3. Proxy configuration

	Configure proxy in a way, that the requests for the Geoserver proxy "utep.it4i.cz/geoserver" are forwarded to the URI of the deployed UtepHandler.

The UtepHandler will log all incoming requests and forward the portal requests to the geoserver. 

UtepDevEnv
----------

WCF service used for the Development Environment use case. Web service contains two methods, one for user authentication (AuthenticateUser) and one for user processor package upload (BundleUpload).

1. *DevEnvService.svc.cs* configuration

	* haasUtil - connection string for the HPC as a Service Middleware database used for user authentication (@"Server=<serverName>;Database=<databaseName>;User Id=<login>;Password=<password>;")
  
2. *Web.config* configuration

	Change the URI of the deployment server.

	.. code-block:: xml
    
		<service name="UtepDevEnv.DevEnvService" behaviorConfiguration="myServiceBehave">
			<endpoint address="https://<serverName>/DevEnv/DevEnvService.svc" binding="webHttpBinding" bindingConfiguration="webHttp" behaviorConfiguration="defaultEndpointBehavior" contract="UtepDevEnv.IDevEnvService" />
			<endpoint address="mex" binding="mexHttpsBinding" contract="IMetadataExchange" />
		</service>
  
2. Deployment
   
	Deploy the UtepDevEnv service to a Web Server (IIS)
   
UtepDevEnv service will now process the requests sent from upload-it4i.sh script located in the Development Environment VM.

DirectorySizeChecker
--------------------

Application used for the Caching use case. It will check the sum size of the user's output folder and if the size is greater than the defined quota, the old results will be automatically deleted.

1. *App.config* configuration

	Change the *mainDirectoryPath* for the results folder, *defaultMaximalSizeMB* for the default quota for all users or add *<result ... />* for a specific users with the specific quota requirements.

	.. code-block:: xml
    
		<applicationConfiguration>
			<results mainDirectoryPath="C:\inetpub\wwwroot\results" defaultMaximalSizeMB="100">
				<result userName="svat_va" maximalSizeMB="150" />
			</results>
		</applicationConfiguration>
		
2. Deployment

	The application is deployed in a Task Scheduler to be automatically invoked every 15 minutes indefinitely. 

BulkProcessing
--------------

Application used for the bulk processing use case. It will check the specific folder for the new data package upload. The WPS request is automatically generated for each new data upload.

1. *App.config* configuration

	Change the *geoServerWPSUrl* for the proxy URI used for the geoserver requests, *mainDirectoryPath* for the data repository directory.

	.. code-block:: xml
    
		<applicationConfiguration>
			<bulkProcess geoServerWPSUrl="http://utep.it4i.cz/geoserver/ows?service=WPS" mainDirectoryPath="C:\UtepApps\BulkProcessingRepository" modifiedDateMinutes="5">
			</bulkProcess>
		</applicationConfiguration>
		
2. Deployment

	The application is deployed in a Task Scheduler to be automatically invoked every 15 minutes indefinitely. 