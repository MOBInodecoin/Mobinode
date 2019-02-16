
Debian
====================
This directory contains files used to package mobinoded/mobinode-qt
for Debian-based Linux systems. If you compile mobinoded/mobinode-qt yourself, there are some useful files here.

## mobinode: URI support ##


mobinode-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install mobinode-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your mobinodeqt binary to `/usr/bin`
and the `../../share/pixmaps/mobinode128.png` to `/usr/share/pixmaps`

mobinode-qt.protocol (KDE)

