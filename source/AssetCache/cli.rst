AssetCache Command Line Interface
=================================

High Sierra 10.13
-----------------

Content caching can be controlled using the :manpage:`AssetCacheManagerUtil(8)` command.

Preferences are located at :file:`/Library/Preferences/com.apple.AssetCache.plist`

Restrictions
------------

- You cannot activate content caching inside a virtual machine, you will receive the message::

    AssetCacheManagerUtil[1200:114934] Failed to activate built-in caching server: Error Domain=ACSMErrorDomain Code=5 "virtual machine" UserInfo={NSLocalizedDescription=virtual machine}

This restriction is implemented by reading the hypervisor bit from the CPU via `sysctl`.

Sierra 10.12
------------

The Caching Service is available as part of ``Server.app``.

Settings may be altered using the :command:`serveradmin settings caching` command, or through the property list located
at `/Library/Server/Caching/Config/config.plist`.

Flushing the Cache
==================

You may flush the cache using `sudo serveradmin command caching:command = flushCache`
