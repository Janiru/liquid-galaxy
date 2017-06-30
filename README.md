# Liquid Galaxy

## Installation

The installation script (`install.sh`) is intended to be used with Ubuntu. It might not work
with other distributions.

Tested with:

- Ubuntu 16.04 LTS
- <strike>Ubuntu 15.10</strike>

### Distribution installation details:

Machine name: lgX (x = screen number*)<br>
Username: lg

\* set them clockwise (i.e. lg4 lg5 lg1 lg2 lg3). lg1 is always the master.

Other details such as the account name or user password are not relevant, write whatever. As a general rule, autologin can be set on the installation, however it is not mandatory since it will be done by the script later.

### Installation script

Get and execute installation file on the target machine (from any user folder):

`bash <(curl -s https://raw.githubusercontent.com/LiquidGalaxyLAB/liquid-galaxy/master/install.sh)`

x64 bits OS might require these additional libraries in order to start Google Earth:

`sudo apt-get install -y libfontconfig1:i386 libx11-6:i386​ libxrender1:i386 libxext6:i386 libglu1-mesa:i386 libglib2.0-0:i386 libsm6:i386`

**Master:**

Machine id: the number that identifies your machine (only the number part of the lgX machine name).<br>
Total machines count: the number of machines running your liquid galaxy.<br>
LG frames: how the final setup will look like (use clockwise order).<br>
Unique number (octet): Unique number that identifies your installation (to avoid conflict with other liquid galaxy installations in your network).

Example filled in form (with a 3 machines setup):<br>
Machine id: 1<br>
Total machines count: 3<br>
LG frames: lg3 lg1 lg2<br>
Unique number (octet): 42

During the installation you will be asked for a SSH passphrase and its verification, just press `enter` twice.

**Slaves:**

Slaves are asked for additional information, in order to sync with the master.

<b>Do NOT install master before having completed its installation and is up and working!</b> (slaves will connect to master to retrieve configuration files during the installation)

Master IP: master machine IP address: `ifconfig eth0 | grep "inet addr" | awk '{print $2}'`.<br>
Master local user password: lg user password.

Example filled in form (with a 3 machines setup):<br>
Machine id: 2<br>
Master machine IP: 192.168.1.10<br>
Master local user password: 1234<br>
Total machines count: 3<br>
LG frames: lg3 lg1 lg2<br>
Unique number (octet): 42

Once the slaves installation has completed (including the reboot), you might have to reload master to send them the init signal. `lg-relaunch`

### Other installation details

- Device GPU drivers might not be enabled by default. Make sure to install your device's latest drivers.
- If your Liquid Galaxy setup is using the screens in vertical, you will have to rotate them manually (on Ubuntu: Launchpad -> Displays -> Rotation -> Counterclockwise).
- In certain devices you might get across a "Clamped Polygons Not Supported" warning. You can hide this warning permanently by editing these files in all Liquid Galaxy machines: ~/earth/config/master/GoogleEarthPlus.conf and ~/earth/config/slave/GoogleEarthPlus.conf ==> paste `MessageEntryList-4_3=fillPolys-disabledByCard` anywhere inside the `[General]` section.

### [Optional] Full-screen

Liquid Galaxy will by default display Earth's menu bar, which is useful for development but not very good looking for demonstrations.

It is up whether to enable it:

`~/tools/earth-fullscreen.sh && sudo reboot`

Full screen uses another window manager ([Openbox](http://openbox.org/wiki/Main_Page)), which does not share Gnome's display settings. If needed, you can rotate your screens by typing the following:

(Openbox tip: use <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>-></kbd> to move onto the next Workspace, then right click on desktop -> terminal emulator)

`xrandr -o left && echo 'xrandr -o left' > ~/.xprofile`

You can disable it anytime:

`~/tools/revert-earth-fullscreen.sh && sudo reboot`


## Within this repository

The "gnu_linux" directory contains example configuration files to aid
with setup of various software pieces. The philosophy employed was
one of letting the underlying distribution simplify the overall setup.

This meant leveraging the default behavior of "Xsession", the 
"alternatives" system, and various hook or "$product.d" directories.

The "php-interface" directory contains an example collection of code
used to provide a touchscreen interface for selecting queries and coordinates
to be consumed by Google Earth. Place everything into a WebServer's Docroot.

## On the system

NOTE: with SVN, it is up to the client to handle symlinks. If your client
doesn't handle symlinks, you might end up with some duplicate files with
various names, but things should still work fine.

The following tree represents the suggested directory hierarchy 
within the "lg" user's home directory:

```
/home/lg
|-- bin
|-- earth
|   |-- builds
|   |   |-- latest -> ./5.2.X.XXXX-X
|   |   `-- 5.2.X.XXXX-X
|   |-- config
|   |   |-- master
|   |   `-- slave
|   `-- scripts
|-- etc
`-- media
    `-- images
        |-- backgrounds
        `-- google
```

Using example scripts and tools within this repo, there should also be a "/lg"
directory on the system owned by "lg" user.
