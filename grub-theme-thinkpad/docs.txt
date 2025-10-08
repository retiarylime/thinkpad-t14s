## Original resource 
- from [https://github.com/Coopydood/HyperFluent-GRUB-Theme](https://github.com/Coopydood/HyperFluent-GRUB-Theme)

## Quick Installation Guide
### Manually edited specially for Thinkpad

1. Downloading the hyperfluent-thinkpad folder
2. Create the directory
```
sudo mkdir -p /usr/share/grub/themes/hyperfluent

```
3. Edit `/etc/default/grub` using (`nano`,`vi`,`vim`,`neovim`, or any text editor) with `sudo` or `su`.
  - If required, change `GRUB_TERMINAL_OUTPUT="console"` to `GRUB_TERMINAL_OUTPUT="gfxterm"`
  - Uncomment `# GRUB_THEME = "..."` and replace the path with `/usr/share/grub/themes/hyperfluent/hyperfluent-thinkpad/theme.txt`

  > [!TIP]
> If you are using Linux Mint, comment all lines in `/etc/default/grub.d/60_mint-theme.cfg`

4. Run the appropriate command in the terminal to apply the above changes

  ```sh
  sudo grub-mkconfig -o /boot/grub/grub.cfg #Debian/Ubuntu
  sudo grub2-mkconfig -o /boot/grub2/grub.cfg #Fedora/CentOS/RHEL/SUSE
  ```

5. Run `reboot` to reboot the system and test your new awesome-looking GRUB theme! :)