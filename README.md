# pi-touchscreen-kiosk

2022-05-04

### hardware

* official 7" Touchscreen
* Pi3B

## configure video

set legacy video driver and rotate display

update `/boot/config.txt`

```
#dtoverlay=vc4-kms-v3d
lcd_rotate=2
```

## install software

`sudo apt-get install xorg lightdm fluxbox midori unclutter`

## configure user

add group `sudo groupadd kiosk`

add user `sudo useradd -g kiosk -m kiosk`

set password `sudo passwd kiosk`

## configure boot

use `raspi-config` to enable autologin to desktop

update `/etc/lightdm/lightdm.conf`

change the `autologin-user=` line to this

```
autologin-user=kiosk
```

create the file `/home/kiosk/.xsession`

```
#!/bin/bash
startfluxbox
```

## configure fluxbox

add the following to `/home/kiosk/.fluxbox/startup`

```
xset -dpms
xset s off
xset s noblank
SPA_URL=""
midori -e fullscreen $SPA_URL &
unclutter -idle 2 &
```

add the following to `/home/kiosk/.fluxbox/init`

```
session.screen0.toolbar.autoHide: false
session.screen0.toolbar.visible:  false
```

## set times for screensaver to work

create file `/home/kiosk/ss_stop.sh`

```
#!/bin/bash
xset s off -display :0
xset s reset -display :0
```

turns off the screen saver and resets it

create file `/home/kiosk/ss_start.sh`

```
#!/bin/bash
xset s blank -display :0
xset s on -display :0
xset s 20 -display :0
```

sets the screensaver to blank, on and 20s delay

make these files executable with `sudo chmod +x <filename>`

update the cron, create the file `/etc/cron.d/kiosk`

```
0 6 * * * kiosk /home/kiosk/ss_stop.sh
0 21 * * * kiosk /home/kiosk/ss_start.sh
```

starts the screen saver at 21:00 and stops it at 06:00
