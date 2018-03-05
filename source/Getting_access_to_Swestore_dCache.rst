Getting access to Swestore dCache
==================================

!! TO DO : Check all wiki links are up to date

These steps are required for all PI:s and users of Swestore dCache resource regardless of how the storage area is later accessed.

Acquire an eScience client certificate (for all users)
```````````````````````````````````````````````````````
Swestore user authentication is currently done by user/client certificates. All project members needs to acquire a certificate by following the instructions on the "`Grid certificates <http://docs.snic.se/wiki/Grid_certificates>`_" page. This step can be performed while waiting for the storage application to be approved. If you already have a valid grid or eScience certificate you don't need to acquire another one.

Manage your eScience client certificate (for all users)
````````````````````````````````````````````````````````
There are multiple suppliers of certificates. Depending on the supplier this step varies a bit.
 
  For Digicert certificates
   If intending to access Swestore from a SNIC resource, please make sure you also `export the certificate <http://docs.snic.se/wiki/Exporting_a_client_certificate>`_, transfer it to the intended SNIC resource and `prepare it for use with grid tools <http://docs.snic.se/wiki/Preparing_a_client_certificate>`_.

  For Nordugrid certificates
   Please make sure to also `install your client certificate in your browser <http://docs.snic.se/wiki/Requesting_a_grid_certificate_from_the_Nordugrid_CA#Installing_the_certificate_in_your_browser>`_.

Register your eScience client certificate in SUPR (for all users)
``````````````````````````````````````````````````````````````````
All project members **have** to register in SUPR and be added to the approved project by the PI. **All users also have to register their certificate in SUPR**. This information is used by Swestore to authenticate the users when accessing the storage area. Registering the certificate is easy though. Make sure your certificate is stored in your browser, log in to SUPR , click "Personal Information" in the left menu, click "Register Client Certificate" and follow the instructions. Please wait for up to 10 minutes for this information to be distributed to Swestore Infrastructure.


