

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

Frameworks
----------

These frameworks are linked to :manpage:`AssetCacheLocatorUtil(8)`

- /System/Library/PrivateFrameworks/AssetCacheServices.framework
- /System/Library/PrivateFrameworks/AssetCacheServicesExtensions.framework

AssetCacheServices.framework
----------------------------

_ACSLocateCachingServer

Log output in this procedure uses the prefix "ACSLocateCachingServer".

calls -> _aclLocateCommon @ 0x1632

XPC Dictionary::

    {
        "tag": (uint64) # Possibly the randomly generated number earlier
        "command": "locate"
        "quantity": (int64) # Possibly to indicate the number of desired results since ACSLocate can choose all or one caching server.
        "capabilities": {
            "import": (bool),
            "namespaces": (bool),
            "personalCaching": (bool),
            "queryParameters": ??? # Unsure if this is inside this dict
        },
        "x-apple-persistent-identifier": ?
        "autoRefresh": (bool),
        "quickMiss": (bool),
        "forceMiss": (bool),
        "sizeHint": (uint64),
        "routing": ... from _aclGetRoutingOption, either "system" or "user"
    }

async dispatch to connection from _aclGetSharedLocatorConnection

mach service **com.apple.AssetCacheLocatorService**

This is likely to be :path:`/System/Library/PrivateFrameworks/AssetCacheServices.framework/Versions/A/XPCServices/AssetCacheLocatorService.xpc`


