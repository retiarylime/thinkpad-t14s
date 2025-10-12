# Trackpoint Settings

```
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

## Changing trackpoint acceleration

Default value `libinput Accel Speed (318):	0.224944`

```
xinput --set-prop "TPPS/2 Elan TrackPoint" "libinput Accel Speed" 0.5
```

## Changing trackpoint sensitivity

Default value 
```
$ cat  /sys/devices/platform/i8042/serio1/sensitivity

128
```

To edit value
```
echo 64 | sudo tee /sys/devices/platform/i8042/serio1/sensitivity
```

### To keep value after reboot

[https://www.reddit.com/r/thinkpad/comments/15l99up/tweak_thinkpad_x1c9_trackpoint_on_debian_12/](https://www.reddit.com/r/thinkpad/comments/15l99up/tweak_thinkpad_x1c9_trackpoint_on_debian_12/)

> However, I want to keep the value after reboot. I noticed that my trackpoint was named as "TPPS/2 Elan TrackPoint", instead of the "TPPS/2 IBM TrackPoint" mentioned in most articles.
> 
> ```
> $ cat /sys/devices/platform/i8042/serio1/input/input6/name
> TPPS/2 Elan TrackPoint
> ```
>
> I wrote an udev rule for my "TPPS/2 Elan TrackPoint" to lower the sensitivity from 128 to 64.
> ```
> $ vi /etc/udev/rules.d/10-trackpoint.rules
> ACTION=="add", SUBSYSTEM=="input", ATTR{name}=="TPPS/2 Elan TrackPoint", ATTR{device/sensitivity}="64", ATTR{device/press_to_select}="1"
> ```
> ```
> $ sudo udevadm control --reload-rules && sudo udevadm trigger
> ```

## evdev driver