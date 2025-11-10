# Thinkpad T14s Gen 2 AMD

This repository collects notes, tweaks and configuration snippets I use to customise my ThinkPad T14s Gen 2 AMD running LMDE 7.

Use this document as a living table of contents for the repo. Each section links to a focused guide or configuration file.

![screenshot](screenshot.png)

## Table of contents

1. System overview
	- [System info](system-info.md) — Hardware, kernel, environment and battery details. 
	- [My favourite apps](my-favourite-apps.md) — Short list of software I love.
3. Display and UI
	- [Gamma fixes](gamma-fix.md) — My screen on Linux somehow has low contrast. So I fixed it by setting a higher contrast display gamma and run as user systemd service.
	- [GTK styling tweaks](gtk-styling/gtk-styling.md) — Highlight active window using coloured borders with custom `gtk.css`. But has totally being replaced with my self-developed Cinnamon extension [dim-unfocused-windows](https://github.com/retiarylime/dim-unfocused-windows)
	- [Dim unfocused windows](https://github.com/retiarylime/dim-unfocused-windows) — My self-developed Cinnamon extension to stay focus on active window by dimming unfocused windows.
	- [Thinkpad wallpaper](wallpaper) — Wallpapers for Thinkpad.
4. Input and pointing devices
	- [Trackpoint tweaks](trackpoint/trackpoint.md) — Smoother TrackPoint tweaks for my ThinkPad.
5. Power management
	- [auto-cpufreq](power-management/auto-cpufreq/auto-cpufreq.md) — CPU and battery optimizer. For me, this works better than TLP at saving battery life.
	- [TLP](power-management/tlp/tlp.md) — Installing and configuring TLP for battery life. But I found that auto-cpufreq does better job. 
	- [Battery charge thresholds](power-management/battery-charge-threshold/battery-charge-threshold.md) — How to set start/stop thresholds without TLP.
6. Boot & firmware
	- [Faster boot time](faster-boot-time/faster-boot-time.md) — `systemd-analyze` and services to consider disabling.
	- [GRUB theme for ThinkPad](grub-theme-thinkpad/grub-theme-thinkpad.md) — Personal customization of GRUB theme for ThinkPad inspired by [hyperfluent theme](https://github.com/Coopydood/HyperFluent-GRUB-Theme).
7. Troubleshooting
	- [Cracking sound on boot](troubleshoot/cracking-sound-on-boot.md) — Workaround for audio crackle on startup.

## Using these customizations

- These are personal customization notes and examples; treat them as guidance, not a polished distribution/package.
- When applying system-wide changes (services, modprobe, udev, GRUB), review commands before running and keep backups (for example: copy config files before editing).

## Layout of this repository

- Root README (this file): docs index and TOC.
- Individual guides are stored as Markdown files organised by topic (power-management, grub-theme-thinkpad, gtk-styling, bugs, etc.)