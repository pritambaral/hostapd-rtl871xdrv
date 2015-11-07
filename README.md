## What is this?
----

AP mode (via hostapd) support for wifi chips that identify themselves as **RTL8188CUS** (or variants thereof). If you've seen or heard someone claim that a certain wifi chip works with a special version of hostapd driver called `rtl871xdrv`, this is it.

This is not the full hostapd source however. These are just patches and extra code to compile `rtl871xdrv` with a fresh hostapd source.

## Why?
----

Wifi chips like **RT8188C** and **RT8192C**, recognized as **RTL8188CUS** (or variants thereof) do not support the standard nl80211 driver of hostapd. Their in-kernel drivers are also buggy and not very featureful. Realtek released their drivers for such chips under the GPL, but never bothered to update them. But I guess a one-time code dump is better than no source code at all.

Realtek's code dump contained a kernel driver and a special driver for hostapd. (What is this? 2005?) As of writing, the kernel driver is being maintained for modern kernels at [dz0ny/rt8192cu](https://github.com/dz0ny/rt8192cu).

The hostapd driver however, wasn't provided as a driver. A modified hostapd, somewhere in its 0.8.x version, was dumped. If you wanted a newer hostapd, well, too bad.

This repo has the extracted modifications to mainline hostapd done by Realtek, adapted for hostapd 2.5. For other versions of hostapd, checkout to the corresponding tag.

## Installation
----

Get a fresh copy of hostapd from http://hostap.epitest.fi/hostapd/ or through your distro's methods.
Inside the directory that contains `hostapd`, `src`, `wpa_supplicant`, apply the patch:
```
$ patch -Np1 -i </path/to/rtl871xdrv.patch>
```

Make sure you enable the driver in `.config`.

If you don't know what `.config` is:
```
$ cd hostapd
$ cp defconfig .config
$ echo CONFIG_DRIVER_RTW=y >> .config
```
Credits: [oblique](https://github.com/pritambaral/hostapd-rtl871xdrv/pull/3#issuecomment-76276806)

Now you're all set to compile. Use your distro's build system, if set up, to compile and package and everything.

For simple compiling: run `make` inside the 'hostapd' folder.

If `make` succeeds, you'll have two binaries in the same directory: `hostapd` and `hostapd_cli`. The former is the actual AP daemon, the latter is just a helper for controlling a running daemon.

A simple `hostapd.conf` file has been provided.

## Why only hostapd, no wpa_supplicant?

At the time of releasing this, the mainline driver worked well enough for STA mode operation. The kernel driver from Realtek was only needed for AP mode.

## LICENSE
----
No code here has been written by me, only existing code from Realtek has been modified. As per the original license:

```
This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License version 2 as
published by the Free Software Foundation.

Alternatively, this software may be distributed under the terms of BSD
license.
```
