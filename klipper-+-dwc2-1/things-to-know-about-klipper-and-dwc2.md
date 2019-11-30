---
description: >-
  The developer has done an extensive job of documenting the features of Klipper
  in the /docs directory. We recommend that this be read thoroughly.
---

# Things to Know about Klipper and DWC2

{% hint style="info" %}
There's a plugin for Cura that will allow you to print directly to DWC2, or to upload to it. You can find it for Cura 4.0  In the marketplace.  And for Cura 3.6 at  [https://github.com/Kriechi/Cura-DuetRRFPlugin/releases/tag/v1.0.2](https://github.com/Kriechi/Cura-DuetRRFPlugin/releases/tag/v1.0.2)

This is based off of the Duet Web Controller. Not everything will be the same, and remember this port is not complete yet, but there's the original manual to help you figure out how it works. [https://duet3d.dozuki.com/Wiki/Duet\_Web\_Control\_Manual](https://duet3d.dozuki.com/Wiki/Duet_Web_Control_Manual)

Klipper messages are found in the G-code Console, and are highlighted in yellow.

DWC2 can be installed alongside a pre-existing Octoprint installation. You could use either one interchangeably. 

Keep in mind that if you are updating a previous Klipper installation on your R Pi while doing this, you will probably have to re-flash the firmware on the printer as well, just as shown in the instructions.

Also you may be in the habit of doing an apt-update and upgrade with Linux. Our experience is that this can load a lot of unnecessary software and in general just mess things up. Just stick to updating Klipper, DWC2 and DWC when they release an update that has something you want. Unless it's adding something that you want to have or is fixing a bug you don't even have to do updates.

Check the main page for each to see what has been updated:

[https://github.com/KevinOConnor/klipper](https://github.com/KevinOConnor/klipper)

[https://github.com/Stephan3/dwc2-for-klipper](https://github.com/Stephan3/dwc2-for-klipper)

[https://github.com/chrishamm/DuetWebControl](https://github.com/chrishamm/DuetWebControl)
{% endhint %}

