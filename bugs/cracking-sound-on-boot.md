# Remove cracking sound on boot LMDE 7

[https://forums.linuxmint.com/viewtopic.php?t=442358](https://forums.linuxmint.com/viewtopic.php?t=442358)

1. Run below (even if your PC is AMD):
    ```
    echo "options snd-hda-intel power_save=0" | sudo tee /etc/modprobe.d/snd-hda-intel.conf
    ```

    ```
    sudo update-initramfs -u
    ```

2. Force the parameter via kernel cmdline

    ```bash
    sudo nano /etc/default/grub
    ```

    Find the line:

    ```
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
    ```

    Change it to:

    ```
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash snd_hda_intel.power_save=0"
    ```

    Save, then:

    ```bash
    sudo update-grub
    ```

3. Reboot & check

    ```bash
    cat /sys/module/snd_hda_intel/parameters/power_save
    ```