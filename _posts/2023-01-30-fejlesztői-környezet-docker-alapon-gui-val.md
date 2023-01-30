---
layout: post
title: Fejlesztői környezet Docker alapon GUI-val
date: 2023-01-30 06:13 +0100
categories: [Hasznos]
---

# Cél
Grafikus felületek tesztelésére alkalmas Docker konténer előállítása.

# Előzmény
Az elmúlt időszakban azon dolgoztam, hogy egy olyan Docker fejlesztői környezetet alakítsak ki, amely az alap konténerizált előnyök mellett továbbiakat is biztosít. Nézzünk ezek közül néhányat:

* grafikus felhasználói felület tesztelése
* konténerben futó böngésző
* selenium teszt futás közben ellenőrizhető
* CI/CD pipeline-ba parancssorból indíthatóan beépíthető

# Megoldás

* hozz létre egy munkamappát
	```shell
	mkdir -p ~/DEV/docker/gui
	cd ~/DEV/docker/gui
	```
* hozz létre grafikus felület inicializátort az alábbi tartalommal **xstartup** néven
	```shell
	#!/bin/sh

	unset SESSION_MANAGER
	unset DBUS_SESSION_BUS_ADDRESS
	OS=`uname -s`
	if [ -x /etc/X11/xinit/xinitrc ]; then
	  exec /etc/X11/xinit/xinitrc
	fi
	if [ -f /etc/X11/xinit/xinitrc ]; then
	  exec sh /etc/X11/xinit/xinitrc
	fi
	[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
	xsetroot -solid grey
	xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
	twm &
	x11vnc --display :0 --forever
	```
* hozzd létre a **Dockerfile**-t az alábbi tartalommal:
	```shell
	FROM fedora:36
	RUN dnf update -y && \
	    dnf install -y firefox \
		xorg-x11-twm \
		tigervnc-server \
		xterm \
		dejavu-sans-fonts \
		dejavu-serif-fonts \
		xdotool \
		x11vnc net-tools && \
		dnf clean all
	RUN mkdir -p /root/.vnc
	ADD ./xstartup /root/.vnc/
	RUN chmod -v +x /root/.vnc/xstartup
	RUN echo 123456 | vncpasswd -f > /root/.vnc/passwd
	RUN sed -i '/\/etc\/X11\/xinit\/xinitrc-common/a [ -x /usr/bin/firefox ] && /usr/bin/firefox &' /etc/X11/xinit/xinitrc
	EXPOSE 5901
	CMD    ["x11vnc", "-create", "-rfbauth", "/root/.vnc/passwd", "-rfbport", "5901" ]
	RUN cat /root/.vnc/passwd
	```

* build-eljük le a konténert
	```shell
	docker build -t hunszasz/gui-firefox .
	```

* futtassuk a konténert
	```shell
	docker run -ti --rm --network host hunszasz/gui-firefox
	```

![docker-run](/assets/docker-run.webp)
![remmina-docker](/assets/remmina-docker.webp)

* remmina segítségével csatlakozhatunk a konténerünkhöz, lokális host-esetén:
	* protokoll: VNC
	* server: 127.18.0.2:1
	* jelszó: 123456

# Hasznos linkek
* [Dockerhub fedora](https://hub.docker.com/_/fedora)
* [Fedora cloud](https://github.com/fedora-cloud/Fedora-Dockerfiles)
* [x11vnc](https://tecadmin.net/how-to-install-x11vnc-server-on-fedora/)
