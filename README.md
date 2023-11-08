# Slackware useful tips

### Bad sound with Bluetooth headset
Solution: switch to Pipewire and HCF mSBC codec
    # slackpkg install pipewire
    # /usr/sbin/pipewire-enable.sh
Then connect the headset, go to Audio Settings, and select mSBC codec to use.

### Disable internal microphone boost
    # echo 'load-module module-echo-cancel aec_args="analog_gain_control=0 digital_gain_control=0"' > /etc/pulse/default.pa.d/echo-cansel.pa
Or try alsamixer, set boost to 0 and save settings:
    # alsactl store

### Using yubikey for 2fa in firefox (in gitlab, for example)
The problem is - wrong device permissions when usb device added. To fix:
    # cat << EOF > /etc/udev/rules.d/70-u2f.rules
    ACTION!="add|change", GOTO="u2f_end"
    KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="1050", ATTRS{idProduct}=="0113|0114|0115|0116|0120|0121|0200|0402|0403|0406|0407|0410", TAG+="uaccess", GROUP="plugdev", MODE="0660"
    LABEL="u2f_end"
EOF

### Launch Zoom from firefox with meeting link
1. Open about:config
2. Add bool-type key 'network.protocol-handler.external.zoommtg', set it to 'true'
3. find ~/.mozilla/firefox -type f -name handlers.json
4. Edit handlers.json, add 
    "zoommtg":{"handlers":[{"name":"zoom.sh","path":"/home/bugfix/bin/zoom.sh"}],"action":2}}
so, in my case, it looks like
    {"defaultHandlersVersion":{"en-US":4},"mimeTypes":{"application/pdf":{"action":3,"extensions":["pdf"]},"text/xml":{"action":0,"extensions":["xml"]},"image/svg+xml":{"action":0,"extensions":["svg"]},"image/webp":{"action":3,"extensions":["webp"]},"binary/octet-stream":{"action":0,"ask":false,"extensions":["xz"]},"text/plain":{"action":0,"ask":false,"extensions":["txt","text"]},"image/avif":{"action":3,"extensions":["avif"]}},"schemes":{"irc":{"stubEntry":true,"handlers":[null,{"name":"Mibbit","uriTemplate":"https://www.mibbit.com/?url=%s"}]},"ircs":{"stubEntry":true,"handlers":[null,{"name":"Mibbit","uriTemplate":"https://www.mibbit.com/?url=%s"}]},"mailto":{"stubEntry":true,"handlers":[null,{"name":"Yahoo! Mail","uriTemplate":"https://compose.mail.yahoo.com/?To=%s"},{"name":"Gmail","uriTemplate":"https://mail.google.com/mail/?extsrc=mailto&url=%s"}]},"ext+treestyletab":{"action":2,"handlers":[{"name":"Tree Style Tab","uriTemplate":"moz-extension://2ae079b1-56e3-45c6-8e1d-50e3aa593bf2/resources/protocol-handler.html?%s"}]},"zoommtg":{"handlers":[{"name":"zoom.sh","path":"/home/bugfix/bin/zoom.sh"}],"action":2}},"isDownloadsImprovementsAlreadyMigrated":true,"isSVGXMLAlreadyMigrated":true}
