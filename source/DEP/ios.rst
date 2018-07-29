iOS Provisional DEP
===================

- Captive portal detection is done by the **CaptiveNetworkSupport-xxx.xx.x** package by attempting to fetch the URL
    `<http://captive.apple.com/hotspot-detect.html>`_.

- iPad may fetch KeyBag from http://init-p01st.push.apple.com/bag which is public (can be curled).
    This server is called **APNS Bagger**

- iPad may fetch KeyBag from http://init-p01md.apple.com/bag
    This server is called **Madrid Bagger**

- 2 more bags

- OCSP checks all the certs

- GET http://static.ess.apple.com/identity/validation/cert-1.0.plist


REBOOT
------

- Hotspot check
- Keybags
- OCSP

LEAVE
-----

