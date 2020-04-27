---
description: Credit for the content of this guide goes to Chris Riley for his guides on setting up klipper and multiple instances of Octoprint, and to user "shadrincev" on 3dtoday.ru for his guide, from which this is mostly copied/plagiarized. Go here to see his original, and let Google translate from the cyrillic https://3dtoday.ru/blogs/shadrincev/double-klipper/
---

# Author: David Paauwe

{% hint style="info" %}
If you want to run multiple printers simultaneously, you need an instance of Octoprint for each instance of Klipper you set up.

Note that you can set up the second instance of klipper before or after flashing the second controller.
{% endhint %}

1. Install your Octoprint instances, download PuTTY and learn how to SSH into your Raspberry Pi, following Chris's Basement (Chris Riley) tutorial video on installing multiple instances of Octoprint. https://www.youtube.com/watch?v=7Saa1HpLRoM&t=371s

2. Install Klipper, and Octoklipper plugin, for your first Octoprint instance, per the install instructions on github https://github.com/KevinOConnor/klipper/blob/master/docs/Installation.md
   or another Chris Riley video, https://www.youtube.com/watch?v=i_541iD5Bj0

3. Flash firmware onto first printer control board

4. Disconnect first printer from RPi

5. Plug in second printer to a different USB port

6. Follow the klipper installation instructions for firmware installation to flash the second printer control board. Note you do not need to clone the klipper github again or run the install script again. SSH into your Raspberry Pi:

   1. in your ~/klipper directory, run the make Menuconfig and modify settings to correspond to your second printer controller/MCU

      ```text
      cd ~/klipper/
      make menuconfig2
      ```

   2. Run “make” command: make
   3. identify the serial port of second printer, ls /dev/serial/by-id/\*
      It should return something like: dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
   4. stop klipper service, sudo service klipper stop
   5. flash the device using correct serial port:

      ```text
      make flash FLASH_DEVICE=/dev/serial/by-id/<serial port ID from above>
      ```

{% hint style="info" %}
The main part of this guide is below, regarding set up of second instance of Klipper called "klipper2". This is very briefly mentioned in the Klipper Github FAQ, but it was not clear to me. In essence you will copy 3 files, edit 2 of them, and activate the second autorun script.
{% endhint %}

1. Copy Klipper autorun files to create second instance "klipper2"  
   Navigate to your top directory that contains "bin, etc, var, tmp, root" and run the following
   a. sudo cp /etc/init.d/klipper /etc/init.d/klipper2
   b. sudo cp /etc/default/klipper /etc/default/klipper2
   c. sudo cp /var/run/klipper.pid /var/run/klipper2.pid

2. Edit the configuration file to create second instances of the log file and virtual printer port:
   a. open the configuration in nano text editor: sudo nano /etc/default/klipper2
   b. edit KLIPP_ARGS line to change the path to the printer2 config file, the klippy2.log file and create the second virtual printer port. It should look something like:

   ```text
   KLIPPY_ARGS="/home/pi/klipper/klippy/klippy.py /home/pi/printer2.cfg -l /tmp/klippy2.log -I /tmp/printer2"
   ```

   c. Exit and save changes (CRTL, X, Y, ENTER)

3. Edit the autorun file and change everything that says "klipper" to "klipper2".

   ```text
   sudo nano /etc/init.d/klipper2
   ```

   and change:
   a. "klipper daemon" to "klipper2 daemon"
   b. "klipper" to "klipper2"
   c. "starting klipper" to "starting klipper2"
   d. "stopping klipper" to "stopping klipper2"
   e. "Restarting klipper" to "Restarting klipper2"
   f. "Usage: /etc/init.d/klipper {start|stop|status|restart|reload|force-reload}" to
   "Usage: /etc/init.d/klipper2 {start|stop|status|restart|reload|force-reload}"
   g. Exit and save changes

4. Make the autorun script active: sudo chmod +x /etc/init.d/klipper2

5. Restart the systemctl so everything starts automatically:

   ```text
   sudo systemctl daemon-reload
   ```

6. update rc.d

   ```text
   sudo update-rc.d klipper2 defaults
   ```

7. Start the new instance of klipper

   ```text
   sudo /etc/init.d/klipper2 start
   ```

8. Check whether the service was successfully added:

   ```text
   systemctl status klipper2.service
   ```

   If there is a green "active", everything is fine (press CTRL + C to exit)
   The klipper2 service is started and you can connect to the second printer

9. Configure your Octoprint instances, and the Octoklipper plugin to run the corresponding instances of Klipper, making sure to set the correct /tmp/printer in Octoprint AND in the Octoklipper plugin. Set up your printer config files, etc.

10. Repeat as necessary to set up more instances.
