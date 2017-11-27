# rtl8812au-5.2.9

it's a cloned repo from Gordboy. She or he is no longer in Github. All credits
go to him or she.

## Realtek 8812AU driver version 5.2.9.3

Works fine with Ubuntu 17.10 Artful 4.13 kernel, and now 4.14 kernel. All flavours of vfs_read now replaced for kernel >= 4.14.

Only support 8812AU. Now has every known (to me) device USB ID, sorted by ID number.

Source builds with no warnings or errors, and is very stable in use. Realtek seem to have done a decent job here.

Added (cosmeticly edited) original Realtek_Changelog.txt, this README.md and dkms.conf.

Removed debian package - too many commits and rebuilds

Added LEDcontrol by Makefile, module parameter and dynamic /proc writing

### Building

To build and install module manually:
```sh
$ make
$ sudo make install
```

To use dkms install:

```sh
  (as root, or sudo) copy source folder contents to /usr/src/rtl8812au-5.2.9
```

```sh
$ sudo dkms add -m rtl8812au -v 5.2.9
$ sudo dkms build -m rtl8812au -v 5.2.9
$ sudo dkms install -m rtl8812au -v 5.2.9 
```

To use dkms uninstall and remove:

```sh
$ sudo dkms remove -m rtl8812au -v 5.2.9 --all
```

### LED control

Thanks to @dkadioglu and others for a start on this.

#### You can now control LED behaviour statically by Makefile, for example:

```sh
CONFIG_LED_ENABLE = n
```
value can be y or n

#### statically by module parameter in /etc/modprobe.d/8812au.conf or wherever, for example:

```sh
options 8812au rtw_led_enable=0
```
value can be 0 or 1

#### or dynamically by writing to /proc/net/rtl8812au/$(your interface name)/led_enable, for example:

```sh
$ echo "0" > /proc/net/rtl8812au/$(your interface name)/led_enable
```
value can be 0 or 1

#### check current value:

```sh
$ cat /proc/net/rtl8812au/$(your interface name)/led_enable
```

### NetworkManager

As others have noted, people using NetworkManager need to add this stanza to /etc/NetworkManager/NetworkManager.conf

```sh
  [device]
  wifi.scan-rand-mac-address=no
```
