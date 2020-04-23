---
description: CANCEL_PRINT Macro
---
# CANCEL_PRINT

This is what I use for a cancel macro. Nothing says it has to be done exactly this way.
```text
[gcode_macro CANCEL_PRINT]
gcode:
   M220 S100 ; Reset Speed factor override percentage to default (100%)
   M221 S100 ; Reset Extrude factor override percentage to default (100%)
   G91 ; Set coordinates to relative
   {% if printer.extruder.temperature >= 170 %}
      G1 F1800 E-1 ; Retract filament 3 mm to prevent oozing
   {% endif %}

   ;if all axis are homed, lift the hotend to leave room for hot filament to ooze and to keep it clear of the bed.
   {% if printer.toolhead.homed_axes == "xyz" %}
      G1 F6000 Z10 ; Move Z Axis up 10 mm to allow filament ooze freely
      G90 ; Set coordinates to absolute
      G1 X0 Y221 F1000 ; Move Heat Bed to the front for easy print removal
      M84 ; Disable stepper motors
   {% endif %}
   
   ;set part fan speed to zero.
   M106 S0
   ;bed and hotend are left at the print temps in case I want to restart.
   ```