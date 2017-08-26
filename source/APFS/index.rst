########################
APFS (Apple File System)
########################

Official Documentation
----------------------

Man Pages
^^^^^^^^^

* :manpage:`APFSUserAgent(8)` - APFS new container observer.
* :manpage:`apfs.util(8)` - APFS file system utility.
* :manpage:`apfs_hfs_convert(8)`
* :manpage:`fsck_apfs(8)` - APFS consistency check.
* :manpage:`mount_apfs(8)` - mount an APFS volume.
* :manpage:`newfs_apfs(8)` - construct a new APFS volume.

Guides
^^^^^^

* `Apple File System Guide <https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html>`_.

Command Examples
----------------

Quick listing of container / volume hierarchy::

    $ diskutil ap list

Remove entire container from Physical Store::

    $ diskutil ap deleteContainer [reference device or name]

.. note:: Interestingly, the disk reverts to an HFS volume called Untitled.

Creating a new container::

    $ diskutil ap createContainer disk1s2

.. note:: This needs to be a physical partition in the case of a disk with an EFI System Partition, hence disk1s2


Space Sharing Notes
===================

The APFS Guide mentions Quotas and Reservations

.. epigraph::

    Containers can optionally configure a quota, or a maximum allotment of disk space for a volume, as well as a reservation, or a guaranteed minimum allotment of disk space for a volume.

    -- Apple File System Guide, Space Sharing.

Disk Utility GUI
----------------

- When adding a new APFS volume, you have a *Size Options* button with which to configure the quota or reservation.


CLI
---

:command:`diskutil apfs addVolume` takes the flags ``-reserve`` and ``-quota``.


Snapshots
=========

snapshots can currently be manipulated using :manpage:`tmutil(8)`.

you cannot rollback a snapshot with any existing tool (?) - Verify

Examples
--------

Create a snapshot of all APFS volumes that are not listed as exclusions::

    $ tmutil localsnapshot

    Created local snapshot with date: YYYY-MM-DD-HHMMSS

List snapshots of an APFS volume by mountpoint::

    $ tmutil listlocalsnapshots /

    com.apple.TimeMachine.YYYY-MM-DD-HHMMSS

Delete a snapshot::

    $ tmutil deletelocalsnapshots 2017-08-27-004037

    Deleted local snapshot '2017-08-27-004037'

