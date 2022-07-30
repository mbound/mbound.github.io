---
layout: post
title:  "installing fedora 36 on a mid-2014 macbook pro."
date:   Sat, 30 Jul 2022 20:22:29 +0100
categories:
---

# basic installation.

I am using Fedora 36 Workstation.
Basic installation goes relatively smoothly.

After install, run system update and update all packages.

# getting wifi to work.

Wifi adapter is a Broadcom, the drivers are not included in the Fedora ISO.  
As per this Reddit post:
https://www.reddit.com/r/Fedora/comments/b2grs2/no_wifi_adapter_found/

The drivers can be installed manually with the following:
```
sudo dnf install broadcom-wl akmods kmod-wl akmod-wl
```

Reboot the system and it should work.

# getting the correct cmd behavior

Install Gnome Tweaks.

Under `Keyboard & Mouse > Additional Layout Options > Alt and Win behavior`, 
set to `Ctrl is mapped to Win and the usual Ctrl`

# right click with touch pad bottom right corner.

In Gnome Tweaks, under `Keyboard & Mouse > Mouse Click Emulation` set to `Area`

done.

# ongoing issues.

Suspend seems broken. The laptop suspends just fine but does not wake up. 
To be investigated.