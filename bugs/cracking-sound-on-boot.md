# Remove cracking sound on boot LMDE 7

[https://forums.linuxmint.com/viewtopic.php?t=442358](https://forums.linuxmint.com/viewtopic.php?t=442358)

1. Run below (even if your PC is AMD):
```
echo "options snd-hda-intel power_save=0" | sudo tee /etc/modprobe.d/snd-hda-intel.conf
```
2. Rebuild initramfs

```
sudo update-initramfs -u
```

3. Reboot
