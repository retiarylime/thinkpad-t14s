# Install & Configure TLP

1. TLP Official Documentation

    [https://linrunner.de/tlp/index.html](https://linrunner.de/tlp/index.html)

<!-- 2. Mask/Disable power-profiles-daemon

    [https://www.reddit.com/r/archlinux/comments/r41ax3/uninstall_or_disable_gnome_powerprofiledaemon/](https://www.reddit.com/r/archlinux/comments/r41ax3/uninstall_or_disable_gnome_powerprofiledaemon/)

    > In TLP's documentation the suggested way to disable power-profiles-daemon is to mask the service:

    [https://linrunner.de/tlp/faq/ppd.html](https://linrunner.de/tlp/faq/ppd.html)

    ```bash
    sudo systemctl stop power-profiles-daemon.service
    sudo systemctl mask power-profiles-daemon.service
    ```

3. Reset battery charge start/stop threshold

    ```bash
    echo 95 | sudo tee /sys/class/power_supply/BAT0/charge_start_threshold
    echo sudo systemctl restart power-profiles-daemon.service100 | sudo tee /sys/class/power_supply/BAT0/charge_stop_threshold
    ``` -->

2. Install TLP

    [https://linrunner.de/tlp/installation/debian.html](https://linrunner.de/tlp/installation/debian.html)

    ```bash
    sudo nano /etc/apt/sources.listbbashash

    # Add the following line to your /etc/apt/sources.list:

    deb http://ftp.debian.org/debian DIST-backports main
    ```

    ```bash
    sudo apt update
    sudo apt -t trixie-backports install tlp tlp-rdw
    ```

    > [!WARNING]
    > Conflicting packages will be  uninstalled automatically i.e power-profiles-daemon.
    > auto-cpufreq also conflict with TLP but need to manually uninstall the daemon.

3. Configure TLP

    [https://linrunner.de/tlp/settings/introduction.html](https://linrunner.de/tlp/settings/introduction.html)

- Create backup of original `/etc/tlp.conf`
    ```bash
    sudo cp /etc/tlp.conf /etc/tlp.conf.bak
    ```
    [https://www.reddit.com/r/thinkpad/comments/1dub49a/tlp_configuration_for_thinkpad_p14s_gen_5_t14_gen/](https://www.reddit.com/r/thinkpad/comments/1dub49a/tlp_configuration_for_thinkpad_p14s_gen_5_t14_gen/)

- Copy [tlp.conf](tlp.conf) into `/etc/tlp.conf`
    ```bash
    sudo cp tlp.conf /etc/tlp.conf
    ```

4. Start TLP

    ```bash
    sudo tlp start
    ```

# Remove TLP & return to power-profiles-daemon

1. Reinstall power-profiles-daemon

    ```bash
    sudo apt install power-profiles-daemon
    ```

    - This package have conflicts with `tlp` package and it will automatically uninstall `tlp`.

2. Unmasking/enable & restart the power-profiles-daemon.service

    ```bash
    sudo systemctl restart power-profiles-daemon.service
    sudo systemctl unmask power-profiles-daemon.service
    sudo systemctl enable power-profiles-daemon.service
    sudo systemctl restart power-profiles-daemon.service
    ```

3. Apply battery charge threshold

    [battery-charge-threshold](../battery-charge-threshold/battery-charge-threshold.md)

4. Reboot to apply changes



