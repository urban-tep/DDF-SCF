Deployment Plan
###############

.. uml::

	:cpation: Portal and Data Agency deployment diagram

	skinparam component {
	    BackgroundColor<<tep>> RosyBrown
	    BorderColor<<tep>> black
	}

	cloud "Terradue Cloud Platform" {
		
		[Firewall] as t2fw

		frame "Data Agency" {

				node "Catalogue" << Cluster >> {
				  frame "ElasticSearch" {
				  	database "Indexes"
				  }
				  [Geosquare] -- ElasticSearch
				}
				}

		t2fw -- [Geosquare] : HTTPS

	}

	cloud "IT4I" {
		
		[Firewall] as it4ifw

		frame "Virtual Hosts" {

				node "Web Portal" << VM >> {
				  [HTTP Web Portal Server] << tep >> 
				  database "MySql" {
				  	[DB Tep]
				  }
				  [HTTP Web Portal Server] -- [DB Tep]
				}
			
		}

		it4ifw -- [HTTP Web Portal Server] : HTTPS
	}

	() "https://urban-tep.eo.esa.int/" -- it4ifw : HTTPS
	() "https://data.terradue.com/" -- t2fw : HTTPS



.. req:: TS-FUN-120
	:show:

	The deployment diagram of this section shows how the Catalogue service is deployed on the Terradue Cloud platform


.. req:: TS-SEC-050
	:show:

	The deployment diagram of this section shows that all access to web interfaces are through HTTPS with a certificate


.. req:: TS-REU-010 
	:show:

	The deployment diagram shows that we reuse the Terradue Cloud Platform catalogue as a service already deployed
