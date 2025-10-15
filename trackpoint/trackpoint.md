# Trackpoint Settings for Thinkpad T14s Gen 2 AMD

## Original settings

```bash
xinput list-props "TPPS/2 Elan TrackPoint"
Device 'TPPS/2 Elan TrackPoint':
	Device Enabled (175):	1
	Coordinate Transformation Matrix (177):	1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
	libinput Natural Scrolling Enabled (309):	0
	libinput Natural Scrolling Enabled Default (310):	0
	libinput Scroll Methods Available (311):	0, 0, 1
	libinput Scroll Method Enabled (312):	0, 0, 1
	libinput Scroll Method Enabled Default (313):	0, 0, 1
	libinput Button Scrolling Button (314):	2
	libinput Button Scrolling Button Default (315):	2
	libinput Button Scrolling Button Lock Enabled (316):	0
	libinput Button Scrolling Button Lock Enabled Default (317):	0
	libinput Middle Emulation Enabled (351):	0
	libinput Middle Emulation Enabled Default (352):	0
	libinput Accel Speed (318):	0.224944
	libinput Accel Speed Default (319):	0.000000
	libinput Accel Profiles Available (320):	1, 1, 1
	libinput Accel Profile Enabled (321):	1, 0, 0
	libinput Accel Profile Enabled Default (322):	1, 0, 0
	libinput Accel Custom Fallback Points (323):	<no items>
	libinput Accel Custom Fallback Step (324):	0.000000
	libinput Accel Custom Motion Points (325):	<no items>
	libinput Accel Custom Motion Step (326):	0.000000
	libinput Accel Custom Scroll Points (327):	<no items>
	libinput Accel Custom Scroll Step (328):	0.000000
	libinput Left Handed Enabled (329):	0
	libinput Left Handed Enabled Default (330):	0
	libinput Send Events Modes Available (290):	1, 0
	libinput Send Events Mode Enabled (291):	0, 0
	libinput Send Events Mode Enabled Default (292):	0, 0
	Device Node (293):	"/dev/input/event8"
	Device Product ID (294):	2, 10
	libinput Drag Lock Buttons (331):	<no items>
	libinput Horizontal Scroll Enabled (332):	1
	libinput Scrolling Pixel Distance (333):	15
	libinput Scrolling Pixel Distance Default (334):	15
	libinput High Resolution Wheel Scroll Enabled (335):	1
```

## Issues

| Driver     | Cursor  | Scrolling                                                              |
|------------|---------|------------------------------------------------------------------------|
| `libinput` | ✅smooth  | ✅smooth                                                                |
| `evdev`    | ✅smooth | ❌jumpy<br>_(due to scrolling is an emulation of up/down arrow button)_ |

## `libinput` config

**Resources:**
- [https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html#pointer-acceleration](https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html#pointer-acceleration)
- [https://www.mankier.com/4/libinput](https://www.mankier.com/4/libinput)

1. Set `libinput` as driver for trackpoint at `/etc/X11/xorg.conf.d/20-thinkpad.conf`. You need to modify the acceleration profile and speed to your liking.

	- You can test and play around with acceleration values using command below:


	```bash
	xinput --set-prop "TPPS/2 Elan TrackPoint" "libinput Accel Profile Enabled" 0 1 0

	xinput --set-prop "TPPS/2 Elan TrackPoint" "libinput Accel Speed" 0.2
	```

	- After confirming the acceleration profile & speed, run the command below:

	```bash
	sudo nano /etc/X11/xorg.conf.d/20-thinkpad.conf
	```

	```conf
	Section "InputClass"
		Identifier    "Trackpoint Wheel Emulation"
		Driver "libinput"
		MatchProduct    "TPPS/2 Elan TrackPoint"
		MatchDevicePath    "/dev/input/event*"
		Option			"AccelProfile"		"flat"
		Option			"AccelSpeed"		"0.2"
	EndSection
	```

	- Profile: Supports three profiles: “adaptive”, “flat” and “custom” respectively in order. Sets either boolean [0, 1]. E.g. to enable `adaptive` profile, set to `1 0 0`. My favourite profile is flat `0 1 0`.

	- Speed:  Sets the pointer acceleration speed within the range [-1, 1]. 0 is default. My favourite speed is `0.2`.


2. You can modify the `udev` rules trackpoint device attributes at `/etc/udev/rules.d/10-trackpoint.rules`

	```bash
	sudo nano /etc/udev/rules.d/10-trackpoint.rules
	```

	```ini
	ACTION=="add",
	SUBSYSTEM=="input",
	ATTR{name}=="TPPS/2 Elan TrackPoint",
	ATTR{device/sensitivity}="190",
	ATTR{device/speed}="150",
	ATTR{device/inertia}="10",
	ATTR{device/rate}="100",libinput
	ATTR{device/press_to_select}="0" 
	```

3. Logout and relogin to apply changes.

4. Final `libinput` properties for trackpoint

	```bash
	xinput --list-props "TPPS/2 Elan TrackPoint"
	```

	```bash
	Device 'TPPS/2 Elan TrackPoint':
		Device Enabled (175):	1
		Coordinate Transformation Matrix (177):	1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
		libinput Natural Scrolling Enabled (309):	0
		libinput Natural Scrolling Enabled Default (310):	0
		libinput Scroll Methods Available (311):	0, 0, 1
		libinput Scroll Method Enabled (312):	0, 0, 1
		libinput Scroll Method Enabled Default (313):	0, 0, 1
		libinput Button Scrolling Button (314):	2
		libinput Button Scrolling Button Default (315):	2
		libinput Button Scrolling Button Lock Enabled (316):	0
		libinput Button Scrolling Button Lock Enabled Default (317):	0
		libinput Middle Emulation Enabled (351):	0
		libinput Middle Emulation Enabled Default (352):	0
		libinput Accel Speed (318):	0.200000
		libinput Accel Speed Default (319):	0.000000
		libinput Accel Profiles Available (320):	1, 1, 1
		libinput Accel Profile Enabled (321):	0, 1, 0
		libinput Accel Profile Enabled Default (322):	1, 0, 0
		libinput Accel Custom Fallback Points (323):	<no items>
		libinput Accel Custom Fallback Step (324):	0.000000
		libinput Accel Custom Motion Points (325):	<no items>
		libinput Accel Custom Motion Step (326):	0.000000
		libinput Accel Custom Scroll Points (327):	<no items>
		libinput Accel Custom Scroll Step (328):	0.000000
		libinput Left Handed Enabled (329):	0
		libinput Left Handed Enabled Default (330):	0
		libinput Send Events Modes Available (290):	1, 0
		libinput Send Events Mode Enabled (291):	0, 0
		libinput Send Events Mode Enabled Default (292):	0, 0
		Device Node (293):	"/dev/input/event11"
		Device Product ID (294):	2, 10
		libinput Drag Lock Buttons (331):	<no items>
		libinput Horizontal Scroll Enabled (332):	1
		libinput Scrolling Pixel Distance (333):	15
		libinput Scrolling Pixel Distance Default (334):	15
		libinput High Resolution Wheel Scroll Enabled (335):	1
	```
