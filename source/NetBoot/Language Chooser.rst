Language Chooser
================

**Language chooser** is invoked in the installation environment to pick a system language and pass control over to
macOS Installer.

path
    /System/Library/CoreServices/Language Chooser.app


Kernel Arguments
----------------

- **debugshell=?** May allow a shell to hang around after installer exits.

Arguments
---------

- Position 1: Target application to launch, usually ``macOS Installer``.
- **-f <minstall_conf>**: Path to minstallconf.xml for installer automation.
- **-AppleLanguages <lang code(s)>**: Language code passed to installer if this is an automated run.
- **-ExternalLog YES**
- **-NSDisabledCharacterPaletteMenuItem YES**

ENV
---

:__OSINSTALL_ENVIRONMENT: Running in basesystem environment
:__FAKE_OSINSTALL_ENVIRONMENT: Faking BaseSystem

NVRAM
-----

Used in installer automation:

:install-product-url:
:install-product:
:install-config:



Notes
-----

0x1c0000 modifierFlags

bit 18,19,20
ctrl opt cmd
cant be zero length

decimalDigitCharacterSet
timeChars

07:34 UTC

__FAKE_OSINSTALL_ENVIRONMENT=1

Boot Mode
---------

Boot mode seems to come with 3 options:

- ``guest``: determined via env ``__OSINSTALL_GUEST_MODE``.
- ``locked``: from nvram ``recovery-boot-mode``.
- ``unused``


