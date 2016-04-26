Install Qemu and Expect
============

Install the necessary packages:

```
    sudo apt-get install qemu qemu-user-static expect
```

Create a new image from scratch
===============================

* Download the most recent Raspbian version:
    http://downloads.raspberrypi.org/raspbian_latest

* Unpack it:

```
    unzip 2015-05-05-raspbian-wheezy.zip
    mv 2015-05-05-raspbian-wheezy.zip raspbian-wheezy.zip
```

Prepare the image
=================

It will be used for the build environment and the final image.

* [Add empty space to the image](resize_img.md)

* Chroot in the image

```
    sudo ./proper_chroot.sh
```

* Change your user to root (your global variables may be broken)

```
    su root
```

* The locales may be broken, fix it (remove `en_GB.UTF-8 UTF-8`, set `en_US.UTF-8 UTF-8`):

```
    dpkg-reconfigure locales
```

* In the image, make sure everything is up-to-date, and remove the old packages

```
    apt-get update
    apt-get dist-upgrade
    apt-get autoremove
    apt-get install git p7zip-full python-dev python-pip python-lxml pmount libjpeg-dev libtiff-dev libwebp-dev liblcms2-dev tcl-dev tk-dev python-tk
```

* Install python requirements

```
    pip install oletools olefile officedissector exifread Pillow
    pip install git+https://github.com/CIRCL/PyCIRCLean.git
```

* Create the user and mtab for a RO filesystem

```
    useradd -m kitten
    chown -R kitten:kitten /home/kitten
    ln -s /proc/mounts /etc/mtab
```

* Copy the files

```
    sudo ./copy_to_final.sh /mnt/arm_rPi/
```

* Enable rc.local

```
    systemctl enable rc-local.service
```
