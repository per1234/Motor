# Motor.h

This class aims to simplify adding motors to Arduino projects. Rather than cluttering each directional change with multiple PWM writes, this class simplifies it to
``` Arduino
#include <Motor.h>
DualPWM motor(9,10,8);

void setup(){
	motor.write(128);
	delay(2000);
	motor.write(-128);
	delay(2000);
	motor.write(0);
}
void loop(){}
```

This class also adds additional feautures to clean up the code base for readablity :

- Initialization automatically configures pin directions, and sets motor in a known state.
- `motor.mirror()` lets you write the same value to motors mounted opposite each other. This avoids tracking negatives all over your code.
- `motor.enable()` and `motor.disable()` will halt all future writes to the motor, even if the motor IC does not have hardware disabling. Great for emergency functions, error handling, or disarm functions via remote control.
- `motor.write()` writes motor values according to  `motor.enableCoastMode(boolean)` which lets you change the default brake/coast modes in a single place.
- `brake()` function accepts optional parameters(`brake(-125)'), allowing you to override enableCoastMode settings for critical movement. 
- Similarly, `coast()` accepts values for mixed-mode decay. However, this mode is not operational on all driver ICs.
- `motor.read()` reports the last value, reducing variables elsewhere


## Extending 
The base class enforces interface compliance, ensuring that any additional motors will be interchangable with existing code. 

In the above example, switching to a new chip can be done by replacing `DualPWM motor(9,10,8);` with  `DVR8837 motor(9,10,8);`, 
correcting the hardware, and the code will respond identically. This makes operating multiple hardware revisions painless. This also allows you to create container
classes that have one or more Motor instances, that don't rely on any specific implimentation. Altering hardware is as simple as a one-line code change.
 
