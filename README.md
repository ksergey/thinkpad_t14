# ArchLinux on Thinkpad T14 gen 1

## Installation

Just use ArchLinux [installation guide](https://wiki.archlinux.org/index.php/installation_guide)

## Fix WiFi

Create `/etc/modprobe.d/iwlwifi.conf` file:
```
options iwlwifi 11n_disable=8
```

Done!

## Make systemd happy

Create `/etc/modprobe.d/blacklist.conf` file:
```
blacklist snd_pci_acp3x
blacklist sp5100_tco
```

Done!

## Fingerprint sensor

Boot into BIOS and clear stored fingerprints.

Install latest firmware for "Synaptics Prometheus Fingerprint Reader":
```
fwupdmgr enable-remote lvfs-testing
fwupdmgr get-devices
fwupdmgr refresh
fwupdmgr get-updates
fwupdmgr update
fwupdmgr disable-remote lvfs-testing
```

Install `fprintd`:
```
pacman -S fprintd
```

Train your sensor:
```
fprintd-enroll
```

If you use lightdm insert `auth sufficient pam_fprintd.so` into `/etc/pam.d/lightdm`.
```
#%PAM-1.0

auth        sufficient  pam_fprintd.so
auth        include     system-login
-auth       optional    pam_gnome_keyring.so
account     include     system-login
password    include     system-login
session     include     system-login
-session    optional    pam_gnome_keyring.so auto_start
```

Done! Now you can login via lightdm without password.

More details on [ArchLinux wiki](https://wiki.archlinux.org/index.php/fprint)

## Smartcard reader

Working from the box, follow [instrunction](https://wiki.archlinux.org/index.php/Smartcards)

## Fix issue with $HOME/.config/locale.conf

In my case xfce4 or lightdm ignores everything in `$HOME/.config/locale.conf`. To fix this add into `/etc/environment`
line `LC_ALL=`:

My `/etc/environment` content:
```
#
# This file is parsed by pam_env module
#
# Syntax: simple "KEY=VAL" pairs on separate lines
#
LANG=
```

Done!

I don't know who overrides `$LANG` variable before execute `/etc/profile.d/locale.sh`, but trick above fix that.

## Troubleshooting

#### USB-A ports on both sides stopped working

Shutting down laptop and battery reset via small pinhole did the trick - USB A ports are now operational ([source](https://www.reddit.com/r/thinkpad/comments/mfvo39/t14_gen_1_amd_usba_ports_on_both_sides_just/))
