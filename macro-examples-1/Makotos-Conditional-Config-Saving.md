---
description: Conditional Saving And Restarting.
---

# Maktoto's Conditional Config Saving

Author: Makoto

## Description

There are occasions where the printer needs to save the active config to the config file.
One of which is after a new bed mesh has been created. Unfortunately, a side-effect of saving is a restart of 
Klipper.

If the bed mesh is created as part of the print start macro (e.g. by using DutchDude's automatic bed selection) this 
aborts the print requiring the user to restart the print again.

A way to save the newly created mesh is to defer the saving action to after the print. For various reasons ond might not 
want to restart the printer unconditionally. The below functions do a conditional save where the SAVE_IF_SET macro saves 
the configfile only if the SAVE_AT_END macro has been called since the printer's startup.

## Macros

### SAVE_AT_END

This macro contains a variable that stores the save flag which is unset by default and set by executing the macro.

```text
[gcode_macro SAVE_AT_END]
variable_save: 0
gcode:
    SET_GCODE_VARIABLE MACRO=SAVE_AT_END VARIABLE=save VALUE=1
```

### SAVE_IF_SET

This macro saves and restarts if the flag has been set or otherwise does nothing.

```text
[gcode_macro SAVE_IF_SET]
gcode:
    {% if printer["gcode_macro SAVE_AT_END"].save == 1 %}
    {printer.gcode.action_respond_info("Saving was requested - saving and restarting now")}
    SAVE_CONFIG
    {% endif %}
```
