---
description: >-
  Install Duet Web Controller, and make some modifications to Klipper so it can
  be used.
---

# Installing DWC2

{% hint style="warning" %}
Due to recent changes within Klipper DWC2 no longer requires you to modify Klipper's gcode.py file.
{% endhint %}

```text
sudo systemctl stop klipper
cd ~
PYTHONDIR="${HOME}/klippy-env"
virtualenv ${PYTHONDIR}
${PYTHONDIR}/bin/pip install tornado==5.1.1
```

Clone the DWC2 software and point klipper at it.

```text
git clone https://github.com/Stephan3/dwc2-for-klipper.git
ln -s ~/dwc2-for-klipper/web_dwc2.py ~/klipper/klippy/extras/web_dwc2.py
```

Set up a virtual SD card for klipper to use.

```text
mkdir -p ~/sdcard/dwc2/web
mkdir -p ~/sdcard/sys
cd ~/sdcard/dwc2/web
```

download and install the Duet UI

{$ hint style="warning" $}
The github file used below is an example using the latest version available at the time of this writing.
Be sure to check https://github.com/Stephan3/dwc2-for-klipper for the most up to date version being used by DWC2
{% endhint%}

```text
wget https://github.com/Duet3D/DuetWebControl/releases/download/3.1.1/DuetWebControl-SD.zip
unzip *.zip && for f_ in $(find . | grep '.gz');do gunzip ${f_};done
sudo systemctl start klipper
```
