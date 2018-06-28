

.. toctree::
   :maxdepth: 3
   
   prerequisites
   kubernetes-install
   kismatic-sample-yaml
   uninstall
   
Sample Helm Chart Parameters  
==========================================

	Build-At-Scale master helm chart is combination of various components and the following page describes all the available options for each helm component.
	
	i) CouchDB:-
		Build-At-Scale uses CouchDB to maintain internal state of various objects and build information. This COuchDB instance runs as a kubernetes pod and is setup via helm chart. To have persistent storage attached for the CouchDB pod ve can specify ontap values in the CouchDB helm chart.
		
		
		
	=======================       ======       ===============================================================================================
	Parameter                     Value        Description                                                                                    
	=======================       ======       ===============================================================================================
	volume                        true         Name of the volume to be created for CouchDB pod                                              
	volumeSize                                 Size of the Volume to be created for CouchDB pod                                              
	volumeMountPath                            Junction Path of the Volume created for CouchDB pod                                            
	volumeUID                                  UID for volume created                                                                        
	volumeGID                                  GID for the volume created                                                                      
	=======================       ======       ===============================================================================================
	

	ii) GitLab:-
		Build-At-Scale deploys GitLab by deafult as an SCM tool. Gitlab is deployed as a kubernetes pod on NetApp storage..
		
		
		
	=======================       ================       ===============================================================================================
	Parameter                     Value                  Description                                                                                    
	=======================       ================       ===============================================================================================
	volume                        true                   Name of the volume to be created for GitLaB pod                                              
	volumeSize                                           Size of the Volume to be created for GitLab pod                                              
	volumeMountPath                                      Junction Path of the Volume created for CouchDB pod                                            
	volumeUID                                            UID for volume created                                                                        
	volumeGID                                            GID for the volume created                                                                      
	=======================       ================       ===============================================================================================
	
	
	
	