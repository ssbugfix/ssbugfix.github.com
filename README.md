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
    # this udev file should be used with udev 188 and newer
    ACTION!="add|change", GOTO="u2f_end"
    
    # Yubico YubiKey
    KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="1050", ATTRS{idProduct}=="0113|0114|0115|0116|0120|0121|0200|0402|0403|0406|0407|0410", TAG+="uaccess", GROUP="plugdev", MODE="0660"
    
    LABEL="u2f_end"
EOF
