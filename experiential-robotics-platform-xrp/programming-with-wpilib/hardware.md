# Hardware

### Hardware, Sensors, and GPIO

The XRP has the following built-in hardware/peripherals:

* 2x geared drive motors with encoders
* 2x additional geared motor connectors with encoder support (marked Motor3 and Motor4)
* 2x Servo connectors (marked Servo1 and Servo2)
* 1x Inertial Measurement Unit (IMU)
* 1x LED (green)
* 1x pushbutton (marked USER)
* 1x Line following sensor (exposed as 2 Analog inputs)
* 1x Ultrasonic PING style rangefinder (uses 2 digital IO pins, exposed as an analog input)

#### Motors, Wheels, and Encoders

The motors used on the XRP have a 48.75:1 gear reduction and a no-load output speed of 90 RPM at 4.5V.

The wheels have a diameter of 60mm (2.3622”). They have a trackwidth of 155mm (6.1”).

The encoders are connected directly to the motor output shaft and have 12 Counts Per Revolution (CPR). With the provided gear ratio, this nets 585 counts per wheel revolution.

The motor channels are listed in the table below.

| Channel    | XRP Hardware Component |
| ---------- | ---------------------- |
| XRPMotor 0 | Left Motor             |
| XRPMotor 1 | Right Motor            |
| XRPMotor 2 | Motor 3                |
| XRPMotor 3 | Motor 4                |

Note

The right motor will spin in a backward direction when positive output is applied. Thus the corresponding motor controller needs to be inverted in the robot code.

The servo channels are listed in the table below.

| Channel    | XRP Hardware Component |
| ---------- | ---------------------- |
| XRPServo 4 | Servo 1                |
| XRPServo 5 | Servo 2                |

The encoder channels are listed in the table below.

| Channel | XRP Hardware Component              |
| ------- | ----------------------------------- |
| DIO 4   | Left Encoder Quadrature Channel A   |
| DIO 5   | Left Encoder Quadrature Channel B   |
| DIO 6   | Right Encoder Quadrature Channel A  |
| DIO 7   | Right Encoder Quadrature Channel B  |
| DIO 8   | Motor3 Encoder Quadrature Channel A |
| DIO 9   | Motor3 Encoder Quadrature Channel B |
| DIO 10  | Motor4 Encoder Quadrature Channel A |
| DIO 11  | Motor4 Encoder Quadrature Channel B |

Note: By default, the encoders count up when the XRP moves forward.

#### Inertial Measurement Unit

The XRP includes an STMicroelectronics LSM6DSOX Inertial Measurement Unit (IMU) which contains a 3-axis gyro and a 3-axis accelerometer.

The XRP will calibrate the gyro and accelerometer upon each boot (the onboard LED will quickly flash for about 3-5 seconds at startup time).

#### Onboard LED and Push Button

The XRP has a push button (labeled USER) and a green LED onboard that are exposed as Digital IO (DIO) channels to robot code.

| DIO Channel | XRP Hardware Component |
| ----------- | ---------------------- |
| DIO 0       | USER Button            |
| DIO 1       | Green LED              |

Note: DIO 2 and 3 are reserved for future system use.

#### Line Following (Reflectance) Sensor

When assembled according to the instructions, the XRP supports a line-following sensor with 2 sensing elements. Each sensing element measures reflectance and exposes these as AnalogInput channels to robot code. The returned values range from 0V (pure white) to 5V (pure black).

| AnalogInput Channel | XRP Hardware Component   |
| ------------------- | ------------------------ |
| AnalogInput 0       | Left Reflectance Sensor  |
| AnalogInput 1       | Right Reflectance Sensor |

#### Ultrasonic Rangefinder

When assembled according to the instructions, the XRP supports an ultrasonic, PING-style, rangefinder. This is exposed as an analog input channel to robot code. The returned values range from 0V (20mm) to 5V (4000mm).

| AnalogInput Channel | XRP Hardware Component |
| ------------------- | ---------------------- |
| AnalogInput 2       | Ultrasonic Rangefinder |

### Compatible Hardware

In general, the XRP is compatible with the following:

* Hobby DC motors with built-in encoders (6-pin connector)
* Standard RC-style PWM output devices (e.g. servos, PWM-based motor controllers)
* “Ping” style ultrasonic sensors (only when connected to the RANGE port)

### Incompatible Hardware

Due to hardware limitations, the XRP is not compatible with the following:

* Encoders other than those already integrated into Hobby motors
* Timing based sensors
* CAN-based devices
