.. toctree::
   :maxdepth: 3
   :caption: Contents:
   
.. prerequisites:
   
Pre-requisites
=================================================

	* 1 running instance of `NetApp Service Level Manager(NSLM) <https://mysupport.netapp.com/documentation/docweb/index.html?productID=62414&language=en-US>`_.
	* Kubernetes 1.9+ installation

	

.. note:: * All validation in this solution has been done with RHEL 7.3, the source files can run on any flavour of Linux.
		  * Any number of nodes can be added in the kubernetes cluster, for demo purposes a 3 node cluster is considered in this validation.
		  * Keep the following storage details handy:
				#. Data LIF IP:                        ________________________
				#. Storage virtual machine(SVM) name:  ________________________
				#. NSLM server IP:                     ________________________
				#. NSLM username:                      ________________________
				#. NSLM password:                      ________________________
				#. Aggregate name:                     ________________________