## Original resource 
- from [https://linuxconfig.org/how-to-set-battery-charge-thresholds-on-linux](https://linuxconfig.org/how-to-set-battery-charge-thresholds-on-linux)

## Quick Guide

1. On your terminal, run below
```
echo 75 | sudo tee /sys/class/power_supply/BAT0/charge_start_threshold
echo 80 | sudo tee /sys/class/power_supply/BAT0/charge_stop_threshold

```
2. You may change the percentage for `charge_start_threshold` and `charge_stop_threshold`.

> [!INFO]
> If your laptop has multiple batteries, run `upower -e` to check the battery name e.g., BAT0, BAT1,...