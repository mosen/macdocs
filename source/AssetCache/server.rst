AssetCache Service
==================


Database
--------

The database is a CoreData store defined by managed object models at :file:`/usr/libexec/AssetCache.momd`, which stores
its persistent data at :file:`/Library/Server/Caching/Data/AssetInfo.db`.

Logging
-------



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
