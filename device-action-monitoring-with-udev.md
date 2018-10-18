How to run a script on connecting USB device
================================================

This tutorial explains how you could run a script made by you (say /usr/local/my_script) when you plug a specific USB device.

1. First run `udevadm monitor --property` while plugin or remove device from your machine
(KERNEL lines are skipped and only last UDEV remove is attached below)

```bash
sudo udevadm monitor --property

...
UDEV  [278963.795670] remove   /devices/pci0000:00/0000:00:14.0/usb1/1-10 (usb)
ACTION=remove
BUSNUM=001
DEVNAME=/dev/bus/usb/001/058
DEVNUM=058
DEVPATH=/devices/pci0000:00/0000:00:14.0/usb1/1-10
DEVTYPE=usb_device
MAJOR=189
MINOR=57
PRODUCT=1871/101/c
SEQNUM=20934
SUBSYSTEM=usb
TYPE=239/2/1
USEC_INITIALIZED=278963771208
ID_MODEL=USB2.0_Camera
```
2. Run `ls /etc/udev/rules.d -lh` to list all of udev rule files

```bash
ls /etc/udev/rules.d -lh

...
-rw-r--r-- 1 root root 747 Aug 28 10:06 60-vboxdrv.rules
-rw-r--r-- 1 root root 58K Jul  6 01:22 70-snap.core.rules
-rw-r--r-- 1 root root 400 Oct 18 09:06 70-snap.kdictionary.rules
-rw-r--r-- 1 root root 370 Oct 18 09:06 70-snap.micropad.rules
-rw-r--r-- 1 root root 340 Oct 18 09:06 70-snap.notes.rules
-rw-r--r-- 1 root root 520 Oct 18 09:06 70-snap.simplescreenrecorder-mardy.rules
-rw-r--r-- 1 root root 630 Jul  6 12:57 97-usbboot.rules
```

3. From above I've used Environment `ID_MODEL=USB2.0_Camera` under my udev rules for action `ACTION=="add"` and `ACTION==remove`.

  As reference from [here](http://reactivated.net/writing_udev_rules.html#files) for rule writing,
  `files in /etc/udev/rules.d/ are parsed in lexical order`;
  so I want my own rules to be parsed after the defaults I put my rules under file `85-aicampj.usb_cameras.rules`
  in order to run some scripts when usb camera is connected/disconnected to/from the system for example.
  
  ```bash
  sudo vim /etc/udev/rules.d/85-aicampj.usb_cameras.rules
  
  ...
  # /etc/udev/rules.d/85-aicampj.usb_cameras.rules/etc/udev/rules.d/85-aicampj.usb_cameras.rules
  ACTION=="add", ENV{ID_MODEL}=="USB2.0_Camera", RUN+="/usr/bin/touch /home/tuanlm/Desktop/udev/camera_is_conected.txt"
  ACTION=="remove", ENV{ID_MODEL}=="USB2.0_Camera", RUN+="/bin/rm -rf /home/tuanlm/Desktop/udev/camera_is_conected.txt"
  ```
4. Run command `sudo udevadm control --reload-rules && udevadm trigger` for udev to reload with new rules.
5. Finally, open file manager to check whether if the file is created or deleted while your usb device connect/disconect to/from the system.
  This is just simple rule file to demonstrate how to use udev rules. The `RUN` option could be path to script file or a set of system commands.
  
  
