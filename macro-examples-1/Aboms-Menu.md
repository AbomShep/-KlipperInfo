{% hint style="info" %}
This menu disables some of the stock menu items that I don't use and adds many others.
There are many ways you can do this, but this will give you an example to work with.
{% endhint %}

```text
[menu __main]
type: list
name: Main Menu
items:
    __tune
    __control
    __calibration
    __temp
    __filament
    __prepare

[menu __calibration]
type: list
name: Calibration
items:
    __calibration_home_all_axes
    __calibration_probe_calibrate
    __calibration_probe_accuracy
    __general_firmware_restart

[menu __calibration_accept]
type: command
name: Accept
gcode:
        ACCEPT

[menu __calibration_abort]
type: command
name: Abort
gcode:
        ABORT
action: back

[menu __calibration_probe_accuracy]
type: command
name: Test accuracy
gcode:
    G28
    PROBE_ACCURACY

[menu __calibration_save_config]
type: command
name: Save config
gcode:
        SAVE_CONFIG

[menu __general_firmware_restart]
type: command
name: Restart firmware
gcode:
        FIRMWARE_RESTART

[menu __calibration_home_all_axes]
type: command
name: Home XYZ
gcode:
        G28

[menu __calibration_probe_calibrate]
type: list
name: Adjust Z offset
enter_gcode:
    G28
    PROBE_CALIBRATE
items:
    __calibration__toolhead_zpos
    __calibration_probe_calibrate_testz_minus, __calibration_probe_calibrate_testz_plus
    __calibration_probe_calibrate_testz_minus_minus, __calibration_probe_calibrate_testz_plus_plus
    __calibration_probe_calibrate_testz_minus_1, __calibration_probe_calibrate_testz_plus_1
    __calibration_probe_calibrate_testz_minus_point_1, __calibration_probe_calibrate_testz_plus_point_1
    __calibration_accept
    __calibration_save_config
    __calibration_abort

[menu __calibration__toolhead_zpos]
type: item
width: 16
name: "Z = {0:.3f}"
cursor: \x20
parameter: toolhead.zpos

[menu __calibration_probe_calibrate_testz_minus]
cursor: \x20
type: command
width: 7
name: "   -"
gcode:
        TESTZ Z=-

[menu __calibration_probe_calibrate_testz_plus]
cursor: \x20
type: command
name: "   +"
width: 7
gcode:
        TESTZ Z=+

[menu __calibration_probe_calibrate_testz_minus_minus]
cursor: \x20
type: command
name: "   --"
width: 7
gcode:
        TESTZ Z=--

[menu __calibration_probe_calibrate_testz_plus_plus]
cursor: \x20
type: command
name: "   ++"
width: 7
gcode:
        TESTZ Z=++

[menu __calibration_probe_calibrate_testz_minus_1]
cursor: \x20
type: command
name: "  -1.0"
width: 7
gcode:
        TESTZ Z=-1

[menu __calibration_probe_calibrate_testz_plus_1]
cursor: \x20
type: command
name: "  +1.0"
width: 7
gcode:
        TESTZ Z=+1

[menu __calibration_probe_calibrate_testz_minus_point_1]
cursor: \x20
type: command
name: "  -0.1"
width: 7
gcode:
        TESTZ Z=-0.1

[menu __calibration_probe_calibrate_testz_plus_point_1]
cursor: \x20
type: command
name: "  +0.1"
width: 7
gcode:
        TESTZ Z=+0.1
```
