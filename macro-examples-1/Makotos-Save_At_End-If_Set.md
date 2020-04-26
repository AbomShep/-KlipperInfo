---
description: Conditional Saving And Restarting.
---

# Maktoto's SAVE_AT_END & SAVE_IF_SET

Author: Makoto

Description:
Defer saving and restarting of the klipper config (e.g. after bed mesh creation) to a later, more convenient time (e.g. after the print finished).
This is can be done with two small macros.

SAVE_AT_END: This macro contains a variable that stores the save flag which is unset by default and set by executing the macro.

```text

[gcode_macro SAVE_AT_END]
variable_save: 0
gcode:
    SET_GCODE_VARIABLE MACRO=SAVE_AT_END VARIABLE=save VALUE=1

[gcode_macro SAVE_IF_SET]
gcode:
    {% if printer["gcode_macro SAVE_AT_END"].save == 1 %}
    {printer.gcode.action_respond_info("Saving was requested - saving and restarting now")}
    SAVE_CONFIG
    {% endif %}

```
