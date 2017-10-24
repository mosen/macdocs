macOS Installer
===============

:path: BaseSystem.dmg:/System/Installation/CDIS/macOS Installer

Debugger
--------

- Loads ``.installer.debuggeroptions`` from the target mountpoint (assume this is the NetInstall.dmg root or install
    source root).
- Content of the file is a Property List with an **array** as the root object.
- Each item contains the key(s) ``Operation`` and/or ``CrashAtProgress``.
- ``Operation`` denotes an operation string. (might be eg. kOSIWelcomePaneIdentifier)
- ``CrashAtProgress`` denotes a double value.
- When the desired operation/progress is reached, an **OSIDebuggerToolException** will be raised.


OSInstaller framework
---------------------

- Will consult nvram var ``install-config`` for the location of the automation file. Default is minstallconfig.xml
- Expects a key ``IAEndDate`` in minstallconfig.xml, see also `Pikes Blog Post <https://pikeralpha.wordpress.com/2017/08/05/osinstall-mpkg-appears-to-be-missing-or-damaged/>`_.
- Generate using ``date -v -2H "+%Y-%m-%dT%H:%M:%SZ"``
