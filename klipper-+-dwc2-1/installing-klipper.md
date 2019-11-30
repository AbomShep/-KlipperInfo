---
description: >-
  Almost the same as Kevin's installation instructions, we add some things to
  replace what Octoprint had done by default.
---

# Installing Klipper

{% hint style="info" %}
It's important to read all of the articles in the [/docs folder](https://github.com/KevinOConnor/klipper/tree/master/docs) in the klipper distribution. There's too much information to repeat here, and it's all useful!
{% endhint %}

Put the card in the PI, boot it, and ssh  into the device. 

{% hint style="info" %}
The default username and password for Raspberry Pi is "pi" and "raspberry"
{% endhint %}

{% hint style="info" %}
Your printer's display \(if any\) will not show anything, and you will not be able to connect with your browser just yet.  You need to connect to the Raspbian OS on the PI.
{% endhint %}

Update the  repositories and install some utilities that we'll need going forward.

```text
sudo apt update
sudo apt install git wget gzip tar build-essential libjpeg8-dev imagemagick libv4l-dev cmake -y
```

Clone Klipper onto the card and run the install script.

```text
git clone https://github.com/KevinOConnor/klipper
./klipper/scripts/install-octopi.sh
```

Setup the firmware for the MCU

```text
cd ~/klipper
make menuconfig
##Select the appropriate micro-controller and review any other options provided. Once configured, run:
make clean
make
```

Identify the name of the USB port

```text
ls /dev/serial/by-id/*
```

{% hint style="warning" %}
Remember that the device shown below is an EXAMPLE.
{% endhint %}

It should report something similar to the following:

/dev/serial/by-id/usb-1a86\_USB2.0-Serial-if00-port0

Flash the firmware onto the MCU \(mainboard\) If you are installing to a Smoothie based board such as the new SKR V1.3 skip this and see the next set of instructions.

```text
sudo service klipper stop
make flash FLASH_DEVICE=yourdevicename
sudo service klipper start
```

{% hint style="info" %}
Smoothie based boards such as the new BigTreeTech SKR V1.3 can not be flashed over the USB port the first time Klipper is installed. Instead find the klipper.bin file in the /klipper/out/ directory and copy it to a microSD card as firmware.bin. Check your board's instructions for loading firmware, but this is the general practice:

Place this card into the board's card reader and turn it on without having the USB cable attached. This will load the firmware to the board and may be checked for success by looking at the file on the microSD card which will now read firmware.cfg
{% endhint %}

