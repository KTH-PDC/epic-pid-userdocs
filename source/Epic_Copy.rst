How does it work?
=================

Persistent Identifiers
----------------------

A persistent identifier (PID) consists of a prefix and a postfix separated by a forward slash /. The prefix is a single number and groups one or more postfixes maintained by an institution or individual. The postfix is a unique alphanumeric string of characters. The combination of the pre- and postfix uniquely identifies a specific piece of data on a specific location by URL. An example PID is::

    "11304/8656df25-1729-4649-b3d5-38e5f13c4388"

With a PID the actual data is not retrieved yet. To achieve this, a PID resolving service is required.

Typically, a PID does not only provide the location URL of the data the PID is referring to. In many cases additional metadata is given, for example the identifier type, owner, etc.

Resolving PIDs
----------------

The EPIC PID service provides unique persistent identifiers and allows management of the metadata accompanying the PID. To actually resolve a given PID to its corresponding location, a PID resolving service is required. For a PID registered with the EPIC PID service, the Handle service run by Corporation for National Research Initiatives (CNRI) provides these locations based on the PID. For example, to resolve the PID given above, use the Handle service proxy URL followed by the PID to obtain the actual location of the data:
    
http://hdl.handle.net/11304/8656df25-1729-4649-b3d5-38e5f13c4388

leads to

https://b2share.eudat.eu/records/257f3e2a22b44f228ef234c576f142ec

The link does not provide the data, but navigates to a landing page on which the description, metadata and files of the data set are given.



Obtaining an account
====================

We are pleased to help you with gaining access to the ePIC PID service, answer your questions or assist you to specific requests you may have about the service and SNIC. Please contact us through our support@swestore.se.

For tests we can provide you with an account under a test prefix. This can be used to test your procedures and handles. The handles under the test prefix are NOT permanent and account is time limited.

Reverse Lookup
================

As extra functionality we provide a reverse lookup function. Normally a handle is created and an URL is placed in. We offer the possibility to search in the database if the URL is already in the database. This could prevent having 2 different handles pointing to the same URL. But that is application dependant. We call this a reverse lookup. If you are interested we will provide you with a separate "<username>:<password>” and link.


New users
=========

As a new user, first you need to generate a private/public keypair and a certificate. This is needed to mint handles. The public key will be uploaded in a handle under a specific index. This will be used to authenticate the user. 

**Before starting first confer with SNIC support at support@swestore.se  which handle and index to use for the certificate to create.** 

The example below uses foo/USER01 at index 310.

Creating the client certificate
-------------------------------

For authentication using client certificates, a special pair of keys and a certificate file is required. Follow these five steps to create them for your users:

    #. Creating a private/public key pair
    #. Send the public key to SNIC
    #. Transforming the binary private key (.bin) to a .pem file
    #. Creating the certificate file
    #. Removing the public key from the certificate

1. Creating a private/public key pair
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create the private/public key pai you can use the command line tool hdl-keygen that is shipped together with the Handle.net system software. Install the software, change directory to the install location (or use relative paths) and execute::

    bash /.../handlesystem_software/hsj-8.x.x/bin/hdl-keygen
              -alg rsa
              -keysize 4096
               foo_USER01_310_privkey.bin foo_USER01_310_pubkey.bin

**Note:** We put foo_USER01_310 into the name to remember for which user name this key pair is generated! When it asks whether you want to encrypt the key, type ‘n’:

Would you like to encrypt your private key? (y/n) [y] n

2. Send the public key to SNIC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Please send your public key file foo_USER01_310_pubkey.bin to the SNIC helpdesk at support@swestore.se.

Create a message for the attention of "The SNIC ePIC PID handle service team". Include your name, the public key and the assigned prefix on the handle system.

3. Transforming the binary private key (.bin) to a .pem file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For this, you can use the command line tool hdl-convert-key that is shipped together with the Handle.net system software::

    bash /.../handlesystem_software/hsj-8.x.x/bin/hdl-convert-key
                                    foo_USER01_310_privkey.bin
                                 -o foo_USER01_310_privkey.pem

4. Creating the certificate file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create the certificate using openssl with specifying a subject::

    openssl req -pubkey -x509 -new -sha256 -subj "/CN=310:foo\/USER01" -days 3652
                                -key foo_USER01_310_privkey.pem
                                -out foo_USER01_310_certificate_and_publickey.pem

A file foo_USER01_310_certificate_and_publickey.pem should be generated.

5. Removing the public key from the certificate file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Execute the following command::

    openssl x509 -inform PEM -in foo_USER01_310_certificate_and_publickey.pem
                         -out foo_USER01_310_certificate_only.pem


EPIC PID Usage
==============

SNIC uses the handle HTTP JSON REST API to mint handles. This means to create, read, update or delete handles. This API is described in the handle documentation http://www.handle.net/tech_manual/HN_Tech_Manual_8.pdf in chapter 14.

Usage of the HTTP REST API
----------------------------

There are several ways to interact with the handle HTTP REST API to {create|modify|delete} handles. Some examples will be given in the following sections.

Native curl
^^^^^^^^^^^^

A simple script is::

    #!/bin/bash
    
    #### modify following lines. assuming 310:<PREFIX>/USER01 for authentication 
    PREFIX=<insert my own prefix here>
    PID_SERVER=https://130.239.81.124:8000/api/handles
    MY_PATH=<insert path to where privkey and certificate are>
    PRIVKEY=${MY_PATH}/${PREFIX}_USER01_310_privkey.pem
    CERTIFICATE=${MY_PATH}/${PREFIX}_USER01_310_certificate_only.pem
    #### end modify lines
    SUFFIX=`uuidgen`
     
    curl -v -k --key $PRIVKEY --cert $CERTIFICATE \
        -H "Content-Type:application/json" \
        -H 'Authorization: Handle clientCert="true"' \
        -X PUT --data \
            '{"values": [
                {"index":1,"type":"URL","data":{"format":"string","value":"http://www.test.com"}},
                {"index":100,"type":"HS_ADMIN","data":{"format":"admin",
                    "value":{"handle":"0.NA/'$PREFIX'","index":200,"permissions":"011111110011"}}}
            ]}' \
    $PID_SERVER/$PREFIX/$SUFFIX
    
**NOTE:** for MacOS users: make sure the curl version is compiled with OpenSSL support. The included version in MacOS does not work out of the box. See Known issues for a solution.

Python library with API
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is possible to access the EPIC PID service using a Python library. The code can be found on GitHub https://github.com/EUDAT-B2SAFE/PYHANDLE

Usage of the HTTP reverse lookup mechanism
---------------------------------------------

SNIC supports a use case where you search the handle database to see if the URL is already used and has a PID assigned to it. This can prevent the case where a URL is assigned two or more PIDs. This is called handle reverse lookup. For this usage a separate username/password needs to be used.

Examples via curl are::

    curl -u "username:password" https://130.239.81.124:8000/hrls/handles?URL=*
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles?URL=http://www.test.com
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles?URL=http://www.test.com&EMAIL=mail@test.com
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles?URL=*&limit=20
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles?URL=*&limit=20&page=0

    curl -u "username:password" https://130.239.81.124:8000/hrls/handles/21.T16999?URL=*
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles/21.T16999?URL=http://www.test.com
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles/21.T16999?URL=*&limit=20
    curl -u "username:password" https://130.239.81.124:8000/hrls/handles/21.T16999?URL=*&limit=20&page=0
    
To retrieve full Handle records, set the optional "retrieverecords" parameter to true::

    https://130.239.81.124:8000/hrls/handles?URL=*&retrieverecords=true

**NOTE:**

    It will decode the standard strings, but NOT the handle specific records.
    The maximum of limit is 100000. The default of limit is 1000. By default it will only show 1000 matches when searching.

Software
========

Resolving all handles can always be done by the Handle software via: http://hdl.handle.net/

The software needed to generate private/public key pairs and convert a binary key to pem format can be found at: http://www.handle.net/download_hnr.html

Known issues
=============

Common problems
----------------

Some common problems when authenticating, together with possible solutions. Please note that the provided problem causes are causes we observed. Of course it is possible that other reasons may cause the same problems, in that case these solutions may not work.

MacOS curl
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Problem

* Trying x.x.x.x...
* TCP_NODELAY set
* Connected to epic3.storage.surfsara.nl (x.x.x.x) port 8007 (#0)
* WARNING: SSL: CURLOPT_SSLKEY is ignored by Secure Transport. The private key must be in the Keychain.
* WARNING: SSL: Certificate type not set, assuming PKCS#12 format.
* SSL: Can't load the certificate "/<path>/<cert>.pem" and its private key: OSStatus -25299
* Closing connection 0

curl: (58) SSL: Can't load the certificate "/<path>/<cert>.pem" and its private key: OSStatus -25299

Possible Solution


The problem is that MacOS default does NOT have openssl compiled within curl. Use homebrew to recompile curl with openssl support included:

brew install --with-openssl curl

Please note that this will not replace the default curl command of MacOS, you have to specifically point to the path of the newly installed version:

$ which curl
/usr/bin/curl

$ /usr/local/opt/curl/bin/curl --version
curl 7.55.1 (x86_64-apple-darwin16.7.0) libcurl/7.55.1 OpenSSL/1.0.2l zlib/1.2.8
Release-Date: 2017-08-14
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp smb smbs smtp smtps telnet tftp 
Features: AsynchDNS IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP UnixSockets HTTPS-proxy 

Add it to your path to use the new version by default:

export PATH="/usr/local/opt/curl/bin:$PATH"

HTTP 401
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Problem

    The handle server returns a JSON object that looks like this: {"responseCode":402,"handle":"myprefix/123456"}
    Handle Server responseCode 402 (Authentication needed)
    HTTP status code 401 (Unauthorized)

Possible solution 1

This error occurs if the username does not have admin permissions yet. Make sure it is referred to in a HS_ADMIN or HS_VLIST that has admin permissions.

Possible solution 2

This error also occurs if the username did not get permissions for this specific handle in its HS_ADMIN entry. Each user can only modify handles whose HS_ADMIN entry (or one of its HS_ADMIN entries) gives write permissions to him, either directly or by pointing to a HS_VLIST that has admin permissions and that contains the username.

Handshake Failure
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Problem

SSL routines:SSL3_READ_BYTES:ssl handshake failure

Possible Solution 1

This error can occur if the private key was encrypted. Please try with an unencrypted private key.

Possible Solution 2

Make sure that openssl version 1.0.1 or higher is used. Openssl 0.98 gives handshake errors.

SSL Error
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Problem

requests.exceptions.SSLError: [SSL] PEM lib (_ssl.c:2525)

Possible Solution

This error occurs if the private key was not provided, for example if a single file instead of two was provided, but the private key was not contained. For this reason, we only recommend and describe passing certificate and private key in two separate files.
SSL Error

Problem

SSLError: SSL3_GET_SERVER_CERTIFICATE:certificate verify failed

Possible Solution:

This error occurs if the server certificate at the handle server can not be verified at the client side. The library default is to verify the certificate. This is normally done with a certificate from a CA authority. The credentials file can have an optional parameter HTTPS_verify to change the behaviour. The problem can be solved in several ways. By adding the correct CA certificate to the bundle on the system. By setting a path to the correct CA certificate as follows: "HTTPS_verify": "/path_to_ca_certificate/ca_certificate". Or by disabling the checking of the certificate: "HTTPS_verify": "False". The last option is the least desired option.


