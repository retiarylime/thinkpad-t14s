# Tips for faster booting time

### 1. Check which daemon takes the longest time

Run `systemd-analyze blame`

Result:
```
5.727s NetworkManager-wait-online.service
 841ms NetworkManager.service
 698ms user@1000.service
 694ms blueman-mechanism.service
 582ms tlp.service
 537ms accounts-daemon.service
 494ms upower.service
 483ms dev-nvme0n1p5.device
 461ms udisks2.service
 413ms polkit.service
 382ms lm-sensors.service
 380ms rsyslog.service
 317ms systemd-logind.service
```

We can see that `NetworkManager-wait-online.service` is taking the longest time which is 5.7s.

### 2. Disable `NetworkManager-wait-online.service`

Run `sudo systemctl disable NetworkManager-wait-online.service`

### 3. Try reboot to see the changes
