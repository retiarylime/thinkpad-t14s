# Remove cracking sound on boot LMDE 7

[https://forums.linuxmint.com/viewtopic.php?t=442358](https://forums.linuxmint.com/viewtopic.php?t=442358)

Run (even if your PC is AMD):
```
echo "options snd-hda-intel power_save=0" | sudo tee /etc/modprobe.d/snd-hda-intel.conf
```