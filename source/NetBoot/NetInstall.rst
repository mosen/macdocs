NetInstall
==========



Structure of an .NBI
--------------------

- **/.IAPhysicalMedia** Contains a plist of *AppName* key pointing to the macOS Installer. (10.13 only??)
- **Packages/Extras**
    - `minstallconfig.xml`_ Automated installer options

- **/Install macOS <Version>.app/Contents/SharedSupport/**
    - **3rd party.pkg** 3rd party bundled pkg. I believe it needs to be a distribution style package AND signed.
      The package needs to be listed in the **InstallInfo.plist** of the installer.



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

minstallconfig.xml
------------------

Keys

- **InstallType** ``automated`` or
- **Language** 2 letter country code eg. ``en``.
- **Package** Path to the main installation package to install. Since 10.13 this can be InstallInfo.plist eg::

    /System/Installation/Packages/InstallInfo.plist

- **ShouldErase** Should automatically erase the target
- **Target** The mounted target disk to erase and install onto.

