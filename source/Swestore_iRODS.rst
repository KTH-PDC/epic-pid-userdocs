Swestore iRODS
=================

Swestore iRODS is distributed across two SNIC centres `NSC <https://www.nsc.liu.se/>`_ and `PDC <https://www.pdc.kth.se/>`_.

Data is stored in two copies with each copy at a different SNIC centre. This enables the system to cope with a multitude of issues ranging from a simple crash of a storage element to losing an entire site while still providing access to the stored data.

To protect against silent data corruption the iRODS storage system checksums all stored data and periodically verifies the data using this checksum.

.. toctree::
   :maxdepth: 1

   Using_Swestore_iRODS

