# Strong Jazz

OS: Raspbian Jessie Lite

## Drive Setup

Connect drive to power and Raspberry Pi USB port. Need to find the Drive's UUID. The following is useful for identifying the drive:

```
$ ls -l /dev/disk/by-label
$ ls -l /dev/disk/by-uuid
```

Add line to `/etc/fstab`:
```
UUID=<your-drive-uuid> /mnt ntfs defaults,nofail,x-systemd.device-timeout=10 0 0
```

Note: This `x-systemd.device-timeout` setting causes systemd to wait 10 seconds for mounting to succeed or fail--if the drive is not plugged in this will cause boot to take 10 seconds longer.

## Samba Setup

Verify Samba is installed
```
$ samba --version
```

Add section to `/etc/samba/smb.conf`:
```
[shared]
comment = HELLO WORLD
path = /mnt
read only = no
guest ok = yes
```

Restart Samba
```
$ sudo /etc/init.d/samba restart
```

## Connecting to the server

On Mac, open Finder, Go (menu item) -> Connect to Server

Server Address: smb://raspberrypi.local/Shared

Connect as Guest

## Enable energy saving spindown after inactivity

(Optional) Checkout this repo (or download files) and edit the UUID in drivespindown.sh to match your drive's.

### Copy files to the Pi

```
$ scp drivespindown.sh pi@raspberry.local:/usr/local/bin/
$ scp drivespindown.service pi@raspberry.local:/etc/systemd/system/
```

### Enable the spindown service

```
$ ssh pi@raspberrypi.local
$ sudo systemctl enable spindown.service
$ sudo reboot
```

Useful links:
Pi Finder application from [here](http://ivanx.com/raspberrypi/) is useful if the Pi stops showing up on the network.
