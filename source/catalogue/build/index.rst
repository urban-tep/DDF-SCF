.. _buildwebserver :


Building the Web Server
#######################



Pre-requisites
""""""""""""""

In addition of the :ref:`webserversystemconfig`, the following software must be installed on the system building the Werb Server.

- Apache maven 3.0.x installed and configured : `https://maven.apache.org/install.html`_
- Mono 4.8+ installed and configured : `http://www.mono-project.com/docs/getting-started/install/`_
- RPM build installed : ``yum install rpm-build``
  

Build
"""""

1. Check out or download the latest source code at https://github.com/urban-tep/webserver
2. In the root directory, execute
   
   .. code-block:: console
   
       /usr/local/apache-maven/bin/mvn -q package



  where /usr/local/apache-maven is where resides your maven installation.

  After few minutes, the build should be completed and the RPM package for the installation is ready in
  ``target/rpm/webserver-X/RPMS/noarch/webserver-X-x.y.el7.noarch.rpm``

3. Keep a copy of the RPM for the installation procedure.


