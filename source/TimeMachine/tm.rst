TimeMachine Notes
=================

Network Time Machine
--------------------

- A **.sparseimage** is created for each machine, named after that machine on the network share.
- The image is formated as HFS+J, Case sensitive, even on Mojave.
- A tree output of the volume is below::

			/Volumes/Time\ Machine\ Backups
			└── Backups.backupdb
				└── admin�\200\231s\ Mac\ mini
					├── 2018-07-28-011314
					│   └── Macintosh\ HD
					└── Latest -> 2018-07-28-011314

- The disk image contains::

	.fseventsd
		fseventsd-uuid
	.Spotlight-V100
		... spotlight db ...
	Backups.backupdb

The backupdb folder contains:

- **.RecoverySets** this folder contains what seems to be one Recovery OS image per directory, starting with the number
	zero. The directory contains a folder named ``com.apple.recovery.boot`` which seems to have the APFS Preboot Volume
	contents.

	The structure of this is below::

		/Volumes/Time\ Machine\ Backups/Backups.backupdb/.RecoverySets/0/com.apple.recovery.boot
		├── BaseSystem.chunklist
		├── BaseSystem.dmg
		├── PlatformSupport.plist
		├── SystemVersion.plist
		├── boot.efi
		├── boot.efi.j137ap.im4m
		├── com.apple.Boot.plist
		├── immutablekernel
		├── immutablekernel.j137ap.im4m
		└── prelinkedkernel


- **.spotlight_repair** and **.spotlight_temp** which were empty on a clean system.
- **ComputerName** The directory containing the main backup, which contains one backup per timestamp, and a folder called
	"Latest", linked to the latest backup.

Each backup "timestamped" folder eg (2018-07-28-011314) contains:

- **.<UUID>.clonedb** A binary plist containing an array of array of integers?
- **.Backup.log** A log of some of the backup operation, in plain text.
- **.com.apple.TMCheckpoint** A property list containing information about the backup progress (probably in the event of
a resume)::

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>SnapshotNumberOfBytesCopied</key>
		<integer>8323784900</integer>
		<key>SnapshotNumberOfBytesToCopy</key>
		<integer>9583538176</integer>
		<key>SnapshotNumberOfBytesToRepresent</key>
		<integer>9583538176</integer>
		<key>SnapshotNumberOfItemsCopied</key>
		<integer>420390</integer>
		<key>SnapshotNumberOfItemsToCopy</key>
		<integer>457037</integer>
		<key>SnapshotNumberOfRollingPreviousBytesCopied</key>
		<integer>0</integer>
	</dict>
	</plist>

- **.exclusions.plist** A binary plist containing the time machine exclusions, which includes user based exclusions and
default system exclusions. The pseudo plist output is this::

	{
		apiExclusionPaths =     (
		);
		sourcePaths =     (
			"/"
		);
		standardExclusionPaths =     (
	",      "/.HFS+ Private Directory Data
			"/.MobileBackups",
			"/.MobileBackups.trash",
			"/.Spotlight-V100",
			"/.TemporaryItems",
			"/.Trashes",
			"/.com.apple.backupd.mvlist.plist",
			"/.fseventsd",
			"/.hotfiles.btree",
			"/.vol",
			"/Backups.backupdb",
			"/Desktop DB",
			"/Desktop DF",
			"/Library/Caches",
			"/Library/Logs",
			"/Library/Updates",
			"/MobileBackups.trash",
			"/Network",
			"/System/Library/Caches",
			"/System/Library/Extensions/Caches",
			"/Users/Guest",
			"/Users/Shared/SC Info",
			"/Volumes",
			"/automount",
			"/cores",
			"/dev",
			"/home",
			"/net",
			"/private/Network",
			"/private/etc/kcpassword",
			"/private/tftpboot",
			"/private/tmp",
			"/private/var/automount",
			"/private/var/db/Spotlight",
			"/private/var/db/Spotlight-V100",
			"/private/var/db/com.apple.backupd.backupVerification",
			"/private/var/db/dhcpclient",
			"/private/var/db/dyld",
			"/private/var/db/dyld/shared_region_roots",
			"/private/var/db/efw_cache",
			"/private/var/db/fseventsd",
			"/private/var/db/systemstats",
			"/private/var/folders",
			"/private/var/lib/postfix/greylist.db",
			"/private/var/log",
			"/private/var/run",
			"/private/var/spool/cups",
			"/private/var/spool/fax",
			"/private/var/spool/uucp",
			"/private/var/tmp",
			"/private/var/vm"
		);
		stickyExclusionPaths =     (
			"/Users/admin/Library/Calendars/Calendar Cache",
			"/Users/Shared/adi",
			"/Users/admin/Library/Assistant/SiriAnalytics.db",
			"/Users/admin/Library/LanguageModeling/en-dynamic.lm",
			"/Users/admin/Library/PersonalizationPortrait",
			"/Library/Application Support/Apple/AssetCache/Data"
		);
		systemFilesExcluded = 0;
		userExclusionPaths =     (
		);
	}

- **One folder per mounted volume that is backed up, eg. Macintosh HD**

Time Machine Processes
----------------------

backupd - /System/Library/CoreServices/backupd.bundle/Contents/Resources/backupd
TimeMachine.framework - /System/Library/PrivateFrameworks/TimeMachine.framework/Versions/A/TimeMachine


