.. Build-At-Scale documentation master file, created by
   sphinx-quickstart on Fri May 11 11:09:06 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Build-at-Scale's documentation
==========================================

Build-At-Scale is an end-to-end CI-CD framework from creating code repository to deploying builds. All these processes run as kubernetes pods and use persistent Netapp storage with NetApp Service Level Manager.

The advantage of running CI process on NetApp for business or asset owner are –

	1. **Improve developer productivity**

Netapp Flexclones allows to cut down the user workspace provisioning from hours and days to few seconds.
Each user workspace is pre-packaged with the source code and the dependencies like libraries, tools, compilers and config files for developers start working quickly. There are no long wait times for developers to prepare their workspaces.

	2. **Reduce build time**

NetApp snapshots and flexclones for the CI data volumes allows developers to clone copies of the latest builds and merge the changes with their own incremental changes. 
Incremental builds allow developers to test and merge the changes quickly in their workspaces and provides consistency to the builds with reduced build times.

	3. **Cloud-IDE integretion**

Along with integretion of Theia cloud IDE, Build-At-Scale provisions workspaces attached to a cloud based IDE which ensure a stable build environment and the developers do not have to worry about any dependency changes.
	
	
	4. **Reduce infrastructure costs**
	
NetApp flexclones are thin provisioned. This allows to provision more build workloads with less storage footprint and optimize compute and network resources for parallel builds. Thus providing an improved return of investment(ROI) by doing “more with less” in the entire build farm.
Data compaction and de-duplication for the source code repositories and the software builds during the CI process on NetApp provides a high degree of storage space efficiency. Data compression of build artifacts in the binary repository also provides space savings.



Contents
=======================================================
.. toctree::
   :maxdepth: 3
   
   prerequisites
   kubernetes-install
   kismatic-sample-yaml
   helm-chart-values
   uninstall
   support
   
   
   
