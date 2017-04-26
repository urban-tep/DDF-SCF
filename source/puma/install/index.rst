
Installing & Configuring PUMA
#############################


Running the following three commands starts the docker container for Panther with data, which should survive stored in volumes. In order to run correctly the docker container needs at least 5GB of RAM available. 

.. code-block:: console

    docker login
    docker pull mbabic84/panther:new
    docker run -dt --name panther -p 80:80 -v panther_tmp:/tmp -v panther_data:/data -v panther_postgresql:/var/lib/postgresql -v panther_tomcat7temp:/var/lib/tomcat7/temp -v panther_tomcat7webapps:/var/lib/tomcat7/webapps -v panther_gnode_uploaded:/home/geonode/geonode/geonode/uploaded mbabic84/panther:new cli


Before proceeding it is necessary to make sure that the configuration repository https://github.com/gisat/docker contains directory with configuration for the new machine. The configuration directory contains configuration files for external dependencies: apache, postgresql, geoserver, geonode, wordpress and internal applications: backend, front-office, back-office.

Core changes in the configuration needs to be done in order to support running on different Ip address or name. The backend/config.js is attached as backendExampleConfig.js and following must be changed

  - remoteAddress, geonodeHost must reflect the current address of the container. In case you are running the docker under urban-tep.gisat.cz it needs to be urban-tep.gisat.cz
  
Then it is necessary to update backoffice/rewrites.js which is attached as backOfficeRewrites.js and following must be changed

  - apiHost, geonodeAddress, frontOfficeAddress must reflect current address of the container. In case you are running the docker under urban-tep.gisat.cz it needs to be urban-tep.gisat.cz
  - geoserverAddress must reflect current address of the container. In case you are running the docker under urban-tep.gisat.cz it needs to be urban-tep.gisat.cz/geoserver
  
Then it is necessary to update frontoffice/config.js which is attached as frontOfficeConfig.js and folowing must be changed

  - url must reflect current address of the container. In case you are running the docker under urban-tep.gisat.cz it needs to be urban-tep.gisat.cz/backend/
  - signupAddress must reflect current address of the container. In case you are running the docker under urban-tep.gisat.cz it needs to be urban-tep.gisat.cz/account/signup/

Once you have prepared directory with the updated config files it is possible to run all the aplications.  This is done via the command _control start -f configurationDirectory in case of configuration stored in directory urban-tep.gisat.cz the command will be: ``_control start -f urban-tep.gisat.cz``
