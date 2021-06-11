# Select desktop profile

    eselect profile list
    eselect profile set <number>
    
# USE flags

## Remove heavyweight software use flags

    -dbus -elogind -libnotify -pam -policykit -udisks -upower
    
## Xorg with lxqt

    emerge --ask lxqt-meta
