# MQTT Desktop Status Updater

This is a very simply python script to send desktop status information into MQTT and ultimately Home Assistant.

It is able to sent two important pieces of information into MQTT/HA and these are:
 * Whether or not the screen is locked
 * Whether or not we are connected to a LAN

This information can be used for whatever purposes you like but by default a "Computer" device is created with an "At Desk" sensor.

This is defined as when you are both connected to the LAN and the screen is unlocked with a status update having been seen within 30s

I use this to control desk/monitor back lights etc without any further hardware sensors (e.g. a chair pressure sensor)

This currently supports the GNOME desktop under Linux (or anything which uses the GNOME screensaver service)

While a LAN connection might not mean YOUR LAN (e.g. office vs home) due to the unique names given to network interfaces these days, the chances are your home network interface will have a different name to your office one (assuming it's provided by a docking station of some kind), hense I didn't feel the need to make this logic more complex.

## Installation

 * Copy the ```mqtt-status-updater``` file to ```/usr/bin/```
 * Copy the ```mqtt-status-updater.service``` file to ```/usr/lib/systemd/user``` (or ```/etc/systemd/user```)
 * Call ```systemctl --user daemon-reload``` then ```systemctl --user enable mqtt-status-updater.service``` and ```systemctl --user start mqtt-status-updater.service```
 * Ensure you configure things in your ```~/.config/mqtt-status-updater.conf``` file.

## Configuration

Use the file ```~/.config/mqtt-status-updater.conf``` to setup connection details

Example:

```
[mqtt]
host=192.168.123.123
username=mqtt
password=supersecret
port=1883 # This is the default
topic=your/status/topic # Defaults to mqtt_status_updater/username

[lan]
interface=enp0s13f0u1u3

[homeassistant]
basetopic=homeassistant # This is the defaults - used for autodiscovery
uniqueid=flooble # Your sensor/device's unique id - defaults to mqtt_status_updater_username

[log]
level=INFO # The default - can be set to DEBUG or other constant name for more details
```

## Improvements

It would be nice to know the screen blanking state rather than lock state, so e.g. the login prompt under GDM triggers the lights

Support other desktops (e.g. KDE)

The MQTT connection and loop could be made a bit nicer. With an established connection I could probably move away from a timestamp and instead use availability information of the writer to know when a computer might have just disappeared (laptop lid closed but no time to send status update)

But for now, this serves it's purpose fine my needs.
