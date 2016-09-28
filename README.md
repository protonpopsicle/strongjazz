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

Note: With this x-systemd.device-timeout setting, systemd will wait 10 seconds for device to mount--if its not plugged in this will cause boot to be 10 seconds slower.

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
