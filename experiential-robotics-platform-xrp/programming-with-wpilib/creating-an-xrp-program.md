# Creating an XRP Program

Creating a new program for an XRP is like creating a normal FRC program, similar to the [Zero To Robot](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-4/index.html) programming steps.

WPILib comes with two templates for XRP projects, including one based on TimedRobot, and a Command-Based project template. Additionally, an example project is provided that showcases some of the built-in functionality of the XRP and shows how to use the vendordep exposed XRP classes. This article will walk through creating a project from this example.

Note

To program the XRP using C++, a compatible C++ desktop compiler must be installed. See [Robot Simulation - Additional C++ Dependency](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/introduction.html#cpp-sim-additional-dependency).

#### Creating a New WPILib XRP Project

Bring up the Visual Studio Code command palette with Ctrl+Shift+P, and type “New project” into the prompt. Select the “Create a new project” command:

![../../\_images/xrp-vscode-new-project.png](https://docs.wpilib.org/en/stable/\_images/xrp-vscode-new-project.png)

This will bring up the “New Project Creator Window”. From here, click on “Select a project type (Example or Template)”, and pick “Example” from the prompt that appears:

![../../\_images/xrp-vscode-select-example.png](https://docs.wpilib.org/en/stable/\_images/xrp-vscode-select-example.png)

Next, a list of examples will appear. Scroll through the list to find the “XRP Reference” example:

![../../\_images/xrp-vscode-reference-example.png](https://docs.wpilib.org/en/stable/\_images/xrp-vscode-reference-example.png)

Fill out the rest of the fields in the “New Project Creator” and click “Generate Project” to create the new robot project.

#### Running an XRP Program

Once the robot project is generated, it is essentially ready to run. The project has a pre-built `Drivetrain` class and associated default command that lets you drive the XRP around using a joystick.

One aspect where an XRP project differs from a regular FRC robot project is that the code is not deployed directly to the XRP. Instead, an XRP project runs on your development computer and leverages the WPILib simulation framework to communicate with the XRP.

To run an XRP program, first, ensure that your XRP is powered on. Next, connect to `XRP-<IDENT>` WiFi network broadcast by the XRP. If you change the XRP network settings (for example, to connect it to your own network), you may change the IP address that your program uses to connect to the XRP. To do this, open the `build.gradle` file and update the `wpi.sim.envVar` line to the appropriate IP address.

```
//Sets the XRP Client Host
wpi.sim.envVar("HALSIMXRP_HOST", "192.168.42.1")
wpi.sim.addXRPClient().defaultEnabled = true
```

Now to start your XRP robot code, open the WPILib Command Palette (type Ctrl+Shift+P) and select “Simulate Robot Code”, or press F5.

![../../\_images/xrp-vscode-simulate.png](https://docs.wpilib.org/en/stable/\_images/xrp-vscode-simulate.png)

If all goes well, you should see the simulation GUI pop up and see the gyro and accelerometer values updating.

Your XRP code is now running!



### Compatible Classes

All classes listed here are supported by the XRP. If a class is not listed here, assume that it is not supported and _will not_ work.

* `Encoder`
* `AnalogInput`
* `DigitalInput`
* `DigitalOutput`
* `BuiltInAccelerometer`

Here is Robot Simulation:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>As can be seen from the image, you'l be able to view all sorts of values like the values from your controller, encoders, analog inputs, and the gyroscope. Also, make sure to set the robot status to teleoperated to actually enable the code.</p></figcaption></figure>

Here's an example using AnalogInput to return values from the range finder and the reflectance sensor:

1. Add the import statements for `AnalogInput`in Drivetrain.java

```java
import edu.wpi.first.wpilibj.AnalogInput;
```

2. Define sensor and range finder

```java
private final AnalogInput m_leftSensor = new AnalogInput(0);
private final AnalogInput m_rightSensor = new AnalogInput(1);
private final AnalogInput m_rangefinder = new AnalogInput(2)
```

3. Read the values from the three `AnalogInput's`&#x20;

```java
  public double getLeftReflectanceValue() {
    return m_leftSensor.getVoltage() / 5.0;
  }

  public double getRightReflectanceValue() {
    return m_rightSensor.getVoltage() / 5.0;
  }

  public double getDistanceMeters() {
    return (m_rangefinder.getVoltage() / 5.0) * 4.0;
  }
  
  public double getDistanceInches() {
    return Units.metersToInches(getDistanceMeters());
  }
```

By adding this code and running "Simulate Robot Code," you should be able to view the AnalogInput data by clicking on "Hardware" and checking "Analog Inputs."

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>How to enable monitoring of Analog Inputs</p></figcaption></figure>

Now, you should be able to view the data from "Analog Inputs."

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

The PWM motor controller classes (e.g. `Spark`) and `Servo` are not supported. The XRP requires use of specialized `XRPMotor` and `XRPServo` classes.

The following classes are provided by the XRP Vendordep (built-in to WPILib).

* `XRPGyro`
* `XRPMotor`
* `XRPServo`
* `XRPOnBoardIO`
