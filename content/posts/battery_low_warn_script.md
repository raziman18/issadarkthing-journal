---
title: "Battery Low Warn Script"
date: 2021-08-17T20:42:57+08:00
draft: false
tags: ["bash", "window_manager"]
---

If you are using window manager for example [i3](https://i3wm.org/) or
[xmonad](https://xmonad.org/). You might miss a feature from
[DE](https://en.wikipedia.org/wiki/Desktop_environment) that you used before
that is battery low warning. If you use pc, that's fine but if you are a laptop
user like me, this script may become handy.

Running `$ upower -i /org/freedesktop/UPower/devices/battery_BAT1` will output
info about your laptop's battery.
```sh
  native-path:          BAT1
  vendor:               Hewlett-Packard
  model:                PABAS0241231
  serial:               41167
  power supply:         yes
  updated:              Tue 17 Aug 2021 09:24:13 PM (41 seconds ago)
  has history:          yes
  has statistics:       yes
  battery
    present:             yes
    rechargeable:        yes
    state:               fully-charged
    warning-level:       none
    energy:              20.1779 Wh
    energy-empty:        0 Wh
    energy-full:         20.4435 Wh
    energy-full-design:  41.7533 Wh
    energy-rate:         0.0781538 W
    voltage:             12.724 V
    percentage:          100%
    capacity:            45.9198%
    technology:          lithium-ion
    icon-name:          'battery-full-charged-symbolic'
```

You can find out the battery percentage under `battery > percentage`.  Using
this info, we can capture the battery level of our laptop by using
[awk](https://en.wikipedia.org/wiki/AWK).  The output of the info can be piped
into 

`awk '/percentage/ {gsub(/%/, ""); print $2}'`. 

Running the following command 

`$ upower -i /org/freedesktop/UPower/devices/battery_BAT1 | awk '/percentage/
{gsub(/%/, ""); print $2}'` 

Will output `100` showing my laptop battery percentage. Now we have to make a
script to run every 30 seconds to check if battery has reached low percentage
i.e. 20%. 

The following script will simply do that but it raises a problem that it will
continuously warn the user battery low every 30 seconds.

```sh
#!/usr/bin/env bash

BATT_THRESHOLD=20 # change here

while :;do

	BAT_PERCENTAGE=$(upower -i /org/freedesktop/UPower/devices/battery_BAT1 \
		| awk '/percentage/ {gsub(/%/, ""); print $2}')

	if [[ $BAT_PERCENTAGE -le $BATT_THRESHOLD ]]; then
		notify-send -u "critical" "Battery Low Warning"
	fi

	sleep 30

done
```

To prevent that from happening, we can set a variable to `true` if the user has
been warned about battery low and set it back to `false` when the battery has
been charged up and surpasses the battery low threshold (20%).
[Bash](https://www.gnu.org/software/bash/) technically has no boolean data type
so we set it as string named 'true' and 'false'.

The final script will look like this:
```sh
#!/usr/bin/env bash

HASBEENWARNED=false
BATT_THRESHOLD=20 # change here

while :;do

	BAT_PERCENTAGE=$(upower -i /org/freedesktop/UPower/devices/battery_BAT1 \
		| awk '/percentage/ {gsub(/%/, ""); print $2}')

	if [[ $BAT_PERCENTAGE -le $BATT_THRESHOLD ]] && [[ $HASBEENWARNED = false ]]; then
		notify-send -u "critical" "Battery Low Warning"
		HASBEENWARNED=true
	elif [[ $BAT_PERCENTAGE -gt $BAT_PERCENTAGE ]] && [[ $HASBEENWARNED = true ]]; then
		HASBEENWARNED=false
	fi

	sleep 30

done
```

Hopefully this will fix the missing feature from WM world :)
