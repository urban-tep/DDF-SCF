
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

HTTP handler used as a proxy for the WPS services.

TODO

UtepDevEnv
----------

TODO

DirectorySizeChecker
--------------------

TODO

BulkProcessing
--------------

TODO