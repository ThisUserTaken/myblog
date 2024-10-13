# Programming with wpilib

Writing a program for the XRP is very similar to writing a program for a regular FRC robot. All the same tools (Visual Studio Code, Driver Station, SmartDashboard, etc) can be used with the XRP.

### WPILib Hardware Support

* The onboard LED and Pushbutton are exposed as DIO channels 0 and 1
* 2 motors (left/right) exposed as `XRPMotor` channels 0/1 (These behave similarly to PWM-based motor controllers)
* 2 additional motors (motor3/motor4) exposed as `XRPMotor` channels 2/3
* 2 servo outputs (servo1/servo2) exposed as `XRPServo` channels 4/5
* Encoders for each motor (channel 4/5 for Left, channel 6/7 for Right, channel 8/9 for motor3, channel 10/11 for motor4)
* 3-axis gyro exposed as `XRPGyro`
* The 3-axis accelerometer is exposed as `BuiltInAccel`
* Support for XRP reflectance sensor (reporting Left/Right values on Analog 0/1)
* Support for XRP Rangefinder (reporting values on Analog 2)
