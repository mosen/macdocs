xp.apple.com
============

This service seems to be largely for reporting errors that occurred back to apple.

Example paylaods

	POST /report/2/psr_ota HTTP/1.1
Host	xp.apple.com
Content-Type	application/json
Connection	keep-alive
Accept	*/*
User-Agent	$%7BPRODUCT_NAME%7D/$%28CURRENT_PROJECT_VERSION%29 CFNetwork/897.15 Darwin/17.5.0
Content-Length	617
Accept-Language	en-au
Accept-Encoding	br, gzip, deflate

{"clientId":"FFC2C490-C7FD-433F-820D-49396F5BC04F","events":[{"reportVersion":"2","splunkErrorCount":1,"failureReason":"V2 networking issue","httpCode":-16,"pallasClientNonce":"B84BCD17-CD95-4D3C-BF42-C8B16A6F2DE7","event":"catalogLookup","eventUuid":"10CCCD86-F250-4C8D-8757-89C2FE6FE06C","currentOSVersion":"11.3","type":"backgroundDiscretionary","splunkResultKey":"fail","BuildVersion":"15E216","eventTime":"1523945461538","assetType":"com.apple.MobileAsset.SoftwareUpdate","sessionId":"FFC2C490-C7FD-433F-820D-49396F5BC04F","MobileAssetAssetAudience":"01c1d682-6e8f-4908-b724-5501fe3f5e5c","HWModelStr":"J81AP"}]}
