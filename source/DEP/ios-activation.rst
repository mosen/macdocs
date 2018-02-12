iOS Activation
==============

1. drmHandshake
---------------

.. http:post:: https://albert.apple.com/deviceservices/drmHandshake

    **Request**:

    .. sourcecode:: http

        POST /deviceservices/drmHandshake HTTP/1.1
        Host: albert.apple.com
        Content-Type: application/xml
        Connection: keep-alive
        Accept: application/xml
        User-Agent: iOS Device Activator (MobileActivation-286.30.34 built on Nov  2 2017 at 22:43:42)
        Content-Length: 27287
        Accept-Language:
        Accept-Encoding: br, gzip, deflate

        <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
        <plist version="1.0">
            <dict>
                <key>CollectionBlob</key>
                <data>(... very long blob ...)</data>
                <key>HandshakeRequestMessage</key>
                <data>(... 28 char data ...)</data>
                <key>UniqueDeviceID</key>
                <string>(device UDID)</string>
            </dict>
        </plist>


    **Response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Cache-Control: private, no-cache, no-store, must-revalidate, max-age=0
        Content-Type: application/xml
        Content-Length: 1483
        Date: Wed, 31 Jan 2018 03:35:05 GMT
        Connection: Keep-alive

        <plist version="1.0">
            <dict>
                <key>FDRBlob</key>
                <data>(... data ...)</data>
                <key>SUInfo</key>
                <data>(... data ...)</data>
                <key>HandshakeResponseMessage</key>
                <data>(... data ...)</data>
                <key>serverKP</key>
                <data>(... data ...)</data>
            </dict>
        </plist>

2. deviceActivation
-------------------

The result of this query is referred to as the activation record for the device.

See Also:

- `Activation Token <https://www.theiphonewiki.com/wiki/Activation_Token>`_.


.. http:post:: https://albert.apple.com/deviceservices/deviceActivation

    **Request**:

    .. sourcecode:: http

        POST /deviceservices/deviceActivation HTTP/1.1
        Host: albert.apple.com
        Content-Type: application/x-www-form-urlencoded
        Connection: keep-alive
        Accept: */*
        User-Agent: iOS Device Activator (MobileActivation-286.30.34 built on Nov  2 2017 at 22:43:42)
        Content-Length: 12621
        Accept-Language:
        Accept-Encoding: br, gzip, deflate

        activation-info	=

        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
        <plist version="1.0">
        <dict>
            <key>ActivationInfoXML</key>
            <data>(... blob ...)</data>
            <key>FairPlayCertChain</key>
            <data>(... blob ...)</data>
            <key>FairPlaySignature</key>
            <data>(... blob ...)</data>
            <key>RKCertification</key>
            <data>(... blob ...)</data>
            <key>RKSignature</key>
            <data>(... blob ...)</data>
            <key>serverKP</key>
            <data>(... blob ...)</data>
            <key>signActRequest</key>
            <data>(... blob ...)</data>
        </dict>
        </plist>

    **Response**:

    .. sourcecode:: http

        HTTP/1.1 200
        ARS: (base64)
        Cache-Control: private, no-cache, no-store, must-revalidate, max-age=0
        Content-Type: text/xml
        Content-Length: 8819
        Date: Wed, 31 Jan 2018 03:35:08 GMT
        Content-Encoding: gzip
        Content-Length: 5766
        Connection: Keep-alive

        <plist version="1.0">
            <dict>
                <key>ActivationRecord</key>
                <dict>
                    <key>AccountTokenCertificate</key>
                    <data>(... blob ...)</data>
                    <key>DeviceCertificate</key>
                    <data>(... blob ...)</data>
                    <key>ack-received</key>
                    <true />
                    <key>FairPlayKeyData</key>
                    <data>(... blob ...)</data>
                    <key>AccountToken</key>
                    <data>(... blob ...)</data>
                    <key>AccountTokenSignature</key>
                    <data>(... blob ...)</data>
                    <key>UniqueDeviceCertificate</key>
                    <data>(... blob ...)</data>
                    <key>show-settings</key>
                    <true />
                    <key>DeviceConfigurationFlags</key>
                    <string>0</string>
                </dict>
            </dict>
        </plist>
