Using Swestore dCache
======================

Download and upload data
-------------------------
From the command line
^^^^^^^^^^^^^^^^^^^^^^
There are several command line tools capable of using the protocols provided by Swestore. For interactive usage on SNIC clusters we recommend using the ARC tools which should be installed on all SNIC resources.

As an integration point for building scripts and automated systems we suggest using the curl program and library.

 ARC
   Please see the instructions for Accessing Swestore with the ARC client. Recommended method when logged in on SNIC resources.
 lftp
   See the instructions for Accessing Swestore with lftp.
 cURL
   Please see the instructions for Accessing Swestore with cURL. We suggest using this as integration point for building scripts and automated systems.
 globus-url-copy
   Please see the instructions for Accessing Swestore with globus-url-copy.
 gfal2
   (not documented yet)
 https://duck.sh/
   Command-line client for Windows/macOS (not documented yet)

Using a GUI client
^^^^^^^^^^^^^^^^^^^
Graphical User Interface (GUI) clients are known to work on some operating systems.
 Cyberduck
  Please see the instructions for `Accessing Swestore using Cyberduck <http://docs.snic.se/wiki/Accessing_Swestore_using_Cyberduck>`_ **Recommended method on Windows and macOS**

From a web browser
^^^^^^^^^^^^^^^^^^
Swestore is accessible in your web browser as a simple directory index interface at https://webdav.swestore.se/. To browse private data you need to have your certificate installed in your browser (see above). Projects are organized under the /snic directory as https://webdav.swestore.se/snic/YOUR_PROJECT_NAME/.

Enabled access protocols
-------------------------
A design criteria for Swestore is to provide the storage over a number of standardized and public protocols. There is no vendor specific client needed for access.
 GridFTP
   Also called gsiftp. Well supported within Swestore.
   Features: Transfer checksums. Writes goes directly to storage pools for high speed transfers.
   Access URL: gsiftp://gsiftp.swestore.se/
 HTTP/WebDAV
   Contender for being the recommended protocol for Swestore.
   Features: Direct support in web browsers. Redirected reads and writes if special url is used and client supports it.
   Access URL: http://webdav.swestore.se/ (unauthenticated, read-only, non-redirected)
   Access URL: https://webdav.swestore.se/ (authenticated, read-write, non-redirected)
   Access URL: http://webdav.swestore.se:1080/ (unauthenticated, read-only, redirected)
   Access URL: https://webdav.swestore.se:1443/ (authenticated, read-write, redirected)
 NFSv4.1
   Used by some communities, not recommended for general use
 SRM - Storage Resource Manager
   Used by some communities, not recommended for general use.
   Access URL: srm://srm.swegrid.se/
 Xrootd
   Used by some communities, not recommended for general use
 DCAP
   Used by some communities, not recommended for general use

Tools and scripts
------------------
There exists a number of tools and utilities developed externally that can be useful. Here are some links:
* ARC Graphical Clients - Contains the ARC Storage Explorer
* Transfer script, swetrans_arc, provided by Adam Peplinski / Philipp Schlatter
* Documentation of the ARC Python API (PDF)

Slides and more
----------------
Slides and material from seminar for Lund users on April 18th

Usage monitoring
-----------------
* On the project page in SUPR: https://supr.snic.se
* On the monitoring server for Swestore: http://status.swestore.se/munin/monitor/monitor/ (only accessible from .se-domains.


