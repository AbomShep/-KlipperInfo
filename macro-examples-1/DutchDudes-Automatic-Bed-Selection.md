---
description: Creates or selects saved bed mesh for your printer to load. Heating your printbed will cause slight warping in your bed, different temperatures different numbers.
---

Author: DutchDude
{% hint style="info" %}
This macro will check your config for a saved bed mesh, if available it is selected.
If the mesh is not available we'll heat up the bed, calibrate and save at whatever temperature you desire.

Run this in your start print gcode.

Example start gcode:
LOAD_MESH_TEMP BED_TEMPERATURE=50

This will cause the macro to load/generate a bed mesh at a temperature of 50 centigrade.

Example2:
LOAD_MESH_TEMP BED_TEMPERATURE=50 FORCE=1

Changed your bed or want to renew your bed mesh, adding FORCE=1 will generate a new mesh.

It's important to write SAVE_CONFIG in the terminal after your print is finished to save your bed mesh, this will cause your printer to restart.
{% endhint %}

```text
[gcode_macro LOAD_MESH_TEMP]
default_parameter_BED_TEMPERATURE: 0
default_parameter_FORCE: 0
gcode:
    {printer.gcode.action_respond_info("- AUTOMATED BED MESH GENERATOR -")}
    {% if BED_TEMPERATURE|int < 30 %}
        {printer.gcode.action_respond_info("Your bed temp is to low, make sure it is at least 30 or higher")}
    {% else %}
        {% if printer.configfile.config["bed_mesh " + BED_TEMPERATURE] is defined and FORCE|int == 0 %}
            G28 #remove line if you ran G28 before starting this macro
            BED_MESH_PROFILE LOAD={BED_TEMPERATURE}
            {printer.gcode.action_respond_info("Succesfully loaded bed_mesh "+ BED_TEMPERATURE)}
        {% else %}
            {% if printer.configfile.config["bed_mesh " + BED_TEMPERATURE] is defined and FORCE|int == 1 %}
                BED_MESH_PROFILE REMOVE={BED_TEMPERATURE}
            {% endif %}
            {printer.gcode.action_respond_info("bed_mesh not defined, your bed temperature will go up!")}
            {printer.gcode.action_respond_info("We will probe the bed and save the mesh as bed_mesh "+ BED_TEMPERATURE)}
            ADD_BED_MESH TARGET_TEMP={BED_TEMPERATURE}
        {% endif %}
    {% endif %}
    
[gcode_macro ADD_BED_MESH]
default_parameter_TARGET_TEMP: 30
gcode:
    M190 S{TARGET_TEMP} # Wait for the bed to hit TARGET_TEMP
    G28 #remove line if you ran G28 before starting this macro
    BED_MESH_CALIBRATE
    BED_MESH_PROFILE SAVE={TARGET_TEMP}
```