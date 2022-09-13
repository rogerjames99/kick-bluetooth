# Synchronizing bluetoothd and Pulse Audio
## The problem
Bluetoothd, the bluetooth daemon that handles bluetooth device in Linux is normally started under the control of a system initialisation manager such as Systemd. The pulseaudio audio routing process as usually started as a per user process from the autostart mechanisms associated with a graphical desktop manager. The desktop manager itself is usually among the last processes started as part of the initial boot processing. There is no synchronisation between the two. This means that bluetoothd has finished its initialisation before pulseaudio has started.
This is not a problem for non audio bluetooth devices. However audio bluetooth devices that attempt to use pulseaudio via dbus as part of their initialisation will always fail.
## The solution
The easiest way to fix this is restart bluetoothd after pulseaudio has fully started.

## Note to self
Use bluez dbus api to trigger rescan from pulseaudio.
