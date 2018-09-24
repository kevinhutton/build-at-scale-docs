.. toctree::
   :maxdepth: 2
   :caption: Contents:
   
.. uninstall:
   
Uninstalling Build-at-Scale
=================================================

	* Build-at-Scale can be uninstalled using a single helm command
	
	.. code ::
	
		>helm del --purge build-at-scale
		
	.. note :: Deleteing helm deployment does not delete the volumes created by the helm chart. You will have to manually delete the volumes created on storage.
	
	
	

.. uninstall_kismatic:
   
Uninstalling kismatic cluster
=================================================

	* Kubernetes cluster installed via kismatic can be uninstalled easily using a single kismatic command.
	
	.. code :: shell 
	
		>./kismatic reset
		
	.. note :: The reset command uninstalls your kubernetes cluster and all packages installed by kismatic on the master as well as the slave nodes. 