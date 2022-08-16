<img src="https://user-images.githubusercontent.com/50086969/185001336-4c43ea15-38f6-4d10-a93b-20a59de1cf58.png" alt="dwm"/>

# dwm - dynamic window manager

dwm is an extremely fast, small, and dynamic window manager for X.

## Table of Contents

- [dwm - dynamic window manager](#dwm---dynamic-window-manager)
  - [Requirements](#requirements)
  - [Installation](#installation)
  - [Running dwm](#running-dwm)
  - [Command Shortcuts](#command-shortcuts)
  - [Configuration](#configuration)

---

<img src="https://user-images.githubusercontent.com/50086969/185001345-ca086ba7-c416-44b6-bfd0-e14f574e82ac.png?raw=true" alt="dwm screenshot"/>

## Requirements

In order to build dwm you need the Xlib header files.

## Installation

By default, dwm is installed into the /usr/local namespace. If this does not match your local setup, edit config.mk accordingly.

Afterwards, enter the following command to build and install dwm (if
necessary as root):

    make clean install

## Running dwm

Add the following line to your .xinitrc / .xprofile to start dwm using startx:

    exec dwm

In order to connect dwm to a specific display, make sure that
the DISPLAY environment variable is set correctly, e.g.:

    DISPLAY=foo.bar:1 exec dwm

(This will start dwm on display :1 of the host foo.bar.)

In order to display status info in the bar, you can do something
like this in your .xinitrc / .xprofile:

    while xsetroot -name "`date` `uptime | sed 's/.*,//'`"
    do
    	sleep 1
    done &
    exec dwm

(Using this with another status bar, e.g., [slstatus](https://tools.suckless.org/slstatus/), will cause issues)

## Command Shortcuts

Here are the default shortcuts configured in this build.

`MODKEY` = **Super / Command / Windows key**. If you want to change this to a different modifier key, you may do so by changing the `MODKEY` in config.h. There are up to 5 modifier keys available (Mod1Mask through Mod5Mask) depending on your system. Simply run `xmodmap` on your system to see which options you have available.

| Shortcut | Description |
| -------- | ----------- |
| `MODKEY+B` | Toggle the top bar. |
| `MODKEY+C` | Terminate the active window. |
| `MODKEY+F` | Activate floating view mode (also expands the active window to full-screen). |
| `MODKEY+M` | Activate monocle view mode. |
| `MODKEY+P` | Open dmenu (application launcher). |
| `MODKEY+T` | Activate tile view mode. |
| `MODKEY+[1-9]` | Switch between workspaces. You can also use your mouse by clicking on a workspace's tab. |
| `MODKEY+0` | View all workspaces in floating view mode (stacked windows). |
| `MODKEY+ENTER` | Swap positions between two open windows on a workspace. |
| `MODKEY+SHIFT+L` | Lock the screen (requires [light-locker](https://github.com/the-cavalry/light-locker)). |
| `MODKEY+SHIFT+Q` | Quit the session / logout. |
| `MODKEY+SHIFT+[1-9]` | Move the active window between workspaces. |
| `MODKEY+SHIFT+0` | Tag the active window to remain in view regardless of what workspace you switch to. |
| `MODKEY+SHIFT+ENTER`  | Open a new terminal window.                                  |
| `MODKEY+CTRL+[1-9]` | Tag the selected workspace's windows to come into view with the active workspace. |
| `MODKEY+TAB` | Toggle between the currently active workspace and the previously selected workspace. |
| `MODKEY+SPACEBAR` | Toggle floating view mode. |
| Volume Up/Down | Adjusts the system volume. |
| Brightness Up/Down | Adjusts the system brightness. Requires xbacklight ([acpilight](https://gitlab.com/wavexx/acpilight) provides this, and is highly recommended). |

> **Note:** Hovering your mouse over a window makes that window active.

## Configuration

My config uses the [Hack Nerd Font](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Hack) with the following [Suckless](https;//suckless.org) patches:

- [dmenu](https://tools.suckless.org/dmenu/)
- [vanitygaps](https://dwm.suckless.org/patches/vanitygaps/)
- [systray](https://dwm.suckless.org/patches/systray/)

The configuration of dwm is done by modifying [config.h](/config.h) and (re)compiling the source code. This keeps it fast, secure and simple. It is recommended to use a text editor such as Vim, nano, or Emacs when you edit config.h (using another utility, e.g., an IDE, can cause issues with seeing glyphs, for example).

> **Note:** Do not modify the [config.def.h](/config.def.h) file. Instead, use this file as a state backup of the original working config in case you ever need to revert back to it.

Here are the respective default variables you'll want to edit for your system:

| constant | Description |
| -------- | ----------- |
| `static Key keys[]` | Contains an array of keyboard shortcuts for invoking commands on your system. Below are the parameters for each value in the array.<br><br>**modifier** - define the keys that should be prefixed before the command invocation key. This can be `0` (for none), or any combination of *MODKEY* (default is `ALT`) and `ShiftMask` (`SHIFT`).<br><br>**key** - the command invocation key. Define a key that, when pressed with the corresponding modifier key(s), invokes the command (e.g., launches an application). By default, this maps to `/usr/include/X11/keysymdef.h` and/or `/usr/include/X11/XF86keysym.h` on most systems.<br /><br />**function** - the function to invoke from [dwm.c](/dwm.c) when the corresponding keyboard shortcut is pressed (e.g., *spawn* calls [execvp](https://man7.org/linux/man-pages/man3/exec.3.html) to execute a file/program).<br /><br />**argument** - the argument to be passed into the aforementioned function. |
| `static const char *tags[][TAGLENGTH]` | This is where you'll set the unicode values (e.g., glyphs, letters, numbers, or special characters) for your workspaces. By default, these appear at the top left of the screen. |
| `static const Rule rules[]` | Contains an array of rules for individual apps. Below are the parameters for each value in the array. <br><br>**tags mask**  - tells dwm to send an application to a specific workspace when it's opened (1 << x = 1 + x; therefore, 1 << 3 = workspace 4).<br><br>**isfloating** - is a boolean (`0 \| 1`) that tells dwm to treat the app's windows as floating windows if true (`1`), else tiled if false (`0`).<br><br>**monitor**    - tells dwm which monitor to send the app's windows to.<br><br>**title**, **instance**, and **class** - Use the following command to get the apropriate arguments: `xprop \| awk '/^WM_CLASS/{sub(/.* =/, "instance:"); sub(/,/, "\nclass:"); print}/^WM_NAME/{sub(/.* =/, "title:"); print}'`. After entering the command into the terminal, hover the mouse over the corresponding app's window and click to generate the output.<br><sub>(See [dwm rules](https://dwm.suckless.org/customisation/rules/) for more information)</sub> |
| `static const char *termcmd[]` | Defines the terminal emulator to use on the system. Default is Alacritty. |
| `static const char *browsercmd[]` | Defines the web browser to use on the system. Default is Brave. |

<sub>See more from suckless at [suckless.org](https://suckless.org/)</sub>