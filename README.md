# Synchronizing bluetoothd and pulseaudio
## The problem
Bluetoothd, the bluetooth daemon that handles bluetooth device in Linux is normally started under the control of a system initialisation manager such as Systemd. The pulseaudio audio routing process as usually started as a per user process from the autostart mechanisms associated with a graphical desktop manager. The desktop manager itself is usually among the last processes started as part of the initial boot processing. There is no synchronisation between these two launch mechanisms. This means that bluetoothd has usually finished its initialisation well before pulseaudio has started.
This is not a problem for non audio bluetooth devices. However audio bluetooth devices that attempt to use pulseaudio via dbus as part of their initialisation will always fail. In practice this means any device that requires 'load-module module-bluez5-discover' in the pulseaudio configuration. This type of failure is characterised by the appearance in the system logs and journals of a message similar to the following.
```
Sep 15 09:04:25 dragon bluetoothd[1423]: src/service.c:btd_service_connect() a2dp-sink profile connect failed for 65:DE:65:37:94:79: Protocol not available.
```

## The solution
The easiest way to fix this is restart bluetoothd after pulseaudio has fully started.
The are two components to this solution. One is simple shell script that used systemctl to restart the bluetooth daemon. This script is called from the kick-bluetooth.desktop file. This file needs to placed in the autostart directory of the user requiring use of the device. This is usually ~/.config/autostart.
## Caveats
This solution works well on gnome desktops. Other environments may need some tweaks. Feel free to contribute what works for you and I will add it to these notes.
