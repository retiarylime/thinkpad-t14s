## Original resource 
- From [https://linuxconfig.org/how-to-set-battery-charge-thresholds-on-linux](https://linuxconfig.org/how-to-set-battery-charge-thresholds-on-linux)
- To set battery start and stop charge threshold without TLP.

## Quick Guide

1. On your terminal, run below
```bash
echo 75 | sudo tee /sys/class/power_supply/BAT0/charge_start_threshold
echo 80 | sudo tee /sys/class/power_supply/BAT0/charge_stop_threshold
```

2. You may change the percentage for `charge_start_threshold` and `charge_stop_threshold`.

> [!TIP]
> If your laptop has multiple batteries, run `upower -e` to check the battery name e.g., BAT0, BAT1,...
