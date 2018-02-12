DEP Provisional Enrollment
==========================

DEP Provisional enrollment is used by Apple Configurator 2 to enroll already purchased devices into the DEP program.

Sequence
--------

1. Apple Configurator 2 logs into the device enrollment service

.. http:get:: https://devicenrollment.apple.com/v2/login

    **Request**:

    .. sourcecode:: http

        GET /v2/login HTTP/1.1
        Host: deviceenrollment.apple.com
        X-Apple-I-MD-RINFO:	(int 8)
        X-Apple-I-MD-LU:	(64 hex)
        Accept:	*/*
        Connection:	keep-alive
        X-Apple-I-Locale:	en_AU
        Accept-Language:	en-au
        Accept-Encoding:	br, gzip, deflate
        X-MMe-Client-Info:	<iMac13,1> <Mac OS X;10.13.3;17D47> <com.apple.AuthKit/1 (com.apple.configurator.ui/379)>
        X-Apple-I-MD-M:	(80 char encoded)
        X-Profile-Protocol-Version: 1
        User-Agent: Configurator/2.6.1
        X-Apple-I-Client-Time: 2018-01-30T02:00:38Z
        X-Apple-GS-Token: (Auth token)
        X-Mme-Device-Id: (Platform UUID of Apple Configurator Machine)
        X-ADM-Idms-Service-Id: com.apple.gs.configurator.auth
        X-Apple-I-MD:	Base64ABCDEF==
        X-Apple-I-TimeZone:	AEDT

    :reqheader X-Apple-I-MD-RINFO:
    :reqheader X-Apple-I-MD-LU:
    :reqheader X-Apple-I-Locale: The locale of the requesting machine eg. ``en_US``
    :reqheader X-MMe-Client-Info: <Hardware Model> <OS family;OS version;OS build> <User-Agent>
    :reqheader X-Apple-I-MD-M:
    :reqheader X-Profile-Protocol-Version: Always ``1`` for most DEP things.
    :reqheader X-Apple-I-Client-Time: ISO8601 time at the AC2 workstation
    :reqheader X-Apple-GS-Token: Auth token (unknown purpose)
    :reqheader X-Mme-Device-Id: Platform UUID of the Apple Configurator 2 Machine.
    :reqheader X-ADM-Idms-Service-Id: For AC2 this is ``com.apple.gs.configurator.auth``.
    :reqheader X-Apple-I-MD: Base64 encoded something?
    :reqheader X-Apple-I-TimeZone: Timezone eg.. ``AEST`` ``PDT``

    **Response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        X-Apple-Request-UUID: AED090FF-CC4F-47E3-B3D3-9F422394B1C3
        X-Apple-Jingle-Correlation-Key: (26 chars)
        apple-seq: 0
        apple-tk: false
        Apple-Originating-System: UnknownOriginatingSystem
        Strict-Transport-Security: max-age=15768000; includeSubdomains
        content-type: text/plain; charset=UTF-8
        X-Content-Type-Options: nosniff
        X-XSS-Protection: 1; mode=block
        content-encoding: gzip
        content-length: 656
        Set-Cookie	ABCDEF;path=/;secure;httponly

        (... the authentication token to use in subsequent requests ...)

    :resheader X-Apple-Request-UUID: UUID of the request
    :resheader X-Apple-Jingle-Correlation-Key: ?
    :resheader apple-seq: 0 (assume a sequence number)
    :resheader apple-tk: boolean, usually ``false``
    :resheader Apple-Originating-System:

2. Apple Configurator 2 Retrieves the device Nonce

.. http:post:: https://devicenrollment.apple.com/v2/nonce

    **Request**:

    .. sourcecode:: http

        POST /v2/nonce HTTP/1.1
        Host: deviceenrollment.apple.com
        Content-Type: application/json
        Accept-Encoding: br, gzip, deflate
        Connection: keep-alive
        Accept: */*
        X-ADM-Auth-Session:	(auth token from login)
        User-Agent: Configurator/2.6.1
        X-Profile-Protocol-Version: 1
        Content-Length: 7924
        Accept-Language: en-au

        {
            "profile": {
                "supervising_host_certs": [ "array of base64 encoded supervision certs" ],
                "is_supervised": true,
                "allow_pairing": true,
                "anchor_certs": ["array of base64 anchor certs"],
                "url": "dep enrollment url for current MDM",
                "profile_name": "Configurator Generated Profile",
                "skip_setup_items": ["Location", "Restore", "Android", "AppleID", "TOS", "Siri", "Diagnostics", "Passcode", "Biometric", "Payment", "Zoom", "DisplayTone", "MessagingActivationUsingPhoneNumber", "HomeButtonSensitivity", "CloudStorage", "ScreenSaver", "TapToSetup", "WatchMigration", "OnBoarding", "TVProviderSignIn", "TVHomeScreenSync"],
                "is_mdm_removable": false,
                "is_mandatory": true,
                "support_email_address": "organisational.email@example.com",
                "is_multi_user": false,
                "org_magic": "UUID"
            },
            "server_name": "Devices Added by Apple Configurator 2",
            "devices": [{
                "serial_number": "SN of ipad",
                "udid": "UDID of ipad"
            }]
        }

    :reqheader X-ADM-Auth-Session: This header is provided to authenticate each request, it is retrieved from the /v2/login endpoint.

    :<json object profile: the json object containing the initial DEP profile (sometimes called pre-stage profile).
    :<json string server_name: the name of the MDM server to add this device to, default ``Devices Added by Apple Configurator 2``.
    :<json array devices: Array of devices to add

    **Response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        date: Mon, 01 Jan 1824 02:00:40 GMT
        X-Apple-Request-UUID: (Request UUID)
        X-Apple-Jingle-Correlation-Key: (26 chars)
        apple-seq: 0
        apple-tk: false
        Apple-Originating-System: UnknownOriginatingSystem
        Strict-Transport-Security: max-age=15768000; includeSubdomains
        content-type: application/json; charset=UTF-8
        X-Content-Type-Options: nosniff
        X-XSS-Protection: 1; mode=block
        content-encoding: gzip
        content-length: 274
        Set-Cookie	cookievalue;path=/;secure;httponly
        Connection	Keep-alive

        {
	        "nonces": [{
		        "status": "SUCCESS",
		        "value": "NONCE VALUE",
		        "serialNumber": "ios-device-serial",
		        "udid": "ios-device-udid"
	        }]
        }

3. ocsp attempts to verify the validity of certificates and contacts **ocsp.apple.com**.
4. from this point on, the iPad will communicate with apple.

