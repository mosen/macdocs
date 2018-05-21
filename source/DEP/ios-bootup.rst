iOS Boot Network Sequence
=========================

Prior to this, if a PAC file is discovered through DHCP WPAD, it will be downloaded.

1. Captive Portal Detection
---------------------------

*ipad-dep-bootup-session3.pcapng, sequence 224*

.. http:get:: http://captive.apple.com/hotspot-detect.html

    **Request**:

    .. sourcecode:: http

        GET /hotspot-detect.html
        Host: captive.apple.com
        Connection: close
        User-Agent: CaptiveNetworkSupport-355.30.1 wispr

    **Response**:

    .. sourcecode:: http

        HTTP/1.0 200 OK
        x-amz-id-2: (base64 encoded)
        x-amz-request-id: (16 chars hex)
        Date: Sun, 11 Feb 2018 10:49:46 GMT
        Last-Modified: Fri, 17 Feb 2017 20:36:28 GMT
        Cache-Control: max-age=300
        Accept-Ranges: bytes
        Content-Type: text/html
        Content-Length: 69
        Server: ATS/7.1.3
        Via: http/1.1 (local region)-edge-lx-004.ts.apple.com (ApacheTrafficServer/7.1.3), http/1.1 (local region)-edge-bx-010.ts.apple.com (ApacheTrafficServer/7.1.3)
        CDNUUID: (uuid)
        X-Cache: hit-fresh, hit-fresh
        Etag: "etag contents"
        Age: 174

        <HTML><HEAD><TITLE>Success</TITLE></HEAD><BODY>Success</BODY></HTML>\n


2. NTP Query to **time-ios.apple.com** (*CNAME time-ios.g.applimg.com*)
-----------------------------------------------------------------------

*ipad-dep-bootup-session3.pcapng, sequence 252*


gsa.apple.com *ipad-dep-bootup-session3.pcapng, sequence 264*

configuration.apple.com *ipad-dep-bootup-session3.pcapng, sequence 266*

basejumper.apple.com *ipad-dep-bootup-session3.pcapng, sequence 278*
    returned 503

mesu.apple.com *ipad-dep-bootup-session3.pcapng, sequence 309*

3. Initialise Apple Push Service Identity
-----------------------------------------

I assume at this point that these bags are to establish the initial trust chain for APNS.

These bags are fetched in no particular order, they will show up in random order for any given packet capture.

**APNS Bagger**

.. http:get:: http://init-p01st.push.apple.com/bag

    **Request**:

    .. sourcecode:: http

        GET /bag HTTP/1.1
        Host: init-p01st.push.apple.com
        Accept: */*
        Accept-Language: en-us
        Connection: keep-alive
        Accept-Encoding: gzip, deflate
        User-Agent: iPad4,1/11.2 (15C5111a)

    **Response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/x-apple-plist
        Last-Modified: Mon, 05 Feb 2018 20:15:19 GMT
        Content-Length: 7939
        Server: APNS Bagger
        Cache-Control: public, max-age=1198
        Date: Sun, 11 Feb 2018 10:52:41 GMT
        Connection: keep-alive

        <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
        <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
        <plist version="1.0">
         <dict>
          <key>signature</key>
          <data>HP60tbDTukzWBuZDUMEg1vNx4D+E3cm1b5HBc617QaRQEOpe/20DuYZ7DoJcQ0LYLVJ1K1iC8pzlF1z+INtm0G4gEcC459A3sZW5/+XtJT5sFh1lenQA4xvdHWjwGlDb81QwAEevGitE4N09kj7IBheoMwnfg3xAuzk/MTZXQc4RunDmOKNNn0cEZ2+1KNVmEYWsS58csYrNUiNv4piRzpC5kUetZuSm4/QamqZVUpbgYXINoobH68Jm6kkMNe1QX8D0P6LUmK0EcjafXIuNrftTJlxWrWgM9paq/Z0Cd8q68J3OwhMO5tKXmDQPigwGQLpcV+e08hsgHTkyg8WxwQ==</data>

          <key>certs</key>
          <array>
           <data>MIIFRjCCBC6gAwIBAgIQXgujPtScX2cAAAAAUN5PkzANBgkqhkiG9w0BAQsFADCBujELMAkGA1UEBhMCVVMxFjAUBgNVBAoTDUVudHJ1c3QsIEluYy4xKDAmBgNVBAsTH1NlZSB3d3cuZW50cnVzdC5uZXQvbGVnYWwtdGVybXMxOTA3BgNVBAsTMChjKSAyMDEyIEVudHJ1c3QsIEluYy4gLSBmb3IgYXV0aG9yaXplZCB1c2Ugb25seTEuMCwGA1UEAxMlRW50cnVzdCBDZXJ0aWZpY2F0aW9uIEF1dGhvcml0eSAtIEwxSzAeFw0xNzA5MjkxNzAyMTNaFw0xOTA5MjgxNzMyMTJaMG8xCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRIwEAYDVQQHEwlDdXBlcnRpbm8xEzARBgNVBAoTCkFwcGxlIEluYy4xIjAgBgNVBAMTGWluaXQtcDAxc3QucHVzaC5hcHBsZS5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDZcQxsOe+k06rGzjHosZBbaLBoiwmVhbXTs6OZbz2YAwT/1qJeToldRnGWks7TafG6Nhfme0QH2gYYf11NUyC65z6rx0Gnhqe94sp4TD4ueawAYnsn+bzkC8VPQuP7KGM6Kvk4lL5EimAO9vhOSTYmKESeOYqupHCEy+J9zfHR/p5qDhRhVXHNpLqm4Hmr4H0eebulfKaCQIBAwaVwhAvfMNl3U5Lp00EsFCiUyhFgdvyR8l9Y68MU9ITft042z9+Dc+LG4sGjiWVlL3nOyuqAyE1cvDBi1lzVxjdnPHbU8ip9VhBaaIsQzpq8GxdYZodt+r3ldkUOpNu7uE9AIKWZAgMBAAGjggGQMIIBjDAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCMDMGA1UdHwQsMCowKKAmoCSGImh0dHA6Ly9jcmwuZW50cnVzdC5uZXQvbGV2ZWwxay5jcmwwSwYDVR0gBEQwQjA2BgpghkgBhvpsCgEFMCgwJgYIKwYBBQUHAgEWGmh0dHA6Ly93d3cuZW50cnVzdC5uZXQvcnBhMAgGBmeBDAECAjBoBggrBgEFBQcBAQRcMFowIwYIKwYBBQUHMAGGF2h0dHA6Ly9vY3NwLmVudHJ1c3QubmV0MDMGCCsGAQUFBzAChidodHRwOi8vYWlhLmVudHJ1c3QubmV0L2wxay1jaGFpbjI1Ni5jZXIwJAYDVR0RBB0wG4IZaW5pdC1wMDFzdC5wdXNoLmFwcGxlLmNvbTAfBgNVHSMEGDAWgBSConB03bxTP8971PfNf6dgxgpMvzAdBgNVHQ4EFgQUhhd/lweSvsZr6DOkMcH+ZuKwtvswCQYDVR0TBAIwADANBgkqhkiG9w0BAQsFAAOCAQEAWkt4Jb9WrYCMIfW/+imqU8f3Eb3KSbwO8jrvtVmVQ8De22USIb9I5HbNzXZEld6uD3VAMjxAf0riEfey1TQD3yQ0cti8pp7peAU5U05zvDa8WJmgSlydsZe7WIdlkMj2YuFY21lhxhHLqaaeWuePy+wbd72QHQIxCf1m2Oy7/hozOW7xlz9mD0OobgrBG0BUQwOTxtycmLI5FycYhw53MaJlsKm7Y5nliCPzigHL5zSSBMf35MEQwffdGpp/YM/Nz8YZQXqV02qFFvAJoiSdZE4A2bm+dMHIzR6PGy1imoBcsPqmwssk4/g0qrC0pSZhs3buInq2FnxQGaWv8YFobg==</data>
           <data>MIIE/jCCA+agAwIBAgIEUc4A/jANBgkqhkiG9w0BAQsFADCBtDEUMBIGA1UEChMLRW50cnVzdC5uZXQxQDA+BgNVBAsUN3d3dy5lbnRydXN0Lm5ldC9DUFNfMjA0OCBpbmNvcnAuIGJ5IHJlZi4gKGxpbWl0cyBsaWFiLikxJTAjBgNVBAsTHChjKSAxOTk5IEVudHJ1c3QubmV0IExpbWl0ZWQxMzAxBgNVBAMTKkVudHJ1c3QubmV0IENlcnRpZmljYXRpb24gQXV0aG9yaXR5ICgyMDQ4KTAeFw0xNDEwMTAxNTIzMTdaFw0yNDEwMTEwNjIyNDdaMIG6MQswCQYDVQQGEwJVUzEWMBQGA1UEChMNRW50cnVzdCwgSW5jLjEoMCYGA1UECxMfU2VlIHd3dy5lbnRydXN0Lm5ldC9sZWdhbC10ZXJtczE5MDcGA1UECxMwKGMpIDIwMTIgRW50cnVzdCwgSW5jLiAtIGZvciBhdXRob3JpemVkIHVzZSBvbmx5MS4wLAYDVQQDEyVFbnRydXN0IENlcnRpZmljYXRpb24gQXV0aG9yaXR5IC0gTDFLMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2j+W0E25L0Tn2zlem1DuXKVh2kFnUwmqAJqOV38pa9vH4SEkqjrQjUcj0u1yFvCRIdJdt7hLqIOPt5EyaM/OJZMssn2XyP7BtBe6CZ4DkJN7fEmDImiKm95HwzGYei59QAvS7z7Tsoyqj0ip/wDoKVgG97aTWpRzJiatWA7lQrjV6nN5ZGhTJbiEz5R6rgZFDKNrTdDGvuoYpDbwkrK6HIiPOlJ/915tgxyd8B/lw9bdpXiSPbBtLOrJz5RBGXFEaLpHPATpXbo+8DX3Fbae8i4VHj9HyMg4p3NFXU2wO7GOFyk36t0FASK7lDYqjVs1/lMZLwhGwSqzGmIdTivZGwIDAQABo4IBDjCCAQowDgYDVR0PAQH/BAQDAgEGMBIGA1UdEwEB/wQIMAYBAf8CAQAwMwYIKwYBBQUHAQEEJzAlMCMGCCsGAQUFBzABhhdodHRwOi8vb2NzcC5lbnRydXN0Lm5ldDAyBgNVHR8EKzApMCegJaAjhiFodHRwOi8vY3JsLmVudHJ1c3QubmV0LzIwNDhjYS5jcmwwOwYDVR0gBDQwMjAwBgRVHSAAMCgwJgYIKwYBBQUHAgEWGmh0dHA6Ly93d3cuZW50cnVzdC5uZXQvcnBhMB0GA1UdDgQWBBSConB03bxTP8971PfNf6dgxgpMvzAfBgNVHSMEGDAWgBRV5IHREYC+2Im5CKMx+aEkCRa5cDANBgkqhkiG9w0BAQsFAAOCAQEAWEJxwT4pFm51WHe1ZU2eKfWuCxwY+aMIQ3XvfW3xkur+zmhc4h++pa8aTKqDO1+iR0b3fJ7Bg8R6JLDpzOmimgcJ6B4dd1ZJ/FNzqEfMyS1aYDSnGgvlK7jf74JK3XBeEBgIO13cioQ9aNgAtMSeQ3hLXvBiaoyQZlOKrMV9WP9Oqa3XpMoSRynl8yIhQDJg2jr+klQeQ6ENqVI3YL+HxKHHeNWHHuV3419b3HFtukSHMQWAWAvF3nQogYMIhNDIRlr+isa9qQ47ZHhtJtw8TPeBXDwRfyU6k2Klo5EFJSNztM3OzDmkA3gwZkZeqXWwtGcDqbGfV/DTds/hk+iAog==</data>
          </array>

          <key>bag</key>
          <data>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/Pgo8IURPQ1RZUEUgcGxpc3QgUFVCTElDICItLy9BcHBsZS8vRFREIFBMSVNUIDEuMC8vRU4iICJodHRwOi8vd3d3LmFwcGxlLmNvbS9EVERzL1Byb3BlcnR5TGlzdC0xLjAuZHRkIj4KPHBsaXN0IHZlcnNpb249IjEuMCI+Cgk8ZGljdD4KCQk8a2V5PkFQTlNDb3VyaWVySG9zdG5hbWU8L2tleT4KCQk8c3RyaW5nPmNvdXJpZXIucHVzaC5hcHBsZS5jb208L3N0cmluZz4KCgkJPGtleT5BUE5TQ291cmllckhvc3Rjb3VudDwva2V5PgoJCTxpbnRlZ2VyPjUwPC9pbnRlZ2VyPgoKCQk8a2V5PkNsaWVudENvbm5lY3Rpb25SZXRyeUF0dGVtcHRzPC9rZXk+CgkJPGludGVnZXI+MTAwPC9pbnRlZ2VyPgoKCQk8a2V5PkFQTlNDb3VyaWVyU3RhdHVzPC9rZXk+CgkJPHRydWUvPgoKCQk8a2V5Pm1pbkNvbnNlY3V0aXZlS2VlcEFsaXZlc01haW50YWluaW5nV2lGaUNvbm5lY3Rpb248L2tleT4KCQk8aW50ZWdlcj4xMDwvaW50ZWdlcj4KCgkJPGtleT5taW51dGVzRGlzYWJsZVN3aXRjaGluZ1RvV2lGaUZyb21DZWxsdWxhcjwva2V5PgoJCTxpbnRlZ2VyPjE1PC9pbnRlZ2VyPgoKCQk8a2V5PkFQTlNOdW1iZXJPZkNyaXRpY2FsTWVzc2FnZUtlZXBBbGl2ZXNCZWZvcmVEaXNjb25uZWN0aW5nPC9rZXk+CgkJPGludGVnZXI+MzwvaW50ZWdlcj4KCgkJPGtleT5BUE5TQ3JpdGljYWxNZXNzYWdlS2VlcEFsaXZlVGltZXJEdXJhdGlvbjwva2V5PgoJCTxyZWFsPjEwLjA8L3JlYWw+CgoJCTxrZXk+QVBOU0NyaXRpY2FsTWVzc2FnZVRpbWVvdXQ8L2tleT4KCQk8cmVhbD4xMC4wPC9yZWFsPgoKCQk8a2V5PkFQTlNXV0FOVHJhY2tlZExpbmtRdWFsaXR5VGltZUludGVydmFsPC9rZXk+CgkJPHJlYWw+NjAwLjA8L3JlYWw+CgoJCTxrZXk+QVBOU1dXQU5UcmFja2VkTGlua1F1YWxpdHlPZmZUcmFuc2l0aW9uczwva2V5PgoJCTxpbnRlZ2VyPjI8L2ludGVnZXI+CgoJCTxrZXk+QVBOU0FXRFNsb3dSZWNlaXZlVGhyZXNob2xkPC9rZXk+CgkJPHJlYWw+NjAuMDwvcmVhbD4KCgkJPGtleT5BUE5TTG93UHJpb3JpdHlNZXNzYWdlQmF0Y2hTaXplPC9rZXk+CgkJPGludGVnZXI+NTA8L2ludGVnZXI+CgoJCTxrZXk+QVBOU0FjdGl2ZUludGVydmFsPC9rZXk+CgkJPGludGVnZXI+NTwvaW50ZWdlcj4KCgkJPGtleT5BUE5TRm9yY2VkU2hvcnRUaW1lb3V0SW50ZXJ2YWw8L2tleT4KCQk8cmVhbD4yLjA8L3JlYWw+CgoJCTxrZXk+QVBOU0Nvc3REcml2ZW5EdWFsQ2hhbm5lbEF0dGVtcHRzPC9rZXk+CgkJPGludGVnZXI+MTAwPC9pbnRlZ2VyPgoKCQk8a2V5PkFQTlNQaWdneWJhY2tEdWFsQ2hhbm5lbEF0dGVtcHRzPC9rZXk+CgkJPGludGVnZXI+NTA8L2ludGVnZXI+CgoJCTxrZXk+QVBOU01heGltdW1Mb3dQcmlvcml0eUJhdGNoZXNQZXJIb3VyPC9rZXk+CgkJPGludGVnZXI+MzwvaW50ZWdlcj4KCgkJPGtleT5BUE5TRGlzYWJsZUNvc3REcml2ZW5EdWFsQ2hhbm5lbDwva2V5PgoJCTxmYWxzZS8+CgoJCTxrZXk+QVBOU0xvd1ByaW9yaXR5QnVyc3RXaW5kb3c8L2tleT4KCQk8cmVhbD4zMC4wPC9yZWFsPgoKCQk8a2V5PkFQTlNMb3dQcmlvcml0eUJ1cnN0RGVsYXk8L2tleT4KCQk8cmVhbD4xMjAwLjA8L3JlYWw+CgoJCTxrZXk+QVBOU0xvd1ByaW9yaXR5QnVyc3RTZW5kUHJvYmFiaWxpdHk8L2tleT4KCQk8cmVhbD4wLjg8L3JlYWw+CgoJCTxrZXk+S2VlcEFsaXZlVjJUaW1lRHJpZnRNYXhpbXVtPC9rZXk+CgkJPGludGVnZXI+MDwvaW50ZWdlcj4KCgkJPGtleT5LZWVwQWxpdmVWMlRpbWVEcmlmdE1heEFsbG93ZWQ8L2tleT4KCQk8aW50ZWdlcj4zMDwvaW50ZWdlcj4KCgkJPGtleT5BUE5TSVBDYWNoaW5nVFRMTWludXRlczwva2V5PgoJCTxpbnRlZ2VyPjE0NDA8L2ludGVnZXI+CgoJCTxrZXk+QVBOU0lQQ2FjaGluZ1BlcmNlbnRhZ2U8L2tleT4KCQk8aW50ZWdlcj4wPC9pbnRlZ2VyPgoKCQk8a2V5PkVudmlyb25tZW50PC9rZXk+CgkJPHN0cmluZz5Qcm9kdWN0aW9uPC9zdHJpbmc+CgoJCTxrZXk+QVBOU05hZ2xlRW5hYmxlZDwva2V5PgoJCTxmYWxzZS8+CgoJCTxrZXk+QVBOU01pbmltdW1JbnRlcnZhbEZhbGxiYWNrRW5hYmxlZDwva2V5PgoJCTx0cnVlLz4KCgkJPGtleT5BUE5TSVBDYWNoaW5nVFRMTWludXRlc1YyPC9rZXk+CgkJPGludGVnZXI+MTQ0MDwvaW50ZWdlcj4KCgkJPGtleT5BUE5TV2lGaUtlZXBBbGl2ZUVhcmx5RmlyZUNvbnN0YW50SW50ZXJ2YWw8L2tleT4KCQk8aW50ZWdlcj4xMjA8L2ludGVnZXI+CgoJCTxrZXk+QVBOU0NvdXJpZXJIb3N0c1ByaW1hcnlJUHY0PC9rZXk+CgkJPGFycmF5PgoJCTwvYXJyYXk+CgoJCTxrZXk+QVBOU0NvdXJpZXJIb3N0c1ByaW1hcnlJUHY2PC9rZXk+CgkJPGFycmF5PgoJCTwvYXJyYXk+CgoJCTxrZXk+QVBOU0NvdXJpZXJIb3N0c1NlY29uZGFyeUlQdjQ8L2tleT4KCQk8YXJyYXk+CgkJPC9hcnJheT4KCgkJPGtleT5BUE5TQ291cmllckhvc3RzU2Vjb25kYXJ5SVB2Njwva2V5PgoJCTxhcnJheT4KCQk8L2FycmF5PgoKCQk8a2V5PkFQTlNDb3VyaWVySG9zdHNEZWZhdWx0SVB2NDwva2V5PgoJCTxhcnJheT4KCQk8L2FycmF5PgoKCQk8a2V5PkFQTlNDb3VyaWVySG9zdHNEZWZhdWx0SVB2Njwva2V5PgoJCTxhcnJheT4KCQk8L2FycmF5PgoKCQk8a2V5PkFQTlNCYWdFeHBpcnk8L2tleT4KCQk8aW50ZWdlcj4xMDgwMDwvaW50ZWdlcj4KCgkJPGtleT5BUE5TRGVmZXJyZWRIb3N0VGltZW91dDwva2V5PgoJCTxpbnRlZ2VyPjM2MDA8L2ludGVnZXI+CgoJPC9kaWN0Pgo8L3BsaXN0Pgo=</data>

         </dict>
        </plist>

**Madrid Bagger**

.. http:get:: http://init-p01md.apple.com/bag

    **Request**:

        .. sourcecode:: http

            GET /bag HTTP/1.1
            Host: init-p01md.apple.com
            Accept: */*
            Accept-Language: en-us
            Connection: keep-alive
            Accept-Encoding: gzip, deflate
            User-Agent: server-bag [iPhone OS,11.2,15C5111a,iPad4,1]

    **Response**:

        seq 314

        .. sourcecode:: http

            HTTP/1.1 200 OK
            Content-Type: application/x-apple-plist
            Last-Modified: Tue, 06 Feb 2018 23:07:15 GMT
            Content-Length: 9823
            Server: Madrid Bagger
            Cache-Control: public, max-age=780
            Date: Sun, 11 Feb 2018 10:52:41 GMT
            Connection: keep-alive

            <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
            <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
            <plist version="1.0">
             <dict>
              <key>signature</key>
              <data>E6/Nq0ILTWUUc/aZZp+sA07WDrHbUdP8zWlC5mA2/BGX0uumPCnYUwyGOwVJgmroe+ub/flY65cPnlXYDu/zXvp7coWd/AAamIaS0TOuVPjToK5pZAJqeIyQqlPUIzECgHcmmy1drOGEGZJQsbF6X2oAeWC9VH7eMcY4vLENoDyiIn/CGu2l72ZqCrsutzEYMuaRv8OMgts/SZSa+uS/qOYwyHr7WMZl07P70LJNw+X8LcvMwSyi8/ssEOD/2hgbh6uU53ID0NkeQJSf3BZ0id4VH07zIByY3agauppCrkaf5OWKD8ji7NKblCHCwwXWIaj6GK+2kCWjctkl+ftesQ==</data>

              <key>certs</key>
              <array>
               <data>MIIFPTCCBCWgAwIBAgIRAIgxe8MPSH1CAAAAAFDY4nswDQYJKoZIhvcNAQELBQAwgboxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1FbnRydXN0LCBJbmMuMSgwJgYDVQQLEx9TZWUgd3d3LmVudHJ1c3QubmV0L2xlZ2FsLXRlcm1zMTkwNwYDVQQLEzAoYykgMjAxMiBFbnRydXN0LCBJbmMuIC0gZm9yIGF1dGhvcml6ZWQgdXNlIG9ubHkxLjAsBgNVBAMTJUVudHJ1c3QgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkgLSBMMUswHhcNMTYwNzE1MTgyMDQ3WhcNMTgwNzEyMTg1MDQ2WjBqMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTESMBAGA1UEBxMJQ3VwZXJ0aW5vMRMwEQYDVQQKEwpBcHBsZSBJbmMuMR0wGwYDVQQDExRpbml0LXAwMW1kLmFwcGxlLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMWTbruOR7dmD3kge/iM9X8B8rN1T3UTvJHGRVRMF3EMfuVH1QWUwZNMBK4NcewD9gbWI/1lxd0qQ2/2jJUqkxBjA4BbIbPhz5Skn9XVwz+asbt6Uq9PhPK0ZN2UBXke03iJw9hW6+mscucmyiD8PaK0yCyXZKErAp+pqxU00lI7iQmTaMBzlJOw47ghfsyN047AAh88RE2btF9MyBVNozCvqkA1XI3Bgw6TtAclS0nB9caK1mwHTDaEb+vVt7Umw10P1nkT+y28XghRGc7xiol7179k6SX3dLOTDB8Z9BAG8xTpbfVqKDPLIFzL4hIGYk+r0ZvCEHEtynt9KRMP4QECAwEAAaOCAYswggGHMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwMwYDVR0fBCwwKjAooCagJIYiaHR0cDovL2NybC5lbnRydXN0Lm5ldC9sZXZlbDFrLmNybDBLBgNVHSAERDBCMDYGCmCGSAGG+mwKAQUwKDAmBggrBgEFBQcCARYaaHR0cDovL3d3dy5lbnRydXN0Lm5ldC9ycGEwCAYGZ4EMAQICMGgGCCsGAQUFBwEBBFwwWjAjBggrBgEFBQcwAYYXaHR0cDovL29jc3AuZW50cnVzdC5uZXQwMwYIKwYBBQUHMAKGJ2h0dHA6Ly9haWEuZW50cnVzdC5uZXQvbDFrLWNoYWluMjU2LmNlcjAfBgNVHREEGDAWghRpbml0LXAwMW1kLmFwcGxlLmNvbTAfBgNVHSMEGDAWgBSConB03bxTP8971PfNf6dgxgpMvzAdBgNVHQ4EFgQUEHvJ5vSApElatRSiHtjlxAnUqj8wCQYDVR0TBAIwADANBgkqhkiG9w0BAQsFAAOCAQEAuFG7CaI1/RD/z/eSTgnD4TkWML4F9EfL7qTYwyynR6rtP6vXim9kdHmdyDwJLeN9xcU5b5abw3eX74uypnHYmt2/KWtp/t1cjPaL//WnF2V3bGDGbezaLhuSOSkttjsYjDfbxOy+s83/uJGZPt8Qi01xNRK54AcD+r/fK+0ShNnzioJIcqOJDjONkjcPQOBV6WKqqfb+2uM4fPDmbBKCoguHUMBEjhapNjibroTRPbaL2WSoDyBpvGxUea6Ox1ZRRxsFj2kLoVzxTiSdng57xUvT8JzriueZJtNzGy5Dj9s3oxKT0pkwP2GrnZsP2pGWR0m9WAksTr/Ao0uDENlWQQ==</data>
               <data>MIIE/jCCA+agAwIBAgIEUc4A/jANBgkqhkiG9w0BAQsFADCBtDEUMBIGA1UEChMLRW50cnVzdC5uZXQxQDA+BgNVBAsUN3d3dy5lbnRydXN0Lm5ldC9DUFNfMjA0OCBpbmNvcnAuIGJ5IHJlZi4gKGxpbWl0cyBsaWFiLikxJTAjBgNVBAsTHChjKSAxOTk5IEVudHJ1c3QubmV0IExpbWl0ZWQxMzAxBgNVBAMTKkVudHJ1c3QubmV0IENlcnRpZmljYXRpb24gQXV0aG9yaXR5ICgyMDQ4KTAeFw0xNDEwMTAxNTIzMTdaFw0yNDEwMTEwNjIyNDdaMIG6MQswCQYDVQQGEwJVUzEWMBQGA1UEChMNRW50cnVzdCwgSW5jLjEoMCYGA1UECxMfU2VlIHd3dy5lbnRydXN0Lm5ldC9sZWdhbC10ZXJtczE5MDcGA1UECxMwKGMpIDIwMTIgRW50cnVzdCwgSW5jLiAtIGZvciBhdXRob3JpemVkIHVzZSBvbmx5MS4wLAYDVQQDEyVFbnRydXN0IENlcnRpZmljYXRpb24gQXV0aG9yaXR5IC0gTDFLMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2j+W0E25L0Tn2zlem1DuXKVh2kFnUwmqAJqOV38pa9vH4SEkqjrQjUcj0u1yFvCRIdJdt7hLqIOPt5EyaM/OJZMssn2XyP7BtBe6CZ4DkJN7fEmDImiKm95HwzGYei59QAvS7z7Tsoyqj0ip/wDoKVgG97aTWpRzJiatWA7lQrjV6nN5ZGhTJbiEz5R6rgZFDKNrTdDGvuoYpDbwkrK6HIiPOlJ/915tgxyd8B/lw9bdpXiSPbBtLOrJz5RBGXFEaLpHPATpXbo+8DX3Fbae8i4VHj9HyMg4p3NFXU2wO7GOFyk36t0FASK7lDYqjVs1/lMZLwhGwSqzGmIdTivZGwIDAQABo4IBDjCCAQowDgYDVR0PAQH/BAQDAgEGMBIGA1UdEwEB/wQIMAYBAf8CAQAwMwYIKwYBBQUHAQEEJzAlMCMGCCsGAQUFBzABhhdodHRwOi8vb2NzcC5lbnRydXN0Lm5ldDAyBgNVHR8EKzApMCegJaAjhiFodHRwOi8vY3JsLmVudHJ1c3QubmV0LzIwNDhjYS5jcmwwOwYDVR0gBDQwMjAwBgRVHSAAMCgwJgYIKwYBBQUHAgEWGmh0dHA6Ly93d3cuZW50cnVzdC5uZXQvcnBhMB0GA1UdDgQWBBSConB03bxTP8971PfNf6dgxgpMvzAfBgNVHSMEGDAWgBRV5IHREYC+2Im5CKMx+aEkCRa5cDANBgkqhkiG9w0BAQsFAAOCAQEAWEJxwT4pFm51WHe1ZU2eKfWuCxwY+aMIQ3XvfW3xkur+zmhc4h++pa8aTKqDO1+iR0b3fJ7Bg8R6JLDpzOmimgcJ6B4dd1ZJ/FNzqEfMyS1aYDSnGgvlK7jf74JK3XBeEBgIO13cioQ9aNgAtMSeQ3hLXvBiaoyQZlOKrMV9WP9Oqa3XpMoSRynl8yIhQDJg2jr+klQeQ6ENqVI3YL+HxKHHeNWHHuV3419b3HFtukSHMQWAWAvF3nQogYMIhNDIRlr+isa9qQ47ZHhtJtw8TPeBXDwRfyU6k2Klo5EFJSNztM3OzDmkA3gwZkZeqXWwtGcDqbGfV/DTds/hk+iAog==</data>
              </array>

              <key>bag</key>
              <data>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/Pgo8IURPQ1RZUEUgcGxpc3QgUFVCTElDICItLy9BcHBsZS8vRFREIFBMSVNUIDEuMC8vRU4iICJodHRwOi8vd3d3LmFwcGxlLmNvbS9EVERzL1Byb3BlcnR5TGlzdC0xLjAuZHRkIj4KPHBsaXN0IHZlcnNpb249IjEuMCI+Cgk8ZGljdD4KCQk8a2V5PmF0dC13aWZpLW1heC1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj41MjQyODgwPC9pbnRlZ2VyPgoKCQk8a2V5PmF0dC1jZWxsdWxhci1tYXgtZmlsZS1zaXplPC9rZXk+CgkJPGludGVnZXI+NTI0Mjg4PC9pbnRlZ2VyPgoKCQk8a2V5PmF0dC13aWZpLXZpZGVvLW1heC1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj4xMDQ4NTc2MDwvaW50ZWdlcj4KCgkJPGtleT5hdHQtd2lmaS12aWRlby1jZWxsdWxhci1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj4xMDQ4NTc2PC9pbnRlZ2VyPgoKCQk8a2V5PmF0dC13aWZpLWF1ZGlvLW1heC1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj4xMDQ4NTc2MDwvaW50ZWdlcj4KCgkJPGtleT5hdHQtd2lmaS1hdWRpby1jZWxsdWxhci1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj4xMDQ4NTc2PC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXJlc2VuZC1hcy1zbXMtdGltZW91dDwva2V5PgoJCTxpbnRlZ2VyPjMwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLW1lc3NhZ2Utc2VuZC10aW1lb3V0LXRpbWU8L2tleT4KCQk8aW50ZWdlcj45MDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC1wb3NpdGl2ZS1jYWNoZS10aW1lPC9rZXk+CgkJPGludGVnZXI+Mjg4MDA8L2ludGVnZXI+CgoJCTxrZXk+bWQtcGVlci1sb29rdXAtbmVnYXRpdmUtY2FjaGUtdGltZTwva2V5PgoJCTxpbnRlZ2VyPjg2NDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLWVuYWJsZS1vdHI8L2tleT4KCQk8aW50ZWdlcj4wPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLXVua25vd24tY2FjaGUtdGltZTwva2V5PgoJCTxpbnRlZ2VyPjE4MDA8L2ludGVnZXI+CgoJCTxrZXk+bWQtYXV0by1yZXNlbmQtYXMtc21zLXRpbWVvdXQtdXBwZXI8L2tleT4KCQk8aW50ZWdlcj42MDA8L2ludGVnZXI+CgoJCTxrZXk+bWQtYXV0by1yZXNlbmQtYXMtc21zLXRpbWVvdXQtbG93ZXI8L2tleT4KCQk8aW50ZWdlcj4zMDA8L2ludGVnZXI+CgoJCTxrZXk+bWQtdXNlLWJpbmFyeS1wbGlzdDwva2V5PgoJCTxpbnRlZ2VyPjE8L2ludGVnZXI+CgoJCTxrZXk+bWQtb2ZmbGluZS1tZXNzYWdlLXJlc3BvbnNlLXRpbWU8L2tleT4KCQk8aW50ZWdlcj4wPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLWlkLXN0YXR1cy1yZXF1ZXN0LWRlbGF5LXNob3J0PC9rZXk+CgkJPGludGVnZXI+MzwvaW50ZWdlcj4KCgkJPGtleT5tZC1pZC1zdGF0dXMtcmVxdWVzdC1kZWxheS1sb25nPC9rZXk+CgkJPGludGVnZXI+ODwvaW50ZWdlcj4KCgkJPGtleT5tZC1tYXgtY2hhdC1wYXJ0aWNpcGFudHM8L2tleT4KCQk8aW50ZWdlcj4zMjwvaW50ZWdlcj4KCgkJPGtleT5tZC10eXBpbmctaW5kaWNhdG9yLW92ZXJyaWRlPC9rZXk+CgkJPGZhbHNlLz4KCgkJPGtleT5hdHQtcmV0ZW50aW9uLWRheXM8L2tleT4KCQk8aW50ZWdlcj4zMDwvaW50ZWdlcj4KCgkJPGtleT5hdHQtYXV4LXZpZGVvLW1heC1maWxlLXNpemU8L2tleT4KCQk8aW50ZWdlcj4yMDk3MTUyPC9pbnRlZ2VyPgoKCQk8a2V5PmF0dC1hdXgtdmlkZW8tbWluLWZpbGUtc2l6ZTwva2V5PgoJCTxpbnRlZ2VyPjEwNDg1NzY8L2ludGVnZXI+CgoJCTxrZXk+bWQtcGVlci1sb29rdXAtcG9zaXRpdmUtY2FjaGUtdGltZS1jb20uYXBwbGUucHJpdmF0ZS5hYzwva2V5PgoJCTxpbnRlZ2VyPjg2NDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLW5lZ2F0aXZlLWNhY2hlLXRpbWUtY29tLmFwcGxlLnByaXZhdGUuYWM8L2tleT4KCQk8aW50ZWdlcj4xMjk2MDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLXVua25vd24tY2FjaGUtdGltZS1jb20uYXBwbGUucHJpdmF0ZS5hYzwva2V5PgoJCTxpbnRlZ2VyPjEyMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC1wb3NpdGl2ZS1jYWNoZS10aW1lLWNvbS5hcHBsZS5lc3M8L2tleT4KCQk8aW50ZWdlcj44NjQwMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC1uZWdhdGl2ZS1jYWNoZS10aW1lLWNvbS5hcHBsZS5lc3M8L2tleT4KCQk8aW50ZWdlcj4xMjk2MDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLXVua25vd24tY2FjaGUtdGltZS1jb20uYXBwbGUuZXNzPC9rZXk+CgkJPGludGVnZXI+MTIwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLXBvc2l0aXZlLWNhY2hlLXRpbWUtY29tLmFwcGxlLnByaXZhdGUuYWxsb3kuYnVsbGV0aW5ib2FyZDwva2V5PgoJCTxpbnRlZ2VyPjg2NDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLW5lZ2F0aXZlLWNhY2hlLXRpbWUtY29tLmFwcGxlLnByaXZhdGUuYWxsb3kuYnVsbGV0aW5ib2FyZDwva2V5PgoJCTxpbnRlZ2VyPjg2NDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLXVua25vd24tY2FjaGUtdGltZS1jb20uYXBwbGUucHJpdmF0ZS5hbGxveS5idWxsZXRpbmJvYXJkPC9rZXk+CgkJPGludGVnZXI+MzYwMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC1wb3NpdGl2ZS1jYWNoZS10aW1lLWNvbS5hcHBsZS5wcml2YXRlLmFsbG95Lm1hcHM8L2tleT4KCQk8aW50ZWdlcj44NjQwMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC1uZWdhdGl2ZS1jYWNoZS10aW1lLWNvbS5hcHBsZS5wcml2YXRlLmFsbG95Lm1hcHM8L2tleT4KCQk8aW50ZWdlcj44NjQwMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1wZWVyLWxvb2t1cC11bmtub3duLWNhY2hlLXRpbWUtY29tLmFwcGxlLnByaXZhdGUuYWxsb3kubWFwczwva2V5PgoJCTxpbnRlZ2VyPjEyMDwvaW50ZWdlcj4KCgkJPGtleT5tZC1kZWNyeXB0aW9uLWZhaWx1cmUtcmV0cmllcy1wZXItd2Vlazwva2V5PgoJCTxpbnRlZ2VyPjEwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXJldHJ5LWF0dGVtcHRzPC9rZXk+CgkJPGludGVnZXI+MjwvaW50ZWdlcj4KCgkJPGtleT5tZC1yZXRyeS1zdGFydC1pbnRlcnZhbDwva2V5PgoJCTxpbnRlZ2VyPjYwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXBlZXItbG9va3VwLW5lZ2F0aXZlLWNhY2hlLXRpbWUtY29tLmFwcGxlLnByaXZhdGUuYWxsb3kuZWxlY3RyaWN0b3VjaDwva2V5PgoJCTxpbnRlZ2VyPjE0NDAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1kLXFyYS1rZWVwLWFsaXZlLWludGVydmFsLXNlY29uZHM8L2tleT4KCQk8aW50ZWdlcj4xODAwPC9pbnRlZ2VyPgoKCQk8a2V5Pm1tY3Mtd2hpdGVsaXN0PC9rZXk+CgkJPGFycmF5PgoJCQk8c3RyaW5nPmljbG91ZDwvc3RyaW5nPgoJCTwvYXJyYXk+CgoJCTxrZXk+SG9tZUtpdENvdXJpZXJIb3N0bmFtZTwva2V5PgoJCTxzdHJpbmc+aG9tZWtpdC5wdXNoLmFwcGxlLmNvbTwvc3RyaW5nPgoKCQk8a2V5PkhvbWVLaXRDb3VyaWVySG9zdGNvdW50PC9rZXk+CgkJPGludGVnZXI+NTA8L2ludGVnZXI+CgoJCTxrZXk+SG9tZUtpdFJlcG9ydFVybDwva2V5PgoJCTxzdHJpbmc+aHR0cHM6Ly9yZXBvcnQuaG9tZWtpdC5wdXNoLmFwcGxlLmNvbS9yZXBvcnQ8L3N0cmluZz4KCgkJPGtleT5jay1jbGllbnQtbWF4LXRpbWUtdG8tZGVmZXItbmlnaHRseS1zeW5jPC9rZXk+CgkJPGludGVnZXI+MzYwMDwvaW50ZWdlcj4KCgkJPGtleT5jay1jYWNoZS1kZWxldGUtdmVyc2lvbjwva2V5PgoJCTxpbnRlZ2VyPjI8L2ludGVnZXI+CgoJCTxrZXk+Y2stc3RvcC10dHI8L2tleT4KCQk8dHJ1ZS8+CgoJCTxrZXk+bm9udXJnZW50X2ludGVybmV0X3NlbmRfaW50ZXJ2YWw8L2tleT4KCQk8aW50ZWdlcj4xODAwPC9pbnRlZ2VyPgoKCQk8a2V5PnN5bmNfdG9waWNzX2FsbG93ZWRfdG9fc2VuZF9pbW1lZGlhdGVseTwva2V5PgoJCTxhcnJheT4KCQkJPHN0cmluZz48L3N0cmluZz4KCQk8L2FycmF5PgoKCQk8a2V5PnNob3VsZC1kdW1wLWxvZ3Mtb24taWNsb3VkLWtpdC1lcnJvcjwva2V5PgoJCTx0cnVlLz4KCgkJPGtleT5zaG91bGQtZHVtcC1sb2dzLWRhaWx5LWlmLWhhdmVudC1zeW5jZWQ8L2tleT4KCQk8dHJ1ZS8+CgoJCTxrZXk+ZGlzYWJsZS1zci1jb250YWluZXItc3luYzwva2V5PgoJCTx0cnVlLz4KCgk8L2RpY3Q+CjwvcGxpc3Q+Cg==</data>

             </dict>
            </plist>

- This could be iTunes bag below

.. http:get:: http://init.ess.apple.com/WebObjects/VCInit.woa/wa/getBag?ix=4

    **Request**:

        .. sourcecode:: http

            Host: init.ess.apple.com
            Accept: */*
            Accept-Language: en-us
            Connection: keep-alive
            Accept-Encoding: gzip, deflate
            User-Agent: server-bag [iPhone OS,11.2,15C5111a,iPad4,1]

    **Response**:

        .. sourcecode:: http

            HTTP/1.1 200 OK
            Server: AppleHttpServer/b636f100
            Content-Type: text/xml
            Content-Length: 6920
            X-Apple-Request-UUID: f21821f0-aed7-b375-dc09-94b2ce3a6f27
            X-Apple-Jingle-Correlation-Key: 6IMCD4FO26ZXLXAJSSZM4OTPE4
            apple-seq: 0
            apple-tk: false
            Apple-Originating-System: UnknownOriginatingSystem
            X-Responding-Instance: Init:400100:magent0756.usprz06.pie.apple.com:31002:18CB:nocommit
            X-Apple-Splunk-Hint: SH:1:VEN-PROD:f21821f0-aed7-b375-dc09-94b2ce3a6f27::1518346361:c24e32fdd59aed694b094309c37b44255e3982241621a003491e3a2406eab0f3:EM
            Cache-Control: max-age=3312
            Content-Encoding: gzip
            X-B3-TraceId: 22ef3c3d11a5402f
            Date: Sun, 11 Feb 2018 10:52:41 GMT
            Connection: keep-alive
            Vary: Accept-Encoding

            <?xml
            <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
            <plist
                version="1.0">
                <dict>
                    <key>
                        signature
                        </key>
                    <data>
                         [truncated]BgD3jlkq94FXBQlnxf8IVSS4FZK9lEZPLfmv0G10cTIcRAjHW6gALGlzfgv7HH6UbYCzof6SdzVb+lx2vJjSvRRwb+WOX7PmpBusOJpJak26fpsDn/p+EQ4tFOKDpuMR4EFCN5lxnU9ianCQm9T5UTKudKqpzS9RlNiRTBZTbxE1WFBHJWMjigl9qrtYMuspW4gTRaTMFGz72e+725tx8NZw5/Fsjzf6L/Y
                        </data>
                    <key>
                        certs
                        </key>
                    <array>
                        <data>
                             [truncated]MIIGbzCCBVegAwIBAgIQI8WdF7Xjfzo7bI40VmbLODANBgkqhkiG9w0BAQsFADB+MQswCQYDVQQGEwJVUzEdMBsGA1UEChMUU3ltYW50ZWMgQ29ycG9yYXRpb24xHzAdBgNVBAsTFlN5bWFudGVjIFRydXN0IE5ldHdvcmsxLzAtBgNVBAMTJlN5bWFudGVjIENsYXNzIDMgU2VjdXJlIFNlcnZlciBDQSA
                            </data>
                        <data>
                             [truncated]MIIFODCCBCCgAwIBAgIQUT+5dDhwtzRAQY0wkwaZ/zANBgkqhkiG9w0BAQsFADCByjELMAkGA1UEBhMCVVMxFzAVBgNVBAoTDlZlcmlTaWduLCBJbmMuMR8wHQYDVQQLExZWZXJpU2lnbiBUcnVzdCBOZXR3b3JrMTowOAYDVQQLEzEoYykgMjAwNiBWZXJpU2lnbiwgSW5jLiAtIEZvciBhdXRob3JpemV
                            </data>
                        </array>
                    <key>
                        bag
                        </key>
                    <data>
                         [truncated]PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjwhRE9DVFlQRSBwbGlzdCBQVUJMSUMgIi0vL0FwcGxlIENvbXB1dGVyLy9EVEQgUExJU1QgMS4wLy9FTiIgImh0dHA6Ly93d3cuYXBwbGUuY29tL0RURHMvUHJvcGVydHlMaXN0LTEuMC5kdGQiPgo8cGx
                        </data>
                    </dict>
                </plist>



OCSP / CRL URLs used in this section
------------------------------------

- `ocsp.entrust.com` CNAME OF:
    - `ecsp.entrust.net.edgekey.net` CNAME OF:
        - *.akamaiedge.net

- `sr.symcd.com` CNAME OF:
    - `ocsp-ds.ws.symantec.com`
- `s2.symcb.com` CNAME OF:
    - `ocsp-ds.ws.symantec.com`

- `ocsp.apple.com`
