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


# `evdev` driver

### Resources
- [https://askubuntu.com/questions/1240460/how-to-get-the-trackpoint-sensitivity-right-on-my-thinkpad-x1-carbon-gen-6-with](https://askubuntu.com/questions/1240460/how-to-get-the-trackpoint-sensitivity-right-on-my-thinkpad-x1-carbon-gen-6-with)
- [https://baach.de/Members/jhb/fixing-the-trackpoint-on-ubuntu](https://baach.de/Members/jhb/fixing-the-trackpoint-on-ubuntu)
- [https://calvinrw.com/thinkpad-trackpoint](https://calvinrw.com/thinkpad-trackpoint)

1. Conf to use `evdev` driver

```
sudo nano /etc/X11/xorg.conf.d/20-thinkpad.conf
```
```
Section "InputClass"
    Identifier    "Trackpoint Wheel Emulation"
    Driver "evdev"
    MatchProduct    "TPPS/2 Elan TrackPoint"
    MatchDevicePath    "/dev/input/event*"
    Option        "EmulateWheel"        "true"
    Option        "EmulateWheelButton"    "2"
    Option        "Emulate3Buttons"    "false"
    Option        "XAxisMapping"        "6 7"
    Option        "YAxisMapping"        "4 5"
EndSection 
```

2. Logout & relogin to apply changes above

3. Configure `udev` rules fro trackpoint

```
sudo nano /etc/udev/rules.d/10-trackpoint.rules
```
```
ACTION=="add",
SUBSYSTEM=="input",
ATTR{name}=="TPPS/2 Elan TrackPoint",
ATTR{device/sensitivity}="200",
ATTR{device/speed}="150",
ATTR{device/inertia}="10",
ATTR{device/press_to_select}="0"
```

3. Apply `udev` rules changes

```
sudo udevadm trigger
```

4. Recheck the driver used for trackpoint

```
grep -i "Using input driver" /var/log/Xorg.0.log
```

```
[ 19868.628] (II) Using input driver 'libinput' for 'Power Button'
[ 19868.739] (II) Using input driver 'libinput' for 'Video Bus'
[ 19868.789] (II) Using input driver 'libinput' for 'Power Button'
[ 19868.829] (II) Using input driver 'libinput' for 'Sleep Button'
[ 19868.870] (II) Using input driver 'libinput' for 'ELAN901C:00 04F3:2D4C'
[ 19868.929] (II) Using input driver 'libinput' for 'ELAN0679:00 04F3:3196 Mouse'
[ 19868.978] (II) Using input driver 'libinput' for 'ELAN0679:00 04F3:3196 Touchpad'
[ 19869.042] (II) Using input driver 'libinput' for 'AT Translated Set 2 keyboard'
[ 19869.099] (II) Using input driver 'evdev' for 'TPPS/2 Elan TrackPoint'
[ 19869.107] (II) Using input driver 'libinput' for 'ThinkPad Extra Buttons'
```

5. xinput output
```
xinput list-props "TPPS/2 Elan TrackPoint"
Device 'TPPS/2 Elan TrackPoint':
	Device Enabled (175):	1
	Coordinate Transformation Matrix (177):	1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
	Device Accel Profile (301):	0
	Device Accel Constant Deceleration (302):	1.000000
	Device Accel Adaptive Deceleration (303):	1.000000
	Device Accel Velocity Scaling (304):	17.873051
	Device Product ID (294):	2, 10
	Device Node (293):	"/dev/input/event8"
	Evdev Axis Inversion (354):	0, 0
	Evdev Axes Swap (356):	0
	Axis Labels (357):	"Rel X" (185), "Rel Y" (186)
	Button Labels (358):	"Button Left" (178), "Button Middle" (179), "Button Right" (180), "Button Wheel Up" (181), "Button Wheel Down" (182), "Button Horiz Wheel Left" (183), "Button Horiz Wheel Right" (184)
	Evdev Scrolling Distance (359):	0, 0, 0
	Evdev Middle Button Emulation (360):	0
	Evdev Middle Button Timeout (361):	50
	Evdev Middle Button Button (362):	2
	Evdev Third Button Emulation (363):	0
	Evdev Third Button Emulation Timeout (364):	1000
	Evdev Third Button Emulation Button (365):	3
	Evdev Third Button Emulation Threshold (366):	20
	Evdev Wheel Emulation (367):	1
	Evdev Wheel Emulation Axes (368):	6, 7, 4, 5
	Evdev Wheel Emulation Inertia (369):	10
	Evdev Wheel Emulation Timeout (370):	200
	Evdev Wheel Emulation Button (371):	2
	Evdev Drag Lock Buttons (372):	0
```

