
Installing & Configuring the BC Processing Center
#################################################

Processor repository and data storage structures for Urban TEP
--------------------------------------------------------------

The HDFS file system structures relevant for Urban TEP are:

  .. code-block:: console

    /calvalus/eodata/...  (input datasets of various missions, among them S2)
    /calvalus/auxiliary/urban-footprint/...  (GUF and other footprints)
    /calvalus/software/...  (shared software, among them calvalus, snap, urbantep-timescan)
    /calvalus/projects/urbantep/...  (project-specific results, among them timescan Germany)
    /calvalus/home/tep_<user>/...  (user areas, datasets and uploaded software)

LDAP groups and users and the authorisation scheme
--------------------------------------------------

Urban TEP users arriving via WPS are automatically registered into the BC LDAP. Their EO-SSO user name gets a prefix tep_ in order to avoid name clashes with users of other projects. All users are registered in the calwps group in order to allow submission of processing requests.

There is one system user urban1 used by the portal. Requests from the portal are authenticated against this user. The Urban TEP user comes as a payload in the request header only, without authentication.

For finer control of the access to datasets and processors including sharing of data and processors, additional groups are introduced for the Urban TEP communities if required. Example: tep_coreteam . Such groups are granted access to resources based on ACLs of the HDFS file system.

Initial groups:

* calwps
* tep_coreteam

Initial users:

* urban1
* tep_<severalBcUsers>
* tep_<severalEsaReviewUsers>

Initial group (calwps) ACLs (mode r--) for access to EO input datasets:

* S2_L1C
* Landsat8
* MER_FSG_1P

Initial group (calwps) ACLs (mode r-x) for access to processors:

* SNAP
* Calvalus
* urbantep-timescan
* urbantep-subsetting

Expert users registered for processor development with the BC processing centre get a password for their account. This is sufficient to use the processor upload interface. Deployment within Calvalus is fully automated.

Initial users with upload capability:

* tep_<severalBcUsers>
* tep_<severalEsaReviewUsers>

The home directories for Urban TEP users are initially completely separated from each other (mode 700). This is different from other Calvalus projects where users initially share results and processors. By group ACLs access can be granted to users that like to share data or processors. 

YARN queue configuration for Urban TEP
--------------------------------------

The configuration of the scheduler allocates 4% of the cluster resources (50% * 8%) to the tep queue.

  .. code-block:: console

       <property>
       <name>yarn.scheduler.capacity.root.project.capacity</name>
       <value>50</value>
       </property>
       <property>
       <name>yarn.scheduler.capacity.root.project.tep.capacity</name>
       <value>8</value>
       </property>
       <property>
       <name>yarn.scheduler.capacity.root.project.tep.maximum-capacity</name>
       <value>8</value>
       <!-- <value>100</value> -->
       </property>
 
WPS setup
---------

The WPS software comes with a deploy.sh script for automated deployment of the WPS into a pre-installed Tomcat 8 after transfer of the software to the target VM.

The WPS runs on a backend VM bc-wps in the second DMZ level of BC, only accessible via the Apache front-end server of www.brockmann-consult.de . 

Processing system instance and ingestion services
-------------------------------------------------

Ingestion system instances of the BC infrastructure systematically ingest data from different sources, among the Sentinel 2 L1C. The ingestion systems got configurations to ingest the data of the 6 cities of Urban TEP.

  .. code-block:: console

     'germany': 'POLYGON((5.98865807458 47.3024876979,5.98865807458 54.983104153115,15.0169958839 54.983104153115,15.0169958839 47.3024876979,5.98865807458 47.3024876979))',

     'urban/sao_paulo': 'POLYGON((-47.41538061305751 -24.16080234326242,-47.41538061305751 -22.723497888671186,-45.84878717258703 -22.723497888671186,-45.84878717258703 -24.16080234326242,-47.41538061305751 -24.16080234326242))',  # T23KLP, T23KLQ
     'urban/mexico_city': 'POLYGON((-99.70900979233465 18.85122310111935,-99.70900979233465 20.198696027298624,-98.27933249282161 20.198696027298624,-98.27933249282161 18.85122310111935,-99.70900979233465 18.85122310111935))',  # T14QMG, T14QNG
     'urban/lagos': 'POLYGON((2.1058789021033513 5.819138620787439,2.1058789021033513 8.154758359498196,4.458968090389325 8.154758359498196,4.458968090389325 5.819138620787439,2.1058789021033513 5.819138620787439))',  # T31NEH
     'urban/ho_chi_minh': 'POLYGON((106.21001049932467 10.350689960784472,106.21001049932467 11.249005244904005,107.12452319208158 11.249005244904005,107.12452319208158 10.350689960784472,106.21001049932467 10.350689960784472))',  # T48PXS, T48PXT, T48PYS, T48PYT
     'urban/delhi': 'POLYGON((76.39266751207894 28.012949099044146,76.39266751207894 29.360422025223436,77.9286520679992 29.360422025223436,77.9286520679992 28.012949099044146,76.39266751207894 28.012949099044146))',  # T43RGM, T43RFM
     'urban/cairo': 'POLYGON((29.56115504481353 29.10019592687848,29.56115504481353 31.974804836060954,32.89844731114351 31.974804836060954,32.89844731114351 29.10019592687848,29.56115504481353 29.10019592687848))',  # T36RUU
     'urban/beijing': 'POLYGON((115.4 39.4,115.4 40.9,117.6 40.9,117.6 39.4,115.4 39.4))',  # T50TMK, T50SMK, T50SMJ, T50TNK, T50SNK, T50SNJ
     'urban/london': 'POLYGON((-0.516627355653 51.2668343542,-0.549537089505 51.6922328979,0.316005469631 51.7152155547,0.34091720084 51.2894714993,-0.516627355653 51.2668343542))',
     'urban/basel': 'POLYGON((7.48819758224 47.6430273583,7.75444272578 47.6462273644,7.7589120672 47.4572936625,7.49362146702 47.4541146329,7.48819758224 47.6430273583))',
     'urban/heraklion': 'POLYGON((25.0262567394 35.2729181073,25.0239112268 35.3693421286,25.2230598463 35.3724357469,25.2251692997 35.276000774,25.0262567394 35.2729181073))',

The processing systems for Urban TEP for processing timescan systematically, are hosted on calvalus-production3, a shared VM for production control:

* calvalus-production3:/home/cvop/tep-inst

Reporting
---------

The common reporting service of Calvalus is running on one of the master nodes of the cluster, master00. On this machine, also the Urban TEP report generator is hosted:

* master00:/home/cvop/reporting-inst

This is the service that asynchronously inserts the reports into the Urban TEP accounting service.
