---
description: >-
  Install Duet Web Controller, and make some modifications to Klipper so it can
  be used.
---

# Installing DWC2

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

 For now, we need to make some changes in klipper to make it work with DWC2. Eventually, this will not be necessary.

{% hint style="warning" %}
Note that we're making a small change to Klipper with these lines. That means that when ever you upgrade Klipper, you'll need to do this step again to keep DWC2 working.
{% endhint %}

```text
gcode=$(sed 's/self.bytes_read = 0/self.bytes_read = 0\n        self.respond_callbacks = []/g' klipper/klippy/gcode.py)
gcode=$(echo "$gcode" | sed 's/# Response handling/def register_respond_callback(self, callback):\n        self.respond_callbacks.append(callback)/')
gcode=$(echo "$gcode" | sed 's/os.write(self.fd, msg+"\\n")/os.write(self.fd, msg+"\\n")\n            for callback in self.respond_callbacks:\n                callback(msg+"\\n")/')
echo "$gcode" > klipper/klippy/gcode.py
```

  Set up a virtual SD card for klipper to use.

```text
mkdir -p ~/sdcard/dwc2/web
cd ~/sdcard/dwc2/web 
```

download and install the Duet UI

```text
wget https://github.com/chrishamm/DuetWebControl/releases/download/2.0.0-RC5/DuetWebControl.zip
unzip *.zip && for f_ in $(find . | grep '.gz');do gunzip ${f_};done
sudo systemctl start klipper
```

