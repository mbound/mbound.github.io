---
layout: post
title:  "installing linux on a mid-2014 macbook pro."
date:   Sat, 30 Jul 2022 20:22:29 +0100
categories:
---

This describes my experiment with getting linux to run with an as much as possible "out-of-the-box" experience on a mid-2014 Apple MacBook Pro 15" (EMC 2876).

I am usually an Arch user, but I deliberately decided against it because:
1) I don't have much time lately, and this is a laptop I use for work, so I wanted to spend as little time possible setting the system up (I had to replace the SSD and re-install the OS);
2) I wanted to see how much of an out-of-the-box experience I could get with Linux on a macbook (ricing and customizations can come later).

My initial experiment was with Fedora 36, as I've seen it touted as "the new user friendly dekstop distro"
> tl;dr - i gave up with fedora 36 and switched to ubuntu 22.04 which has a much better out of the box experience with Macbook

# fedora 36
## basic installation.

I am using Fedora 36 Workstation.
Basic installation goes relatively smoothly.

After install, run system update and update all packages.

## getting wifi to work.

Wifi adapter is a Broadcom, the drivers are not included in the Fedora ISO.  
As per this Reddit post:
https://www.reddit.com/r/Fedora/comments/b2grs2/no_wifi_adapter_found/

The drivers can be installed manually with the following:
```
sudo dnf install broadcom-wl akmods kmod-wl akmod-wl
```

Reboot the system and it should work.

## getting the correct cmd behavior

Install Gnome Tweaks.

Under `Keyboard & Mouse > Additional Layout Options > Alt and Win behavior`, 
set to `Ctrl is mapped to Win and the usual Ctrl`

## right click with touch pad bottom right corner.

In Gnome Tweaks, under `Keyboard & Mouse > Mouse Click Emulation` set to `Area`

done.

## suspend and ongoing issues.

Suspend seems broken. The laptop suspends just fine but does not wake up. 
I have tried several workarounds.  This one listed below with disabling `d3cold` state for the NVMe drive seemed the most promising:
https://github.com/Dunedan/mbp-2016-linux/issues/37

```
echo 0 > /sys/bus/pci/devices/0000\:04\:00.0/d3cold_allowed
```
However, the laptop does not seem to wake up.
I had more luck by installing a low-latency XanMod kernel alongside with disabling `d3cold` state, and with this configuration the laptop comes back to sleep in 3-4 seconds.  However, XanMod `xm1` kernel breaks Wifi and a bunch of much more serious things around input, display and mouse behaviour which basically makes windows unclickable and the internal touchpad only moves vertically, all the gestures stopped working.

Gave up on Fedora 36 for now.


# ubuntu 22.04 desktop (jammy jellyfish)

Installed with the default USB live GUI installer.
Partitioning is much more clunky than Fedora, which has a very nice partitioning wizard.
So far, the best out-of-the-box experience:
- Wifi drivers are easy to enable with standard Ubuntu `Additional Drivers` tool after installation;
- Suspend works perfect out of the box, no issues at all;
- Gnome customizations are the same as for Fedora 36 (both use Gnome 42)
- Auto-switch between light and dark theme mode can be neable with Night Theme Switcher Gnome Extension (installed via Extension Manager application);
- Webcam requires custom compiled drivers/firmware, but seems to work well.

## webcam drivers
Got it working with this procedure:
https://askubuntu.com/questions/1385307/how-to-install-a-driver-for-a-webcam-on-macbook-pro-13-mid-2014-using-ubuntu

Install deps:
```
sudo apt install git debhelper dkms build-essential fakeroot cpio curl xz-utils
```

Clone custom drivers and firmware:
```
mkdir -p work/facetimehd
cd work/facetimehd
git clone https://github.com/whitty/facetimehd
git clone https://github.com/patjak/facetimehd-firmware
```
Compile deb packages:
```
make -C facetimehd-firmware/ deb 
cp facetimehd-firmware/debian/*.deb .
cd facetimehd/
dpkg-buildpackage -us -uc
cd ..
```
Install packages:
```
sudo -s
apt install ./facetimehd*.deb
```
Reboot.
Enjoy.
