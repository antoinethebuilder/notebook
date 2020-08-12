# Extruder Calibration

## Instructions

#### Extrusion Mode

Connect your printer to your computer and enable the _relative extrusion mode_ by sending `M83`.

#### Initial Measurement

Start by measuring 120 mm before the entrance to the extruder, then mark it.

#### Extrusion Test 1

We will extrude 100 mm of filament by sending `G1 E100 F100`.

### Writing the results

After the printer has successfully extruded 100 mm of filament, measure from the extruder to your original mark.

{% hint style="info" %}
If the measurement is more than 20 mm, your printer is doing under-extrusion.

If the measurement is less than 20 mm, your printer is over-extruding.

If the measurement equals to 20 mm, you are already good to go.
{% endhint %}

### Adjust Extruder

#### Current steps/mm

To know which steps/mm your printer is currently doing, send the command `M503` to your printer.

On the first line, where you see `M92` as shown below:

```text
echo: M92 X80.00 Y80.00 Z400.00 E107.27
```

Write down the E-value, this is your printer's steps/mm.

#### New steps/mm

$$
120 - (100 - result) / (steps * 100)= newsteps
$$

$$
120 - (100 - 23.5)/(107.27 * 100) = 119.99
$$

Finally, to set the new steps/mm, send `M92 E{NEWSTEPS}` which in my case is: `M92 E119.99`

Save your changes by sending: `M500`

{% hint style="info" %}
You can quickly verify if your settings were apply by:

1. Turning your printer off and on again.
2. Send `M503` and verify the steps/mm setting is updated
3. Send `M104 S200` to pre-heat your hotend
4. Test again with the steps as mentioned above.
{% endhint %}

