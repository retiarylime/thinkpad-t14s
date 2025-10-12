# Fix Gamma on Thinkpad 14s Gen 2 AMD Display

### Original 

	```
	xrandr --verbose | grep -i gamma
		Gamma:      1.0:1.0:1.0
		GAMMA_LUT_SIZE: 4096 
		DEGAMMA_LUT_SIZE: 4096 
		GAMMA_LUT: 0 
		DEGAMMA_LUT: 0 
		GAMMA_LUT_SIZE: 4096 
		DEGAMMA_LUT_SIZE: 4096 
		GAMMA_LUT: 0 
		DEGAMMA_LUT: 0 
		GAMMA_LUT_SIZE: 4096 
		DEGAMMA_LUT_SIZE: 4096 
		GAMMA_LUT: 0 
		DEGAMMA_LUT: 0 
		GAMMA_LUT_SIZE: 4096 
		DEGAMMA_LUT_SIZE: 4096 
		GAMMA_LUT: 0 
		DEGAMMA_LUT: 0 
	```

## Tweaks

1. Mask `colord.service`

	```
	sudo systemctl mask colord.service
	```

2. Persistent gamma 0.9:0.9:0.9 for `eDP`

	1. Create a user-level systemd service

	```bash
	mkdir -p ~/.config/systemd/user
	nano ~/.config/systemd/user/set-gamma.service
	```

	Paste this:

	```
		[Unit]
		Description=Apply gamma correction after Cinnamon starts
		After=graphical-session.target
		Wants=graphical-session.target

		[Service]
		Type=oneshot
		ExecStart=/bin/bash -c "sleep 1 && /usr/bin/xrandr --output eDP --gamma 0.9:0.9:0.9"

		[Install]
		WantedBy=default.target
	```

	---

	2. Enable it for your user

		```bash
		systemctl --user daemon-reload
		systemctl --user enable --now set-gamma.service
		```

	3. You can check it immediately

		```bash
		xrandr --verbose | grep -i gamma
		```

	---

	4. Reboot and confirm

		After you log in again, check:

		```bash
		xrandr --verbose | grep -i gamma
		```

## After tweaks

```
xrandr --verbose | grep -i gamma
	Gamma:      1.1:1.1:1.1
	GAMMA_LUT_SIZE: 4096 
	DEGAMMA_LUT_SIZE: 4096 
	GAMMA_LUT: 0 
	DEGAMMA_LUT: 0 
	GAMMA_LUT_SIZE: 4096 
	DEGAMMA_LUT_SIZE: 4096 
	GAMMA_LUT: 0 
	DEGAMMA_LUT: 0 
	GAMMA_LUT_SIZE: 4096 
	DEGAMMA_LUT_SIZE: 4096 
	GAMMA_LUT: 0 
	DEGAMMA_LUT: 0 
	GAMMA_LUT_SIZE: 4096 
	DEGAMMA_LUT_SIZE: 4096 
	GAMMA_LUT: 0 
	DEGAMMA_LUT: 0 

```
- Note that the gamma is `1.1:1.1:1.1` even we set as `0.9:0.9:0.9`