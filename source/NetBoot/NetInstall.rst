NetInstall
==========

All of this information has been gathered from 10.13 High Sierra thus far.


Structure of an .NBI
--------------------

- **/.IAPhysicalMedia** Contains a plist of *AppName* key pointing to the macOS Installer. (10.13 only??)
- **Packages/Extras**
    - `minstallconfig.xml`_ Automated installer options

- **/Install macOS <Version>.app/Contents/SharedSupport/**
    - **3rd party.pkg** 3rd party bundled pkg. I believe it needs to be a distribution style package AND signed.
      The package needs to be listed in the **InstallInfo.plist** of the installer.

Boot Environment
----------------

- /dev/disk9 on /System/Installation.

- :file:`/System/Installation` contains a single symbolic link item called **PackagesLink**.
- :file:`/System/Installation/PackagesLink` points to :file:`/Volumes/Image Volume/Packages`.
- :file:`/Volumes/Image Volume` is the root of the **NetInstall** media, therefore anything normally under
    :file:`/System/Installation/Packages` in macOS Sierra can be made available at the root of the NetInstall.dmg.

How SIU Adds Configuration Profiles
-----------------------------------

- SIU adds an **Additional Package** item called ``netInstallConfigurationProfiles.sh.pkg``, a distribution style package
  which in turn contains a package called ``netInstallConfigurationProfiles.sh.inner.pkg``. This package contains a
  postinstall script.

- The postinstall script simply runs ``/System/Installation/Packages/Extras/postinstall/installConfigurationProfiles.sh``
  with the path to the target installation directory (at the time this would be the install destination).

.. note:: You can abuse the **Additional Packages** section to run items out of the S/I/P/Extras directory.

The install script interacts directly with ``/var/db/ConfigurationProfiles``.

- It creates ``/var/db/ConfigurationProfiles/Setup``. ``root:wheel 755``.
- It touches ``/var/db/ConfigurationProfiles/.profilesAreInstalled``.
- It copies all of our source profiles from the NetInstall dmg at
  ``/System/Installation/Packages/Extras/ConfigurationProfiles`` to the Setup directory.
- ``/var/db/ConfigurationProfiles/Setup/.profileSetupDone`` is removed.
- ``/var/db/ConfigurationProfiles/Setup/.profileSetupRetryFailedProfiles`` is added (touched).
- MCX debugging is enabled

It is interesting to note that the output of this script is scraped by installer to get information about progress.
The messages are colon delimited eg. ``netrestore:finish``.

How SIU Provides Additional Packages
------------------------------------

Within the operating systems ``/Install macOS <Version>.app/Contents/SharedSupport/InstallInfo.plist`` are several optional keys that can be populated to provide installation
of additional packages.

- **Additional Installers** is an array of strings containing names of packages. The names are usually assumed to be
relative to the ``InstallInfo.plist``.
- **Additional Wrapped Installers** TBD.


How SIU Provides Post-Installation Scripts
------------------------------------------

- Each script is converted into a payload free package similar to the one that executes the Configuration Profiles
  installer.
- If you provide a script called **post_install.sh** it will be wrapped in a .pkg, and then in a distribution pkg eg.
  You will end up with **post_install.sh.pkg** which is a distribution style package containing a package called
  **post_install.sh.inner.pkg**.
- The contents of your script will be executed as the **postinstall** script.
- The package bundle identifier is ``com.apple.SystemImageUtility.<Script name with extension>``.

How SIU Binds Clients to OD/AD
------------------------------

- A payload free package, ``netInstallApplyConfigurationSettings.sh.pkg`` is created, and added to the
  **Additional Installers** key.
- This package begins by executing ``installClientHelper.sh`` which does the rest of the workflow.
- This script copies a few different files to allow the machine to bind:
    - NetBootClientHelper: The utility which helps manage the binding (this also manages host naming).
    - bindingNames.plist: The configuration, copied to /etc which contains the domains to bind.
    - ``com.apple.NetBootClientHelper.plist``: The launchdaemon responsible for starting ``NetBootClientHelper``

minstallconfig.xml
------------------

Keys

- **InstallType** ``automated`` or
- **Language** 2 letter country code eg. ``en``.
- **Package** Path to the main installation package to install. Since 10.13 this can be InstallInfo.plist eg::

    /System/Installation/Packages/InstallInfo.plist

- **ShouldErase** Should automatically erase the target
- **Target** The mounted target disk to erase and install onto.
- **displayCountdown** Number of seconds to display countdown

Package Only Installation
-------------------------

Structural Differences
^^^^^^^^^^^^^^^^^^^^^^

A package only NetInstall image contains a completely different structure as it does not require the use of the
Installer or the InstallESD.dmg content.

- :file:`/BaseSystem.dmg` + :file:`/BaseSystem.chunklist`: The Base Operating System Environment and its chunklist.
- :file:`/Packages`
    - :file:`ASRInstall.pkg`: In a package-only installation, this seems to contain no payloads and no scripts.
    - :file:`InstallPreferences.plist`: Contains a single key **packageOnlyMode** which is **TRUE**.
    - :file:`{packageName}.pkg`: The package(s) to install.
    - :file:`System.dmg`: A DMG which is currently zero bytes.
    - :file:`/Extras`
        - :file:`install.packagePath`: A file with a single line pointing to :file:`/System/Installation/Packages/ASRInstall.pkg`
        - :file:`postRestorePackages.txt`: A file listing package(s), one per line, eg. :file:`/System/Installation/Packages/munkitools-3.1.0.3398.pkg`.
        - :file:`postInstallPackages.sh`: A script which reads package(s) from :file:`postRestorePackages.txt`, one per line
        and installs them using :command:`installer -pkg`.
        - :file:`z_preserveInstallLog.sh`: A script which copies the installer log :file:`/var/log/install.log` from the NetBoot
        environment back into the target volume in the same location.

Process
^^^^^^^

- :file:`/etc/rc.install` checks for the existence of :file:`/System/Installation/Packages/Extras/install.packagePath`
  which typically exists in a NetRestore Package Only environment.
- Instead of running the installer, the NetRestore application :file:`/System/Installation/CDIS/NetRestore.app/Contents/MacOS/NetRestore`
  is run.
- Depending on whether the ``packageOnlyMode`` key is true in :file:`InstallPreferences.plist`, the NetRestore application
  proceeds with an **InstallerOperation**. If it is false, it proceeds with the **ASROperation**.
- **NetRestore.app** considers any executable in :file:`/Packages/Extras/postinstall`, sorted by file name, to be executed.

.. note:: NetRestore may only consider shell scripts because it does check the extension.


