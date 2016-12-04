# Strong Jazz

OS: Raspbian Jessie Lite

Connect drive to power and Raspberry Pi USB port.

```
$ ls -l /dev/disk/by-uuid
```

Add line to `/etc/fstab`:
```
UUID=<your-drive-uuid> /mnt ntfs defaults,nofail,x-systemd.device-timeout=10 0 0
```

Note: This `x-systemd.device-timeout` setting causes systemd to wait 10 seconds for mounting to succeed or fail--if the drive is not plugged in this will cause boot to take 10 seconds longer.

Add section to `/etc/samba/smb.conf`:
```
[shared]
comment = HELLO WORLD
path = /mnt
read only = no
guest ok = yes
```

## Enable energy saving spindown after inactivity

Edit the UUID in drivespindown.sh to match your drive's.

(12*5=60 seconds)

```bash
$ mv drivespindown.sh /usr/local/bin/
$ mv spindown.service /etc/systemd/system/
$ sudo systemctl enable spindown.service
$ sudo reboot
```

Useful links:
Pi Finder application from [here](http://ivanx.com/raspberrypi/).
