# Gentoo + DWL = ♥
I finally took the time to create this page which describes my 2 biggest loves (besides my girlfriend and my cat): Gentoo and DWL.  I'm fully aware my other repositories are about Archlinux which isn't a bad distribution at all, but I have been using Gentoo exclusively on my dekstop since 2004 and experimented with it when it was still called Enoch.
Because my laptop is used a lot less than my desktop I never bothered with installing Gentoo on it at first.

I decided to go for a rice with a Gentoo feeling to it.  I'm using DWL from https://codeberg.org/dwl/dwl which currently is ```dwl v0.8-dev-73-gab4cb6e-dirty```.

## Dotfiles.tar
The dotfiles.tar file contains:
- ds: the binary to start dwl.  I put it in /usr/bin and simply boot to console. It's short for "Dwl Start" :)  Its only content is ```dwl -s .local/src/dwl-startup.sh```.
- dwl.tar: my entire ~/.local/src/dwl folder with all applied patches and my own modified config.h
- dwl-startup.sh: I also put this in ~/.local/src.  This script contains everything that needs to be started with DWL.  Yes I'm well aware about an autostart patch but I prefer this.
- dunst, kitty, waybar and wofi are folders with my configs that could be placed in ~/.config.
- gentoo_alien.jpg is my wallpaper

## dwl-startup.sh
This script is passed on to dwl via de -s parameter in ```ds```.  It contains everything that DWL should auto-start for me.
```
#!/bin/bash
waybar &
swaybg --image ~/Downloads/gentoo_alien.jpg &
wl-clip-persist --clipboard regular &
wl-paste --type text --watch cliphist store &
wl-paste --type image --watch cliphist store &
dunst &
udiskie --tray &
swayidle -w timeout 300 'swaylock --screenshots --clock --effect-blur 7x5 -f' &
exec /usr/libexec/polkit-gnome-authentication-agent-1
```

## Screenshots
![Clean desktop.](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/clean.png)

![Wofi](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/wofi.png)

![Fake busy](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/fake_busy.png)

![Fake busy 2](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/fake_busy2.png)

