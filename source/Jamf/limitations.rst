JAMF Pro Limitations
====================


Network Segments
----------------

- May use reported client IP instead of recon IP, which breaks Network Segment DP's under jamfcloud if clients are behind
	NAT or proxy.


Sites
-----

- Not fully isolated


DEP
---

- jamf binary tries to force user re-enrollment, breaking UAMDM by removing the profile installed via DEP.


