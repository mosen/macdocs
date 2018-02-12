AssetCache Service (Tethered 10.12 and 10.13 Content Caching)
=============================================================

.. contents::


Database
--------

The database is a CoreData store defined by managed object models at :file:`/usr/libexec/AssetCache/AssetCache.momd`,
which stores its persistent data at :file:`/Library/Server/Caching/Data/AssetInfo.db` (10.12) and
:file:`/Library/Application Support/Apple/AssetCache/Data/AssetInfo.db` (10.13).

Activation
----------

On 10.13, a flag file is used and will be present at :file:`/Library/Application Support/Apple/AssetCache/.activated`.

On 10.12 with Server.app, the activation information is stored in the path indicated by this command::

    $(getconf DARWIN_USER_CACHE_DIR)com.apple.AssetCacheLocatorService/diskCache.plist

Logging
-------

10.12 Sierra w/Server.app
^^^^^^^^^^^^^^^^^^^^^^^^^

Caching Logs
    /Library/Server/Caching/Logs/Debug.log

Service Start, Stop and Registration
    /Library/Server/Caching/Logs/Service.log

10.13 High Sierra
^^^^^^^^^^^^^^^^^

You can filter unified logs by the subsystem ``com.apple.AssetCache`` to receive messages about the content caching
service.

Metrics
-------

10.12 Sierra w/Server.app
^^^^^^^^^^^^^^^^^^^^^^^^^

Metrics are stored in an sqlite database at :file:`/Library/Server/Caching/Logs/Metrics.sqlite`.

10.13.0 - 10.13.3
^^^^^^^^^^^^^^^^^

Metrics are only logged to the Unified Logging System

10.13.4 -
^^^^^^^^^

Commands
--------




Network
-------

The following CDN hosts and domains are whitelisted by AssetCache:

- basejumper.apple.com:443
- basejumper.apple.com:80
- deimos3.apple.com:80
- deimos.apple.com:80
- audiocontentdownload.apple.com:443
- audiocontentdownload.apple.com:80
- swdownload.apple.com:443
- swdownload.apple.com:80
- swdist.apple.com:443
- swcdn.apple.com:80
- validation.isu.apple.com:80
- appldnld.apple.com:80
- oscdn.apple.com:80
- *.assets.itunes.com:443
- *.phobos.apple.com:80
- *.itunes.apple.com:80
- *.itunes.apple.com:443
- *.assets.itunes.com:80


Links
-----

- `macOS Server - About caching service <https://help.apple.com/serverapp/mac/5.3/#/apd74DDE89F-08D2-4E0A-A5CD-155E345EFB83>`_.
- `Content types supported by the Caching service in macOS Server <https://support.apple.com/en-au/HT204675>`_.
- `Configure advanced cache settings <https://help.apple.com/serverapp/mac/5.3/#/apd5E1AD52E-012B-4A41-8F21-8E9EDA56583A>`_.
