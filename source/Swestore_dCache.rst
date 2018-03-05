Swestore dCache
================

Swestore is distributed across the SNIC centres `C3SE <http://www.c3se.chalmers.se>`_, `HPC2N <https://www.hpc2n.umu.se/>`_, `Lunarc <http://www.lunarc.lu.se/>`_, `NSC <https://www.nsc.liu.se/>`_ and `Uppmax <http://www.uppmax.uu.se/>`_. 

Data is stored in two copies with each copy at a different SNIC centre. This enables the system to cope with a multitude of issues ranging from a simple crash of a storage element to losing an entire site while still providing access to the stored data.

One of the major advantages to the distributed nature of Swestore is the excellent aggregated transfer rates possible. This is achieved by bypassing a central node and having transfers going directly to/from the storage elements if the selected transfer protocol allows it. Swestore can achieve aggregated transfer rates in excess of 100 Gigabit per second, but in practice this is limited by connectivity to the end user, each university or a limited number of files (typically max 1 Gbit/s per file/connection).

To protect against silent data corruption the dCache storage system checksums all stored data and periodically verifies the data using this checksum.

The dCache system does NOT yet provide protection against user errors like inadvertent file deletions.

.. toctree::
   :maxdepth: 1

   Getting_access_to_Swestore_dCache
   Using_Swestore_dCache
