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

`sudo apt-get install xorg lightdm fluxbox midori`

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
```

add the following to `/home/kiosk/.fluxbox/init`

```
session.screen0.toolbar.autoHide: false
session.screen0.toolbar.visible:  false
```
