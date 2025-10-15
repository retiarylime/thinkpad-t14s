## Original resource 
- Inspired by [https://github.com/Coopydood/HyperFluent-GRUB-Theme](https://github.com/Coopydood/HyperFluent-GRUB-Theme)

## Showcase

### Background
  ![background](hyperfluent-thinkpad/background.png)
### Preview (captured on camera)
  ![preview](hyperfluent-thinkpad/preview.png)

## Quick Installation Guide
### Manually edited specially for Thinkpad

1. Git clone this repo
2. Create the directory
```
sudo mkdir -p /usr/share/grub/themes/hyperfluent
```

3. Copy the `hyperfluent-thinkpad` folder to `/usr/share/grub/themes/hyperfluent`
```
sudo cp -r /path/to/the/cloned/repo/thinkpad-t14s/grub-theme-thinkpad/hyperfluent-thinkpad /usr/share/grub/themes/hyperfluent
```

4. Edit `/etc/default/grub` using (`nano`,`vi`,`vim`,`neovim`, or any text editor) with `sudo` or `su`.
  - If required, change `GRUB_TERMINAL_OUTPUT="console"` to `GRUB_TERMINAL_OUTPUT="gfxterm"`
  - Uncomment `# GRUB_THEME = "..."` and replace the path with `/usr/share/grub/themes/hyperfluent/hyperfluent-thinkpad/theme.txt`

> [!NOTE]
> If you are running Linux Mint, comment all lines in `/etc/default/grub.d/60_mint-theme.cfg`

4. Run the appropriate command in the terminal to apply the above changes

  ```sh
  sudo grub-mkconfig -o /boot/grub/grub.cfg #Debian/Ubuntu
  sudo update-grub
  ```

> [!NOTE]
> If you are running Fedora/CentOS/RHEL/SUSE, run below `sudo grub2-mkconfig -o /boot/grub2/grub.cfg`

5. Run `reboot` to reboot the system and test your new awesome-looking GRUB theme! :)

> [!IMPORTANT]
> Make sure to disable secure boot in your BIOS settings
