

Troubleshooting
---------------

You can run :manpage:`AssetCacheLocatorUtil(8)` to query apple directory and get information about located caching
servers.


Locating the cache
------------------

- If you have multiple public IP addresses, you should configure a DNS TXT record as described in
    `Enable caching service discovery across multiple public IP addresses <https://help.apple.com/serverapp/mac/5.3/#/apd6015d9573>`_.


The AssetCacheLocatorService stores its cache at::

    defaults read $(getconf DARWIN_USER_CACHE_DIR)com.apple.AssetCacheLocatorService/diskCache.plist



Preferences
-----------

**com.apple.AssetCacheLocatorService**


AffinityQueryTimeout
    number

ConcurrentDNSResolutions
    integer

LocateTimeout
    double

DNSResolutionTimeout
    double

RefreshValidityInterval
    double

LocateURL
    string, default is https://lcdn-locator.apple.com/lcdn/locate

RedactLogs
    bool

SkipEVCheck
    bool

SkipLocalhostCheck
    bool

