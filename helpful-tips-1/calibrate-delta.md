---
description: Calibration guide for Deltas with removable probes
---

# Calibrate Delta

## Special Thanks

> Credit for the content of this guide goes to user @manu7irl on our Discord

## Getting Started

{% hint style="info" %}
It is recommended, prior to following this guide, to do an endstop calibration to ensure the motor phases are synced to a full step every time it homes. Ensure it homes to exactly the same position each time to verify consistent results.
{% endhint %}

As an example of the type of probe used in this guide, refer here:  
[https://www.aliexpress.com/item/32841920169.html](https://www.aliexpress.com/item/32841920169.html)  
  
This type of probe has a reported deviation of 0.0000 \(tested up to 100mm/s\). You can also use something like this, which will work as well:  
[https://www.aliexpress.com/item/32853123390.html](https://www.aliexpress.com/item/32853123390.html)

## Measure the Max Height Between Bed and Nozzle

Once calculated, add it to the `[stepper_a]` section of the configuration file as shown below. In this example, a value of 520 mm was measured.

```yaml
[stepper_a]
...
position_endstop: 520
```

## Measure Bed Radius and Z Minimum Position

Once calculated, add the radius \(calculated as `diameter / 2`\) both to `[printer]` and `[delta_calibrate]` sections, and the Z minimum position to the `[printer]` section as shown below. In this example, the radius is 170 mm \(diameter 340 mm\) and our minimum Z position is \(-15\) mm

```yaml
[printer]
...
bed_radius: 170
minimum_z_position: -15

[delta_calibrate]
...
radius: 170
```

## Verify \[probe\] Section

Ensure you have a valid `[probe]` section in your configuration file -- if you don't have a probe **THIS WILL NOT WORK**!! An example of a probe configuration is below.

```yaml
[probe]
pin: ar30
speed: 45
z_offset: 0
# X and Y offsets are not needed if the probe is fixed on the nozzle
```

## Add \[bed\_mesh\] Section

Bed meshing allows Klipper to know the geometry of the bed. Even if it looks flat to the naked eye, a probe can pick up on minor inconsistencies and allow better print adhesion as a direct result.

```yaml
[bed_mesh]
speed: 45
horizontal_move_z: 4
#
# Set to what your probe area radius should be
bed_radius: 140
#
# Number of probing points at the largest diameter possible
# 7 points probes 49 times
round_probe_count: 7
algorithm: bicubic
```

## Ease of Use Macros

These make running calibration commands simpler

```yaml
[gcode_macro M851]
gcode:
G28
probe_calibrate

[gcode_macro G32]
gcode:
G28
delta_calibrate
G1 X0 Y0 F4200
save_config

[gcode_macro G29]
gcode:
G28
bed_mesh_calibrate
G1 X0 Y0 Z15 F4200
save_config
```

## Z Offset Set-Up

{% hint style="danger" %}
Removable probe **MUST** be attached for this to work. Failure to do so will cause your nozzle to crash into the bed, and hardware damage can occur.  
  
Also, ensure you are probing **COLD** -- failure to do so can destroy some probes, including the ones linked to at the start of this guide. If your probe can handle temperatures, determine what these temperatures are and stay within the range. If in doubt, don't apply heat.
{% endhint %}

In the GCode terminal of your front end of choice \(OctoPrint, DWC, etc.\), send this command:

```bash
M851
```

{% hint style="danger" %}
Remove the mounted probe at this time, and place a sheet of paper under the nozzle. It will be needed for the next step!  
  
Heat up both nozzle and bed to printing temp as well, this will ensure accurate results at print temperature.
{% endhint %}

Once you have done this, issue the following command until the nozzle catches the paper sheet. It should drag but not firmly:

```bash
testz z=-1
```

If you go too far and the paper sheet will not move, run the following until the paper can be moved but with some resistance \(it should not require a lot of force to do this however\):

```bash
testz z=0.2
```

Once you have done this, the z height is calculated, so we need to run these commands to save this value to the printer configuration:

```bash
accept
save_config
```

This will save the calculated `z_offset` for the first layer height.

## Printer Tower Angle Calibration

{% hint style="danger" %}
Removable probe **MUST** be attached for this to work. Failure to do so will cause your nozzle to crash into the bed, and hardware damage can occur.

Also, ensure you are probing **COLD** -- failure to do so can destroy some probes, including the ones linked to at the start of this guide. If your probe can handle temperatures, determine what these temperatures are and stay within the range. If in doubt, don't apply heat.
{% endhint %}

Now we need to calibrate the printer tower angles, delta\_radius and endstop position according to your arm\_length. To do so, run the following command

```bash
G32
```

Results are saved automatically. It is recommended to do this twice or more for more accurate results.

## Calculate Bed Mesh

{% hint style="danger" %}
Removable probe **MUST** be attached for this to work. Failure to do so will cause your nozzle to crash into the bed, and hardware damage can occur.  
  
Also, ensure you are probing **COLD** -- failure to do so can destroy some probes, including the ones linked to at the start of this guide. If your probe can handle temperatures, determine what these temperatures are and stay within the range. If in doubt, don't apply heat.
{% endhint %}

Now we need to use the probe to calculate the bed mesh. To do so, run the following command:

```bash
G29
```

## Start a Test Print!

Adjust `z_offset` if needed. If everything checks out, you are good to go!

