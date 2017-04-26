
Overview
=========

.. toctree::
   :maxdepth: 2

   changelog
   references


This Urban TEP Sofware Configuration File describes the build and installation of the different Urban TEP subsystems. Each component is autonomous and can be configured stand-alone independently of the infrastructure. For the interface items exchanged and the protocol used between the subsystems, see the :ref:`ICD/API in [RD8] <RD8>`. The configuration described in this plan are specific to Urban TEP operational platform.

This SCF assumes that readers are familiary with the :ref:`SDD in [RD8] <RD8>`. Please refer to the SDD for the context of the interface information.


Deployment overview
-------------------

The following diagram shows the deployment plan of the Urban TEP.

.. include:: deployment_plan.rst

The main components for the Urban TEP are:

 - Web Server (portal)
 - PUMA
 - Catalogue
 - Processing centers

The document is therefore divided according to the components. Build and install procedures for each of them are described in the following chapters.

