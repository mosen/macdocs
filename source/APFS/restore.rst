Restoring an APFS DMG
=====================

GUI (Disk Utility)
------------------

beta 7

- Context (right) click container disk
- Choose Restore..., select image.
- apfs dmg is restored
- volume count shows correctly but volumes are not mounted and do not appear until you restart disk utility.

- AutoDMG OR this process has created a VM container that does not exist in a clean install from the install media.

- I did not see any startup disk options after this, but after reboot they appeared.

- Failed to bless the newly restored volume when selected in Startup Disk

asr
---

Restore .dmg to container from booted system::

    Example APFS container

    /dev/disk2 (synthesized):
       #:                       TYPE NAME                    SIZE       IDENTIFIER
       0:      APFS Container Scheme -                      +42.7 GB    disk2
                                     Physical Store disk1s2


    $ sudo asr restore --erase --source /Users/vagrant/Documents/osx-10.13-17A344b.apfs.dmg --target /dev/disk2

* target volumes will not be automatically mounted
* target volume must be mounted before **Startup Disk** will show the choice.
* attempting to restore to a disk that contains HFS fails, even with erase flag.
* createContainer may be used to stomp on a HFS volume.
* if you attempt to :command:`asr` a running system you will receive::

    Source volume is read-write and cannot be unmounted, so it can't be block copied.



Block restore APFS dmg volume to existing APFS volume w/Invert
--------------------------------------------------------------

need sourcevolumename

Gotchas
-------

* Restoring a single APFS volume from dmg MIGHT replace your Preboot and Recovery volumes even though your target was
    a new volume.

