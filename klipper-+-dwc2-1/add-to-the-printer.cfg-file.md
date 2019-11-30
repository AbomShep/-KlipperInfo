---
description: Last step! (maybe)
---

# Add to the printer.cfg file

{% hint style="warning" %}
\*\*For Trinamic Drivers\*\*

You'll need to add an additional section to your printer.cfg file for each stepper that is using a Trinamic Driver. Currently, Klipper can use TMC2130, 2208, 2224 and 2660 drivers. Follow the  directions at [https://github.com/KevinOConnor/klipper/blob/master/config/example-extras.cfg](https://github.com/KevinOConnor/klipper/blob/master/config/example-extras.cfg)
{% endhint %}

If you are installing klipper for the first time, and have never used it, you will need to set a printer.cfg file that will tell klipper everything it needs to run on your machine. This is an excerpt from /doc/installation.md:

> The Klipper configuration is stored in a text file on the Raspberry Pi. Take a look at the example config files in the [config directory](https://github.com/KevinOConnor/klipper/blob/master/config). The [example.cfg](https://github.com/KevinOConnor/klipper/blob/master/config/example.cfg) file contains documentation on command parameters and it can also be used as an initial config file template. However, for most printers, one of the other config files may be a more concise starting point.
>
> Arguably the easiest way to update the Klipper configuration file is to use a desktop editor that supports editing files over the "scp" and/or "sftp" protocols. There are freely available tools that support this \(eg, Notepad++, WinSCP, and Cyberduck\). Use one of the example config files as a starting point and save it as a file named "printer.cfg" in the home directory of the pi user \(ie, /home/pi/printer.cfg\).
>
> Alternatively, one can also copy and edit the file directly on the Raspberry Pi via ssh - for example:
>
> ```text
> cp ~/klipper/config/example.cfg ~/printer.cfg
> nano ~/printer.cfg
> ```
>
> Make sure to review and update each setting that is appropriate for the hardware.

Once you have your printer.cfg file created and added to the home directory, add the following to your file.

```text
[virtual_sdcard]
path: /home/pi/sdcard

[web_dwc2]
## optional - defaulting to Klipper
printer_name: Reiner Calmund
# optional - defaulting to 0.0.0.0
listen_adress: 0.0.0.0
# needed - use above 1024 as nonroot
listen_port: 4750
# optional defaulting to dwc2/web. Its a folder relative to your virtual sdcard.
web_path: dwc2/web
```

{% hint style="info" %}
Connect to your UI by directing your browser to the IP address of your Raspberry PI with the port number added to the end ex. 192.168.2.3:4750.  Common reasons for your connection to be refused are a missing/incorrectly placed printer.cfg file, or a typo somewhere in the file.  Check /tmp/klippy.log for hints as to the problem.
{% endhint %}

