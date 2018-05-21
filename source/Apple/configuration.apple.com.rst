configuration.apple.com
=======================

Used by:
    - **locationd** (iOS)
    - **geod** (iOS)


	GET /configurations/internetservices/bt/bcwv.plist HTTP/1.1
Host	configuration.apple.com
Accept	*/*
Accept-Language	en-us
Connection	keep-alive
Accept-Encoding	br, gzip, deflate
User-Agent	locationd/2245.0.41 CFNetwork/897.15 Darwin/17.5.0

<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
	<dict>
		<key>version</key>
		<integer>12</integer>
	</dict>
</plist>


geod example
------------

Fetch geod settings from

GET /configurations/pep/config/geo/networkDefaults-ios-11.0.plist HTTP/1.1

