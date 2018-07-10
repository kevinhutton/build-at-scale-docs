

.. _installing_kubernetes:
   
Installing Kubernetes using Kismatic Tool
================================================================
	.. note :: We recommend using Kismatic to install Kubernetes, although users are free to install kubernetes cluster using their own preffered method as well.
	
Pre-Requisites 
--------------------------------------	
	i)    3 RHEL 7.2 nodes with atleast 8GB memory
	ii)   All machines should be resolvable by hostnames
	iii)  A user account with passwordless 'sudo' access on all the nodes.
	
	
	
Installation 
--------------------------------------

	i) Setting up new useraccount on RHEL nodes
	
	Create a new user account on all RHEL nodes using following command :-
	
	.. code-block:: shell
				
		>useradd kubernetes
		
	
	ii) Set a password for this newly created user
	
	.. code-block:: shell
				
		>passwd kubernetes
		
	iii) Set a passwordless ssh access between all hosts 
	
	.. code-block:: shell
		
		 >ssh-keygen -t rsa
		 
	iv) Copy the generated key to other hosts using ssh-copyid
	
	
	.. code-block:: shell
	
		>ssh-copy-id -i ~/.ssh/mypublickey kubernetes@host1
		>ssh-copy-id -i ~/.ssh/mypublickey kubernetes@host2
		>ssh-copy-id -i ~/.ssh/mypublickey kubernetes@host3
		
		
	5. Setup passwordless sudo for the newly created user by editing /etc/sudoers

	
	.. code-block:: shell
	
		>nano /etc/sudoers
		
	Append the following line at the end of /etc/sudoers
		
	.. code-block:: shell	
	
		kubernetes            ALL = (ALL) NOPASSWD: ALL
		
	
	6. Turn swap off on all linux machines
	
	.. code-block :: shell
	
		> swapoff -a
	
	7. Install Kismatic as per `following documentation	 <https://github.com/apprenda/kismatic>`_.
	
		A sample installation yaml file is described `here <kismatic-sample-yaml.html>`_.
	
	
	8. Download and Install Helm Package Manager
	
		8.1 Download the lastest stable version of `helm <https://github.com/kubernetes/helm/releases>`_.
		
		8.2 Unpack it 
		
		.. code-block :: shell
		
			>tar -zxvf helm-package.tar.gz
			
		8.3 Move the extracted helm binary to /usr/local/bin
		
		.. code-block :: shell
		
			>mv linux-amd64/helm /usr/local/bin/helm

.. _installing_build-at-scale:
   
Installing Build-at-Scale
================================================================


Installation 
--------------------------------------

	1. Download Build-At-Scale code 
	
	.. code-block:: shell
	
			git clone ssh://git@ngage.netapp.com:7999/dcs-bb/na-build-at-scale.git
			
	2. Specify Helm Configuration for build-at-scale installation. This yaml file consists of your storage details.
	
	.. code-block:: shell 
	
		>cat values.yaml
 
		global:
		 
		  scm:
		 
			type: "gitlab"
		 
		  registry:
		 
			type: "docker"
		 
		  ontap:
		 
			automaticVolumeCreation: true
		 
			dataIP: <ontap_data_lif_ip>
		 
			apiIP: <ontap_nslm_ip>
		 
			user: <ontap admin username>
		 
			password: <ontap admin password>
		 
			svm: <svm>
		 
			aggregate: <aggegate>
	
	
	
	
	Provide values.yaml to tha Build-at-Scale Helm YAML chart.

   
    
    =======================       ======       ===============================================================================================
    Parameter 	                  Value        Description
    =======================       ======       ===============================================================================================
    automaticVolumeCreation       true         This option allows you to automatically create volumes required for build-at-scale installation 
    dataIP                                     IP address of Data LIF on your ONTAP cluster
    apiIP                                      Enter IP-Adress:Port for your NSLM install
    user                                       Enter username for NSLM instance
    password                                   Enter password for NSLM instance
    svm                                        Enter SVM name
    aggregate                                  Enter the Aggregate name to create your volumes
    =======================       ======       ===============================================================================================
   
	
	3. Initialize Helm  with tiller Service account 
	
	.. code-block:: shell
	
		>helm init --service-account tiller --upgrade
	
		
	
	
	4. Install Helm Chart using following command :
	
	.. code-block:: shell 
	
		>helm install --name build-at-scale .
		
		
	5. Wait for all services to be ready :
	
	.. code-block:: shell 
	
		>kubectl get pods | grep build-at-scale
 
		NAME                                              READY     STATUS    RESTARTS   AGE
		 
		build-at-scale-couchdb-58f48c5b8d-vw9mb           1/1       Running   0          3m
		 
		build-at-scale-docker-registry-7969844c9f-phshp   1/1       Running   0          3m
		 
		build-at-scale-gitlab-6c6dc79b77-j4dww            1/1       Running   0          3m
		 
		build-at-scale-jenkins-74d87d6fd5-th29g           1/1       Running   0          3m
		 
		build-at-scale-webservice-5bbcdbf88c-rjrp4        1/1       Running   0          3m
		
	.. note:: It may take 10minutes for all the pods to come up.
	
	
	6. Take note of the service ports from the following command :
	
	.. code-block:: shell
	
		>kubectl get svc
	
				NAME                                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                                  AGE
		 
		build-at-scale-couchdb                     NodePort    10.108.249.65    <none>        5984:14339/TCP                           5m
		 
		build-at-scale-docker-registry             NodePort    10.97.110.240    <none>        5000:24646/TCP                           5m
		 
		build-at-scale-gitlab                      NodePort    10.102.216.157   <none>        80:*30593*/TCP,22:8639/TCP,443:18600/TCP   5m
		 
		build-at-scale-jenkins                     NodePort    10.99.97.28      <none>        8080:*12899*/TCP                           5m
		 
		build-at-scale-jenkins-agent               ClusterIP   10.100.249.190   <none>        50000/TCP                                5m
		 
		build-at-scale-webservice                  NodePort    10.101.38.243    <none>        5000:*12054*/TCP   

	
	
	.. note:: Kubernetes assigns random ports for services, make sure you copy the service ports correctly.
	
	

Configuring Build-at-Scale
--------------------------------------

	1. **Setting up CouchDB:**
	
		
		Build-at-Scale uses CouchDB in backend to store all the build data and information. 
		To configure CouchDB just visit the following URL: 
		
		.. code :: shell 
		
			http://<<Kubernetes-IP>>:<<CouchDBPort/backend/admin/setup
		
		.. figure:: images/couchdbconf.PNG
			:width: 100%
			:alt: CouchDB configuration
			
		.. note:: 
		
				Build-at-Scale automatically configrues the iniital CouchDB tables just by visiting the above link.
				
				
	2. **Configure GitLab:**
	
	
		Build-at-Scale packages Gitlab as a SCM in its helm chart. An initial account has to be created on GitLab before starting to use it.
		To create an account on GitLab, visit the following URL and sign up.
		
		.. code :: shell 
		
			http://<<Kubernetes-IP>>:<<GitLabPort>>
		
		
		.. figure:: images/gitlab1.PNG
			:width: 100%
			:alt: GitLab	
		

	3. **Creating a CI Pipeline from Build-at-Scale:**
	
	
		Buiid-at-Scale allows you to setup a Jenkins-CI pipeline from the Build-at-Scale UI itself. CI Pipeline can be created from following location-

		.. code :: shell 
		
			http://<<kubernetes-url>>:<<webservicePort>>/frontend/project/create
		
	
    =======================       =======      ================================================================================================
    Parameter 	                  Value        Description
    =======================       =======      ================================================================================================
    SCM URL                                    Enter URL of your git project from GitLab                                                     
    SCM Branch                    master       Enter code branch for the CI process         
    Export Policy                 default      Export policy on storage for the volume created for this CI Build
    =======================       =======      ================================================================================================
	
		.. figure:: images/create_pipeline.PNG
			:width: 100%
			:alt: Create CI Pipeline
	
	
	4. **Deploy your App to Apprenda :**
	
	
		4.1) Your app can be deployed directly to Apprenda from Jenkins
		
		4.2) Install `Apprenda Jenkins Plugin  <https://plugins.jenkins.io/apprenda>`_.
		
		4.3) Create a New FreeStyle Jenkins job (e.g: Deploy_To_Apprenda) in Jenkins.
		
		4.4) Add a Label parameter "node" for the Freestyle job	
		
		.. figure:: images/labelparameter.PNG
			:width: 100%
			:alt: Label parameter   
		
		
		4.5) Configure Apprenda credentials and build zip path in the FreeStyle job as described `here <https://github.com/jenkinsci/apprenda-plugin>`_.
		
		4.6) Copy the below groovy stage and add it to the pipeline generated by Build-At-Scale as a  pipeline stage
		
		.. code :: shell
			
				stage('Deploy To Apprenda') {
                      
                     build job: 'Deploy_To_Apprenda', parameters: [[$class: 'LabelParameterValue', name: 'node', label: "${podLabel}"]]
                    
                   } 
		

		4.7) Save the pipeline
		
		
		.. note:: Deploying app to Apprenda is optional. This step can be skipped if you have some other CD mechanism.
				   
		
	5. **Create user workspaces from Build-at-Scale UI:**
		
		Build-at-Scale allows you to create userworkspaces bound with an cloud IDE. To create cloud workspace, navigate to Create Workspace tab in the Build-at-Scale UI.
			
			
		
    =======================       =======      ================================================================================================
    Parameter 	                  Value        Description
    =======================       =======      ================================================================================================
    Git Project                                Select project to create a workspace                                                          
    Username                                   Enter developer username                     
    Workspace Prefix                           Enter a prefix to identify workspaces                               
	Build                                      Select a successful build to create a workspace
    =======================       =======      ================================================================================================
	
				
		.. figure:: images/create_workspace.PNG
			:width: 100%
			:alt: Cloud9IDE	
		
		5.1) Once a workspace is created, Build-at-Scale automatically attaches the workspace to a Cloud9 IDE. This cloud workspace is accesible via a web browser.
		
		5.2) User can set his git username and start working in the workspace.
		
		
	.. figure:: images/cloud9IDE.PNG
			:width: 100%
			:alt: Cloud9IDE	
		
		
	6. **Create user workspaces from Build-at-Scale UI:**
		
		Build-at-Scale alows users to merge workspaces. This allows users to pull in the latest code without losing their own changes. Merging workspaces will also saves up on build times. To merge workspaces, navigate to the Merge Workspace tab and fill in the following values :-

    =======================       =======      ================================================================================================
    Parameter 	                  Value        Description
    =======================       =======      ================================================================================================
    Username                                   Enter developer's username 
    Workspace Name Prefix                      Enter an prefix for workspace for easy identification.
    Source Workspace name                      Enter name of the source workspace to merge                         
	Build                                      Enter the build name to merge a workspace from.
    =======================       =======      ================================================================================================
	
	
	.. figure:: images/create_workspacemerge.PNG
			:width: 100%
			:alt: Workspace Merge
	
				