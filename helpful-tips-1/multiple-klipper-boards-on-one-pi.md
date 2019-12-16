---
description: >-
  Setup guide for how to run multiple Klipper instances on a single Raspberry
  Pi. Much of this should translate well for Linux Desktops also.
---

# Multiple Klipper Boards on One Pi

## Special Thanks

> Credit for the content of this guide goes to Chris Riley \(Chris's Basement\) for his guides on setting up Klipper and multiple instances of Octoprint, and to user "shadrincev" on 3dtoday.ru for his guide, from which this is mostly copied/plagiarized. Go here to see his original, and let Google translate from the Cyrillic [https://3dtoday.ru/blogs/shadrincev/double-klipper/](https://3dtoday.ru/blogs/shadrincev/double-klipper/)
>
> @sprocketscientist

## Step-By-Step Instructions

{% hint style="info" %}
If you want to run multiple printers simultaneously, you need an instance of Octoprint for each instance of Klipper you set up.

Note that you can set up the second instance of Klipper before or after flashing the second controller.
{% endhint %}

{% hint style="warning" %}
In some use cases, users report only one mainboard displaying when running the following command:  
`ls /dev/serial/by-id/*`  
  
If you only see one mainboard when running this command, try this command instead:  
`ls /dev/serial/by-path/*`  
  
If the second command shows multiple results but the first one does not, adjust the directions accordingly, so all instances of `/by-id/` are replaced with `/by-path/`
{% endhint %}

1. Install your Octoprint instances, download PuTTY and learn how to SSH into your Raspberry Pi, following Chris's Basement \(Chris Riley\) tutorial video on installing multiple instances of Octoprint: [https://www.youtube.com/watch?v=7Saa1HpLRoM&t=371s](https://www.youtube.com/watch?v=7Saa1HpLRoM&t=371s) 
2. Install Klipper, and Octoklipper plugin, for your first Octoprint instance, per the install instructions on github [https://github.com/KevinOConnor/klipper/blob/master/docs/Installation.md](https://github.com/KevinOConnor/klipper/blob/master/docs/Installation.md) or another Chris Riley video, [https://www.youtube.com/watch?v=i\_541iD5Bj0](https://www.youtube.com/watch?v=i_541iD5Bj0) 
3. Flash firmware onto first printer control board 
4. Disconnect first printer from RPi 
5. Plug in second printer to a different USB port 
6. Follow the klipper installation instructions for firmware installation to flash the second printer control board. Note you do not need to clone the klipper github again or run the install script again. SSH into your Raspberry Pi: 
   1. in your `~/klipper` directory, run the make Menuconfig and modify settings to correspond to your second printer controller/MCU `cd ~/klipper/ make menuconfig` 
   2. Run “make” command: `make` 
   3. identify the serial port of second printer, `ls /dev/serial/by-id/*`  It should return something like: `dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0` 
   4. stop klipper service, `sudo service klipper stop` 
   5. flash the device using correct serial port: `make flash FLASH_DEVICE=/dev/serial/by-id/<serial port ID from above>` 
7. The main part of this guide is below, regarding set up of second instance of Klipper called "klipper2". This is very briefly mentioned in the Klipper Github FAQ, but it was not clear to me. In essence you will copy 3 files, edit 2 of them, and activate the second autorun script.  
  
   Copy Klipper autorun files to create second instance "klipper2" Navigate to your top directory that contains "bin, etc, var, tmp, root" and run the following:  
   `sudo cp /etc/init.d/klipper /etc/init.d/klipper2`

   `sudo cp /etc/default/klipper /etc/default/klipper2`

   `sudo cp /var/run/klipper.pid /var/run/klipper2.pid`  

8. Edit the configuration file to create second instances of the log file and virtual printer port: 
   1. open the configuration in nano text editor: `sudo nano /etc/default/klipper2` 
   2. edit KLIPP\_ARGS line to change the path to the printer2 config file, the klippy2.log file and create the second virtual printer port. It should look something like: `KLIPPY_ARGS="/home/pi/klipper/klippy/klippy.py /home/pi/printer2.cfg -l /tmp/klippy2.log -I /tmp/printer2"` 
   3. Exit and save changes \(CTRL, X, Y, ENTER\) 
9. Edit the autorun file and change everything that says "klipper" to "klipper2". `sudo nano /etc/init.d/klipper2` and change: 
   1. "`klipper daemon`" to "`klipper2 daemon`" 
   2. "`klipper`" to "`klipper2`" 
   3. "`starting klipper`" to "`starting klipper2`" 
   4. "`stopping klipper`" to "`stopping klipper2`" 
   5. "`Restarting klipper`" to "`Restarting klipper2`" 
   6. "`Usage: /etc/init.d/klipper {start|stop|status|restart|reload|force-reload}`" to "`Usage: /etc/init.d/klipper2 {start|stop|status|restart|reload|force-reload}`" 
   7. Exit and save changes 
10. Make the autorun script active: `sudo chmod +x /etc/init.d/klipper2` 
11. Restart the systemctl so everything starts automatically: `sudo systemctl daemon-reload` 
12. update rc.d `sudo update-rc.d klipper2 defaults` 
13. Start the new instance of klipper `sudo /etc/init.d/klipper2 start` 
14. Check whether the service was successfully added:

    `systemctl status klipper2.service`

    If there is a green "active", everything is fine \(press CTRL + C to exit\)

    The klipper2 service is started and you can connect to the second printer  

15. Configure your Octoprint instances, and the Octoklipper plugin to run the corresponding instances of Klipper, making sure to set the correct /tmp/printer in Octoprint AND in the Octoklipper plugin. Set up your printer config files, etc. 
16. Repeat as necessary to set up more instances 

