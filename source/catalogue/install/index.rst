
Installing & Configuring the Web Server
#######################################



Pre-requisites
""""""""""""""

In addition of the :ref:`webserversystemconfig`, the following software must be installed on the system building the Werb Server.

- Apache Server 2.4
- Mono 4.8+ installed and configured : `http://www.mono-project.com/docs/getting-started/install/`_
- MySQL server 5.6
- Shibboleth
  

A batch script of all the commands to execute for the preriquisites to be installed properly on CentOS7


  .. code-block:: console
  
      yum install yum-utils
      rpm --import "http://keyserver.ubuntu.com/pks/lookup?op=get&search=0x3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF" 
      yum-config-manager --add-repo http://download.mono-project.com/repo/centos/
      curl -o /etc/yum.repos.d/shibboleth.repo http://download.opensuse.org/repositories/security://shibboleth/CentOS_7/security:shibboleth.repo
      yum install epel-release httpd mysql-server mod_fcgid mod_ssl shibboleth
      yum update -y
  


Initialize the database
"""""""""""""""""""""""

1. Start mysql

  .. code-block:: console
    
       systemctl start mysqld.service


2. Set root password for mysql 
   
  .. code-block:: console
   
       mysqladmin -u root password install


  .. warning:: This password is temporary for the installation and configuration time. Change it as soon as the installation is completed.
    
3. Enable the mysql service at startup

  .. code-block:: console
  
      systemctl enable mysqld.service


Install the Web Server
""""""""""""""""""""""

1. Download the latest RPM for CentOS7 in the releases at https://github.com/urban-tep/webserver/releases or :ref:`build from source <buildwebserver>`
   
2. Execute the RPM installation with yum
   
   .. code-block:: console
   
       yum localinstall webserver-X-x.y.el7.noarch.rpm


3. Control the installation output, especially for the first time install where the database is built.
   

Configure the server certificate
""""""""""""""""""""""""""""""""

From the Certificate Authority, there should be 3 files that you store locally on the operating machine

- server certificate in /etc/pki/tls/certs/|Xtepaddress|-cert.pem
- server key (unprotected) in /etc/pki/tls/private/|Xtepaddress|-key.pem
- server CA bundle (intermediary CA certificates) in /etc/pki/tls/certs/|Xtepaddress|-ca-bundle


Configure Shibboleth
""""""""""""""""""""

1. Copy the shibboleth config file from the installation directory to the shibboleth directory
   
  .. code-block:: console
   
      cp /etc/shibboleth/shibboleth2-tep[X].xml /etc/shibboleth/shibboleth2.xml


2. Start shibd

  .. code-block:: console
   
      systemctl start shibd


3. Control /var/log/shibd/shibd.log to check there is no error in the logs


4. Enable shibboleth at startup
   
  .. code-block:: console
  
      systemctl enable shibd



Start Apache
""""""""""""

1. Start Apache

  .. code-block:: console
   
      systemctl start httpd


2. the command output should not return any error


4. Enable Apache at startup
   
  .. code-block:: console
  
      systemctl enable httpd


The server should now be accessible at https://|Xtepaddress|
