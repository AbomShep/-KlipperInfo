---
description: >-
  Since we're not relying on Octoprint to do all the heavy lifting there's a few
  more steps
---

# Installing Raspbian Stretch Lite

 Download Raspbian Stretch Lite from the following URL: [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

{% hint style="info" %}
Some of us are using Linux, so here are instructions for installing Raspbian to the SD card using Windows or Linux
{% endhint %}

{% tabs %}
{% tab title="Windows" %}
Download BalenaEtcher from the following URL and install the Raspbian image to your SD card: [https://www.balena.io/etcher/](https://www.balena.io/etcher/)

{% hint style="info" %}
while this is being done, windows will tell you that drive has an invalid file system and ask you to format it. DON'T. You are installing a Linux image and Windows doesn't recognise it.
{% endhint %}

Etcher will eject the card when it is finished. remove and insert the card again. 

Open File Explorer and open the USB device named "boot" Make sure you are in the root directory. 

create an empty text file and rename it to ssh with no extension. 

create another text file and rename it to "wpa\_supplicant.conf"

edit the file and insert the following: 

```text
	network={   
		ssid="SSID" #Insert the name of your wifi service    
		psk="WIFI-PASS" #Insert the Wifi password
		key_mgmt=WPA-PSK
	}
	country=XX #replace XX with your country code
	ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
	update_config=1
```
{% endtab %}

{% tab title="Linux" %}
```text
	sudo dd bs=4M if=2018-11-13-raspbian-stretch-lite.img of=/dev/sdc status=progress conv=fsync
    ###cd to the card and create an empty ssh file
    touch ssh
	### replace the XX with the your country code. See the hint below for help.
	printf 'ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev\nupdate_config=1\ncountry=XX\n\nnetwork={\n    ssid="SSID"\n    psk="WIFI-PASS"\nkey_mgmt=WPA-PSK\n}\n' > wpa_supplicant.conf
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`For a full list of country codes see:` [**`https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2`**](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)**\`\`**
{% endhint %}





