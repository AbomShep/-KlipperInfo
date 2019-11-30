# BLtouch Tips



{% hint style="info" %}
If you are using a clone of the Bltouch you  may have some issues using it due to small differences  in the way it operates from the original. The following 2 additions to your printer.cfg file should help: 

1. pin\_up\_reports\_not\_triggered: False

   \#Set if the BLTouch consistently reports the probe in a "not triggered" state after a successful "pin\_up" command. This should be True for a genuine BLTouch; some BLTouch clones may require False.  The default is True.

2. pin\_up\_touch\_mode\_reports\_triggered: False

   \#Set if the BLTouch consistently reports a "triggered" state after the commands "pin\_up" followed by "touch\_mode". This should be True for a genuine BLTouch; some BLTouch clones may require False. The default is True.
{% endhint %}

{% hint style="danger" %}
The latest version of the Bltouch, V3 has a problem with some main boards requiring a help to get them working. Here's a great video:
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=sOFxalLOZOI" %}





This is aimed at Ender 3 users and Marlin Firmware, but will still give you an idea of where to start.

{% embed url="https://www.youtube.com/watch?v=sUlqrSq6LeY&t=1s" %}

Once again Ender 3 and Marlin, but includes how to set up BLTouch on the MKS Gen L If you are converting to a MKS Gen L, this video could be helpful It's aimed at Ender 3 Users and Marlin firmware, but trust me, it will still contain useful information

{% embed url="https://www.youtube.com/watch?v=LNdMYgwez8Y" %}

Kevin now has excellent instructions on installing and calibrating the Bltouch in the docs folder of Klipper. Here's a [link to the web version](https://github.com/KevinOConnor/klipper/blob/master/docs/BLTouch.md).



