Author: mental

Description: 
>Increases the brightness from black to bright red based on extruder temperature. Shows green when extruder is safe to touch.

{% hint style="info" %}
This macro uses a gcode override on the M105 gcode to call this macro every time M105 is called. It is important to call the overridden gcode M105.1 somewhere in the macro so that the base functionality of the command is not lost. A jinja template is then evaluated based on the temperature parameters. After the template is applied, the gcode that is actually output is one only two lines of gcode containing 
```text
M105.1
SET_LED LED=temp_leds RED=*VAL* GREEN=0 BLUE=0 INDEX=0  TRANSMIT=1
```
{% endhint %}

```text
[neopixel temp_leds]
pin:                                P1.18 
chain_count:                        1

[gcode_macro M105]
rename_existing:            M105.1
gcode:  

   M105.1
   
   #if the extruder is off
   {% if printer.extruder.target == 0 %}

      #Set the LED to red if the extruder is off but is still hot, otherwise 
      # set the color to green
      {% if printer.extruder.temperature > 60.0 %}
         SET_LED LED=temp_leds RED=1 GREEN=0 BLUE=0 INDEX=0  TRANSMIT=1
      {% else %}
         SET_LED LED=temp_leds RED=0 GREEN=1 BLUE=0 INDEX=0  TRANSMIT=1
      {% endif %}     

   {% else %}
 
         #if the extruder temp is at target temperature 
         {% if printer.extruder.temperature >= printer.extruder.target - 4.0 %}
            SET_LED LED=temp_leds RED=1 GREEN=0 BLUE=0 INDEX=0  TRANSMIT=1

         #if the extruder is still heating
         {% else %}
            {% set scaler = printer.extruder.temperature|float / printer.extruder.target|float %}
            SET_LED LED=temp_leds RED={ scaler|float * 1 } GREEN=0 BLUE=0 INDEX=0  TRANSMIT=1
         {% endif %}  

   {% endif %}
```