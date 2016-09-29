# Strong Jazz

Download Pi Finder application from [here](http://ivanx.com/raspberrypi/).

Connect drive to power and Raspberry Pi USB port.

```
$ ls -l /dev/disk/by-label
```

Add line to `/etc/fstab`:
```
/dev/sda2 /mnt ntfs defaults,nofail,x-systemd.device-timeout=10 0 0
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

Enable energy saving spindown after inactivity (24*5=120 seconds)

```
$ scp drivespindown.service pi@raspberrypi.local:/etc/systemd/system/
```

```
$ sudo systemctl daemon-reload
$ sudo systemctl enable drivespindown.service
```
