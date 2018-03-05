Grid certificates
==================

Short introduction to certificates
----------------------------------

In order to get access to Swestore or any other grid compute or storage resources you must have a valid (eScience) client certificate. This certificate is used instead of a username and password when accessing the resource. The resource itself has a server certificate that ensures you have connected to the right resource. This is exactly the same mechanism used when you use a web browser to contact your bank.

A certificate is the similar to a passport in real-life. In the same way you have to prove who you are when you acquire a passport the same is true for a certificate. A third party, the Certificate Authority (CA), issues a certificate (passport) that both you and the resource trust. They vouch for your identity and has signed your certificate.

A certificate consist of a public key, some user information and the signature of the CA. In addition to the certificate you have a corresponding private key. The private key is secret and should be kept as secure as possible.

The grid certificate and private key is stored in your web browser and/or in your home directory on the host where you will be accessing the resource. Standard file names are::

 ~/.globus/usercert.pem
 ~/.globus/userkey.pem

The certificate contains your public key, your name and organization and a signature by the CA. It is does not contain any username.
The certificate is valid for 13 month and should be renewed yearly.
The private key should be handled with great care. It should only be readable by you and not by the group or others (i.e. "chmod 400 userkey.pem"). Store the key on trusted computers and transfer the key between computers using encryption (using for example scp).
On shared file systems make sure that ~/.globus is not readable by everybody::

     chmod 700 ~/.globus

and on AFS::

    fs sa ~/.globus system:anyuser none

The private key should be encrypted using a passphrase. Anyone that can decrypt the private key will be able to authenticate as you to grid resources. This is similar to the private key in SSH. You must choose a strong passphrase for the private key. This passphrase must not be used anywhere else. You must never ever give away or share the certificate, passphrase or the unencrypted key to someone else.

For more information regarding certificates and public key cryptography:

* http://en.wikipedia.org/wiki/Public-key_cryptography
* http://en.wikipedia.org/wiki/Public_key_certificate
* http://www.nordugrid.org/documents/certificate_howto.html

Requesting a certificate
-------------------------

Certificates are issued by a Certificate Authority or CA. The certificate needed for accessing the Swestore or other grid resources should have the eScience Personal or Grid Premium type, not all CA:s are certified by `The International Grid Trust Federation <https://www.igtf.net/>`_ to issue these.
For users residing in the Nordics there are two relevant CA:s that can issue grid/eScience/e-Science certificates: Digicert and Nordugrid. The Digicert CA is preferred if it is available for your university or research group, but some institutions has not enabled this service yet. The Nordugrid CA can also be used but requires more manual labor by all parties.

NOTE: The Terena CA portal has been replaced with the DIGICERT portal, instructions below might need updating

Recommended procedure for each university:

======================================== ============== ===============================================================================================================
University                               Recommended CA Specific instructions
======================================== ============== ===============================================================================================================
Chalmers University of Technology (CTH)  Digicert	`CTH C3SE Documentation <http://www.c3se.chalmers.se/index.php/Personal_certificates>`_
University of Gothenburg (GU)	         NorduGrid	`SNIC for GU Documentation <http://docs.snic.se/wiki/GU_Certificate_Instructions>`_
Karolinska Institutet (KI)	         Digicert	`KI Documentation <https://internwebben.ki.se/sv/personliga-certifikat>`_
KTH Royal Institute of Technology (KTH)	 Digicert	`SNIC for KTH Documentation <http://docs.snic.se/wiki/KTH_Certificate_Information>`_
Linköping University (LiU)	         Digicert	`LiU Documentation <http://liu.se/insidan/it/irt/personliga-certifikat>`_
Luleå University of Technology (LTU)	 NorduGrid	`Nordugrid CA Documentaion  <http://docs.snic.se/wiki/Requesting_a_grid_certificate_from_the_Nordugrid_CA>`_
Lund University (LU)	                 Digicert	`LU Documentation <http://www.ldc.lu.se/tjanster/it-sakerhet/certifikat>`_
Sveriges Lantbruksuniversitet (SLU)	 Digicert	`SNIC for SLU Documentation <http://docs.snic.se/wiki/Requesting_a_grid_certificate_using_the_Digicert_SSO_Portal>`_
Stockholm University (SU)	         Digicert	`SNIC for SU Documentation <http://docs.snic.se/wiki/SU_Certificate_Information>`_
Umeå University (UmU)	                 Digicert	`SNIC for UmU Documentation <http://docs.snic.se/wiki/UmU_Certificate_Information>`_
University of Borås (UB)	         Digicert	`SNIC for UB Documentation <http://docs.snic.se/wiki/Requesting_a_grid_certificate_using_the_Digicert_SSO_Portal>`_
Uppsala University (UU)	                 Digicert       `SNIC for UU Documentation <http://docs.snic.se/wiki/UU_Certificate_Instructions>`_
======================================== ============== ===============================================================================================================       

`Instructions for the Digicert CA <http://docs.snic.se/wiki/Requesting_a_grid_certificate_using_the_Digicert_SSO_Portal>`_

`Instructions for the NorduGrid CA (use this only if Digicert isn't available at your site) <http://docs.snic.se/wiki/Requesting_a_grid_certificate_from_the_Nordugrid_CA>`_


Proxy certificates
-------------------

Authentication to Swestore can by done using your client certificate directly (as done with your web browser). But on the command line it's usually good practice to use a special short lived proxy certificate. When using other grid resources you must use proxy certificates or other similar mechanisms.

A proxy certificate is bascially a new short lived certificate you issue yourself and then sign using your reglar certificate (or rather your secret key). If you lose this proxy certificate it will shortly expire and then be useless for bad guys. In many grid applications you upload your proxy certificate to the grid resource (the compute element might need your credentials for accessing a storage element as you) and if stolen it can be used to authenticate as you on alla resources you have access to.

There are several tools available for creating, checking and destroying these proxy certificates. The examples below demonstrates the **arcproxy** command from the ARC software suite. Another common tool is the grid-proxy-init from the globus packages.

Creating a proxy certificate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This example requires that the certificate is available for use with grid tools. This is the default with **Nordugrid certificates**, although you might need to transfer the certificate to the resource where you are using the grid tools.

For **Digicert certificates** you must first `export the certificate <http://docs.snic.se/wiki/Exporting_a_client_certificate>`_, transfer it to the resource where you are using the grid tools if needed and `prepare it for use with grid tools <http://docs.snic.se/wiki/Preparing_a_client_certificate>`_. ARC can use the Firefox certificate store directly, as described in the next section.

To create a short lived proxy that can be used for authentication with grid services, the **arcproxy** command can be used. A 12 hour (default) proxy is created in the following example::

 $ arcproxy
 Your identity: /O=Grid/O=NorduGrid/OU=lunarc.lu.se/CN=Kalle Kula
 Enter pass phrase for /home/kalle/.globus/userkey.pem:
 .++++++
 .....++++++
 Proxy generation succeeded
 Your proxy is valid until: 2016-03-11 03:00:14

The proxy file itself will be created in the **/tmp** directory with the format **x509up_uid**, where uid is the user id number for your account.

In some cases a longer lived proxy will be needed. This is achieved using the --constraint switch. A 24-hour can be created by issuing the following command::

 $ arcproxy --constraint="validityPeriod=24H"
 Your identity: /O=Grid/O=NorduGrid/OU=lunarc.lu.se/CN=Kalle Kula
 Enter pass phrase for /home/kalle/.globus/userkey.pem:
 ....++++++
 .....++++++
 Proxy generation succeeded
 Your proxy is valid until: 2011-03-11 15:03:19

Creating a proxy certificate using the Firefox/Thunderbird credential store
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using the ARC client tools it is possible to generate a proxy certificate directly from the Firefox or Thunderbird credential stores. To do this the -F flag is used as shown in the following example::

 $ arcproxy -F
 There are 2 NSS base directories where the certificate, key, and module datbases live
 Number 1 is: /Users/lindemann/Library/Application Support/Firefox/Profiles/t22f3aj2.default
 Number 2 is: /Users/lindemann/Library/Thunderbird/Profiles/7abb733v.default
 Please choose the NSS database you would use (1-2): 1

Here ARC finds the available Firefox and Thunderbird profile in which the credential stores are stored. Next the passphrase for the credential store is used to unlock the stored credentials::

 NSS database to be accessed: /Users/lindemann/Library/Application Support/Firefox/Profiles/t22f3aj2.default
 Enter Password or Pin for "internal (software)":

If the passphrase was correct, ARC will list the available certificates in the credential store and ask you for which you would like to use. ::

 There are 2 user certificates existing in the NSS database
 Number 1 is with nickname: Jonas Lindemann xxxxx@lu.se's TERENA ID (Jonas Lindemann xxxxx@lu.se)
    expiration time: 2013-06-04 01:59:59
 Number 2 is with nickname: Imported Certificate (Jonas Lindemann)
    expiration time: 2014-01-18 16:55:52
 Please choose the one you would use (1-2): 1
 Certificate to use is: Jonas Lindemann xxxxxx@lu.se's TERENA ID
 Proxy generation succeeded
 Your proxy is valid until: 2013-05-01 04:11:37

Checking proxy lifetime
^^^^^^^^^^^^^^^^^^^^^^^^

The remaining lifetime of a proxy certificate can be checked using the **arcproxy** command with the **--info** switch.::

 $ arcproxy --info
 Subject: /O=Grid/O=NorduGrid/OU=lunarc.lu.se/CN=Kalle Kula/CN=1567862803
 Identity: /O=Grid/O=NorduGrid/OU=lunarc.lu.se/CN=Kalle Kula
 Time left for proxy: 11 hours 55 minutes
 Proxy path: /tmp/x509up_u500
 Proxy type: X.509 Proxy Certificate Profile RFC compliant restricted proxy
 In this example the proxy certificate is valid for 11 hours 55 minutes more.

Destroying a proxy certificate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
A proxy can be destroyed with the **-r** or **--remove** switch.::

 $ arcproxy -r

or::

 $ arcproxy --remove

