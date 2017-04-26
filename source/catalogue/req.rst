.. _webserversystemconfig :


Compulsory system configuration
"""""""""""""""""""""""""""""""

The Web Server system must be build and installed on a system with the configuration described in this section.

The computing constraints are calculated on the basis of the number of users and
their insfrastructure needs for data and processing.

General Operating System Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Web Server is built and deployed on a similar baseline for the operating system
and the initial configuration:

- Centos 7 x86_64 arch


Computing Constraints
^^^^^^^^^^^^^^^^^^^^^

The minimum computing requirements of the system on which the Web Portal are:

- 8 core or thread CPUs at 2Ghz
- 16Gb RAM
- 250Gb HD
- Gigabit Network interface


Network Security Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default all ports of any protocol on every external network interface is closed for any inbound connection.
The following exceptions must be defined in hte firewall system:

**Frontline Web Server**

- port 443 for HTTPS traffic on protocol TCP 


Web address certificate
^^^^^^^^^^^^^^^^^^^^^^^

A valid and recognised certificate must ne issued for the web address where the web server will run. This certificates and its bundles (intermediary certificates if any) shall be used later in the installation procedure.