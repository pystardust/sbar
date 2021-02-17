# sbar
Simple bar for dwm and other window managers.

## Features
* Modules update with variable timing
* Signalling to update module when needed

# Setup

* Add bar module as a function. Make sure to assign it to a variable.
```
update_cpu () { 
	cpu="$( grep -o "^[^ ]*" /proc/loadavg )" 
}
```

* Modify display function to change the positioning of output
```
display () { 
	xsetroot -name " [$weather] [$memory $cpu] [$bat] [$backlight] [$vol] $time "
}
```
you can use printf instead of xsetroot to feed it into some other bar line lemonbar.

* If signalling needed then add
```
trap "update_cpu;display" "RTMIN+6"
```
	* this will update cpu when signal 40=34+6 is given to the script (RTMIN = 34)
	* to update it from external commands
	```
		kill -40 "$(cat ~/.cache/pidofbar)"
	```
	* Example from my sxhkrc
	```
		{XF86AudioRaiseVolume,XF86AudioLowerVolume}
			pulsemixer --change-volume {+,-}5 ; \
			kill -34 "$(cat ~/.cache/pidofbar)"
	```


* Add the update information in the while loops as follows
``` 
[ $((sec % 60)) -eq 2 ] && update_cpu
```
to update item ever 60 seconds with an offset of 2 seconds.


