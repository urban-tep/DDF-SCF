.. _bcpcsystemconfig :


System configuration
""""""""""""""""""""

The Urban TEP BC processing system is configured into the Calvalus system of BCthat runs on top of a Hadoop cluster. Additional common elements of the BC infrastructure used are the LDAP user management and some VMs hosted on a Proxmox Hypervisor.

Hadoop cluster
--------------

The Hadoop cluster of BC currently comprises 98 compute nodes, 5 additional archive nodes, 2 master nodes, and about 1.9 PB of EO data storage. The cluster is used in a shared way, where Urban TEP is one of many concurrent projects running jobs on the cluster. Urban TEP indirectly makes use of Hadoop HDFS and Hadoop YARN.

* Hadoop version 2.7.3

Calvalus system
---------------

Urban TEP makes use of Calvalus by using the file system organisation on HDFS, the pre-installed EO software packages for processing, among them SNAP, the request handling and production workflows, the processing queue configurations, the WPS interface, the processor interfaces and the automated deployment, the versioning approach for concurrent versions, and the access control scheme of Calvalus.

* Calvalus version 2.10 and 2.11-SNAPSHOT

ESA Sentinel toolbox
--------------------

* SNAP 5.0 and 6.0-SNAPSHOT

VM host
-------

Urban TEP uses VMs to host the WPS server and several shared virtualised production servers to control automated data ingestion and data-driven production. The hypervisor hosting the VMs is controlled by Proxmox.

LDAP user management
--------------------

Urban TEP makes use of the user management of BC. Urban TEP users processing or developing on the BC processing centre are registered in an LDAP server.

Operating system
----------------

The operating system used for the infrastructure is Ubuntu. Docker images with processors can be based on CentOS or other Linux distributions.

* Ubuntu 14.0.4 LTS
* Ubuntu 16.0.4

