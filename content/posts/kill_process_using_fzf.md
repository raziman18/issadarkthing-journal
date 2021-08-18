---
title: "Kill Process Using Fzf"
date: 2021-08-17T17:48:24+08:00
tags: ["bash", "fzf"]
---


I wrote a helper script to find processes and kill them using fzf. By default
it uses sigterm 15 to kill the process. If you want to change the sigterm code
for example, 9 you can just pass it as argument like so `kp 9`. To exit without
killing any process, press `esc` key.

Copy and paste this to a file and rename it as `kp`. After that, change the
file's permission to executable `chmod +x kp`. Move the file to be in one of the
`$PATH` directory so that you can freely use command `kp` anywhere in your
system.

```bash
#!/bin/bash

# default to sigterm -15
SIGCODE=-${1:-15}

pid=$(ps -ef \
	| sed 1d \
	| eval "fzf ${FZF_DEFAULT_OPTS} -m --header='[kill:process:$SIGCODE]'" \
	| awk '{print $2}')

if [[ -n $pid ]]; then
  echo $pid | xargs kill $SIGCODE
  $0
fi
```
