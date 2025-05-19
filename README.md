# `SMJobBless` + XPC #

`SMJobBlessXPC` is based off Apple's `SMJobBless` sample code, but uses XPC over Mach ports for communication between the app and privileged helper tool.

## Original README ##

For the original `README` that ships with Apple's `SMJobBless` sample code, see `ReadMe-Original.txt`.

## Outline of changes to the original sample code ##

The following changes have been made to Apple's sample code:

* Project files and build configuration have been updated to more modern settings.
* Issues that were causing compile warnings have been corrected.
* Code signing identity has been changed from "Joe Developer" to "Mac Developer". You will need to edit `SMJobBlessApp/SMJobBlessApp-Info.plist` and `SMJobBlessHelper/SMJobBlessHelper-Info.plist` to reflect the Common Name (CN) of the certificate you're using to sign your app & helper tool.
* The helper tool registers a Mach service named `com.apple.bsd.SMJobBlessHelper` with `launchd`.
* The host application connects to the helper tool using XPC via the registered Mach service, sends a message and receives a response.
* A status log is displayed in a text field in the host application to show what's going on.

## SMAuthorizedClients needs to be set for "SMJobBlessHelper-Info.plist" and "SMJobBlessApp-Info.plist"
  1. codesign --display --requirement - /path/to/YourClientApp.app. 🔑 [Apple Development]
     designated => identifier "com.apple.bsd.SMJobBlessApp" and anchor apple generic and certificate leaf[subject.CN] = "Apple Development:" and      certificate 1[field.xxx.xxx.xxx] /* exists */
  2. codesign --display --requirement - /path/to/YourClientApp.app. 🔑 [Developer ID Application]
     designated => anchor apple generic and identifier "com.apple.bsd.SMJobBlessApp" and (certificate leaf[field.xx.xx.xx.xx] /*     exists */       or certificate 1[field.xx.xx.xx.xx] /* exists */ and certificate leaf[field.xx.xx.xx] /* exists */   and certificate leaf[subject.OU] = xx)
  3. Open "SMJobBlessApp-Info.plist", edit
      "<dict>
		  <key>com.apple.bsd.SMJobBlessHelper</key>
		  <string>identifier com.apple.bsd.SMJobBlessHelper and certificate leaf[subject.CN] = &quot;Mac Developer: Nathan de Vries (T3RB9JQ8KZ)&quot;      </string>
	</dict>"
  4. Open "SMJobBlessHelper-Info.plist, edit <key>SMAuthorizedClients</key>
	<array>
		<string>identifier com.apple.bsd.SMJobBlessApp and certificate leaf[subject.CN] = &quot;Mac Developer: Nathan de Vries (T3RB9JQ8KZ)&quot;</string>
	</array>
   
     
