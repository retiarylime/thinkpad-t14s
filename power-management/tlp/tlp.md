# Install & Configure TLP

1. TLP Official Documentation

    [https://linrunner.de/tlp/index.html](https://linrunner.de/tlp/index.html)

2. Mask/Disable power-profiles-daemon

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
    ```

4. Install TLP

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

5. Configure TLP

    [https://linrunner.de/tlp/settings/introduction.html](https://linrunner.de/tlp/settings/introduction.html)

- Create backup of original `/etc/tlp.conf`
    ```bash
    sudo cp /etc/tlp.conf /etc/tlp.conf.bak
    ```
    [https://www.reddit.com/r/thinkpad/comments/1dub49a/tlp_configuration_for_thinkpad_p14s_gen_5_t14_gen/](https://www.reddit.com/r/thinkpad/comments/1dub49a/tlp_configuration_for_thinkpad_p14s_gen_5_t14_gen/)

- Create & copy files into `/etc/tlp.d/*.conf`
    ```bash
    sudo touch /etc/tlp.d/98-tlp.conf
    sudo sublime_text /etc/tlp.d/98-tlp.conf
    ```

- Copy below config to `/etc/tlp.d/98-tlp.conf` <br>
    [https://gist.github.com/kikislater/4de3d79b0459681933ba630e29bcb0e0](https://gist.github.com/kikislater/4de3d79b0459681933ba630e29bcb0e0)


    **AND**

    ```bash
    sudo touch /etc/tlp.d/99-tlp.conf
    sudo sublime_text /etc/tlp.d/99-tlp.conf
    ```

    Copy below config to `/etc/tlp.d/99-tlp.conf` to override above <br>
    [https://gist.github.com/pauloromeira/787c75d83777098453f5c2ed7eafa42a](https://gist.github.com/pauloromeira/787c75d83777098453f5c2ed7eafa42a)


6. Start TLP

    ```bash
    sudo tlp start
    ```
7. Logout & relogin to apply changes

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

    [https://github.com/retiarylime/thinkpad-t14s/blob/main/power-management/battery-charge-threshold/battery-charge-threshold.md](https://github.com/retiarylime/thinkpad-t14s/blob/main/power-management/battery-charge-threshold/battery-charge-threshold.md)

4. Reboot to apply changes



