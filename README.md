# Gentoo + DWL = ♥
I finally took the time to create this page which describes my 2 biggest loves (besides my girlfriend and my cat): Gentoo and DWL.  I have been using Gentoo exclusively on my dekstop since 2004 and experimented with it when it was still called Enoch.
Because my laptop is used a lot less than my desktop I never bothered with installing Gentoo on it at first.

I decided to go for a rice with a Gentoo feeling to it.  I'm using DWL from https://codeberg.org/dwl/dwl which currently is ```dwl v0.8-dev-73-gab4cb6e-dirty```.

As of August 2025 dwl is no longer maintained :( 

## Dotfiles.tar
The dotfiles.tar file contains:
- ds: the binary to start dwl.  I put it in /usr/bin and simply boot to console. It's short for "Dwl Start" :)  Its only content is ```dwl -s .local/src/dwl-startup.sh```.
- dwl.tar: my entire ~/.local/src/dwl folder with all applied patches and my own modified config.h
- dwl-startup.sh: I also put this in ~/.local/src.  This script contains everything that needs to be started with DWL.  Yes I'm well aware about an autostart patch but I prefer this.
- dunst, kitty, waybar and wofi are folders with my configs that could be placed in ~/.config.
- gentoo_alien.jpg is my wallpaper

## dwl-startup.sh
This script is passed on to dwl via the -s parameter in ```ds```.  It contains everything that DWL should auto-start for me.
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

## Auto-mount USB devices like external hard drives
I prefer my USB connected external hard drives to be mounted automatically. For this I installed udisks2 which is then enabled at boot via ```systemctl enable --now udisks2.service```. As for automount I installed udiskie which is autostarted with ```udiskie --tray &``` in my dwl-startup.sh script.  External USB devices will now be auto-mounted and accessible via Thunar.  Udiskie in traymode is started via ```ds```.

## Patches
DWL, by default, is very spartan.  But its functionalities can be extended with the patches from https://codeberg.org/dwl/dwl-patches.  I only applied the following patches:
* gaps.patch from https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/gaps because I prefer small gaps between windows.
* focusdir.patch from https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/focusdir because I was used to that from i3.
* ipc.patch from https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/ipc because this provides an ipc for wayland clients to get and set DWL state. The ipc is intended for status bars like Waybar like I'm using.

## How to patch
From the ~/.local/src/dwl folder, enter the command ```patch -p1 < /path/to/filename.patch```.  Then ```make``` and finally ```sudo make install clean```.  Login in DWL again to see the changes.
You can check beforehand with ```patch -p1 --dry-run < /path/to/filename.patch```.

## DWL config modifications
DWL's configuration can be adapted by changing config.h and config.mk files in the ~/.local/src/dwl folder.  Modifying the C-code can be challenging to begin with. My changes and all keybinds are included in dotfiles.tar.

## config.h
As you might know DWL is configured through modifying the config.h file and recompîling DWL's source code.  The following modifications were made.  This is a snippet of my config.h.  Keep in mind that I'm using a Belgian AZERTY keyboard.
```
static const struct xkb_rule_names xkb_rules = {
	/* can specify fields: rules, model, layout, variant, options */
	/* example:
	.options = "ctrl:nocaps",
	*/
	.layout = "be",
};

...etc...

static const char *screenshotcmd[] = { "grim", NULL };
static const char *lockcmd[] = { "swaylock", "--screenshots", "--clock", "--effect-blur", "7x5", "-f", NULL };
static const char *hibernatecmd[] = { "systemctl", "hibernate", NULL};
static const char *suspendcmd[] = { "systemctl", "suspend", NULL};

...etc...

        { MODKEY,                    XKB_KEY_Print,	spawn,          {.v = screenshotcmd} },
        { WLR_MODIFIER_CTRL|WLR_MODIFIER_ALT,            XKB_KEY_l,          spawn,          {.v = lockcmd} },
	{ MODKEY|WLR_MODIFIER_CTRL,  XKB_KEY_h,          spawn,          {.v = hibernatecmd} },
        { MODKEY|WLR_MODIFIER_CTRL,  XKB_KEY_s,          spawn,          {.v = suspendcmd} },

...etc...

         TAGKEYS(          XKB_KEY_ampersand, XKB_KEY_1,                     0),
	TAGKEYS(          XKB_KEY_eacute, XKB_KEY_2,                         1),
	TAGKEYS(          XKB_KEY_quotedbl, XKB_KEY_3,                 2),
	TAGKEYS(          XKB_KEY_apostrophe, XKB_KEY_dollar,                     3),
	TAGKEYS(          XKB_KEY_parenleft, XKB_KEY_percent,                    4),
	TAGKEYS(          XKB_KEY_section, XKB_KEY_asciicircum,                5),
	TAGKEYS(          XKB_KEY_egrave, XKB_KEY_ampersand,                  6),
	TAGKEYS(          XKB_KEY_exclam, XKB_KEY_asterisk,                   7),
	TAGKEYS(          XKB_KEY_ccedilla, XKB_KEY_parenleft,                  8),

```

## Screenshots
![Clean desktop.](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/clean.png)

![Wofi](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/wofi.png)

![Fake busy](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/fake_busy.png)

![Fake busy 2](https://github.com/D4rkOnE/Gentoo-DWL-laptop/blob/main/fake_busy2.png)

