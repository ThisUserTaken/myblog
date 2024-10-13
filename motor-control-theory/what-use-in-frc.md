# What use in FRC

In the **FIRST Robotics Competition (FRC)**, several motor control techniques are commonly used, depending on the application and the precision required. The most prominent motor control methods employed in FRC include:

#### 1. **Closed-Loop Control**

FRC robots frequently use closed-loop control systems, where feedback from sensors like encoders or gyros is used to adjust the motor's speed or position. This is essential for tasks requiring precise control, such as moving arms, controlling drivetrain speed, or positioning mechanisms.

* **Applications in FRC**: Drivetrains, manipulators, lift mechanisms, and autonomous movement.

#### 2. **Proportional-Integral-Derivative (PID) Control**

PID control is highly popular in FRC, particularly for controlling speed, position, and other dynamic parameters. PID loops allow for smooth and accurate control by minimizing the error between the target and actual performance.

* **Applications in FRC**: Autonomous driving, arm positioning, turret control, and other precise movement tasks.

#### 3. **Pulse Width Modulation (PWM) Control**

PWM is widely used to control the speed of DC motors by varying the average voltage supplied to the motor. Most FRC motor controllers, such as the Talon SRX, Victor SPX, or Spark Max, use PWM to manage motor speeds smoothly.

* **Applications in FRC**: Speed control of motors for drivetrains, intake systems, and flywheels.

#### 4. **Field-Oriented Control (FOC)**

While not used as frequently as PID or PWM in basic systems, Field-Oriented Control is employed in advanced FRC designs, particularly for brushless motors like the Kraken and Falcon 500. These motors, which use FOC for efficient and smooth operation, are gaining popularity due to their high torque and efficiency.

* **Applications in FRC**: Drivetrains, shooting mechanisms (like flywheels), and any system using brushless motors.

#### 5. **Sensorless Control**

Sensorless control, which estimates motor position without physical sensors, is sometimes used with brushless motors in FRC. However, most FRC teams prefer to use feedback sensors like encoders for better accuracy.

* **Applications in FRC**: Brushless motor systems where simplicity and cost are important.

#### 6. **Open-Loop Control**

Open-loop control is occasionally used in FRC for simple systems where feedback isn’t critical. This approach is typically reserved for tasks where precision isn’t as important, such as controlling simple fans or basic systems with fixed movement.

* **Applications in FRC**: Simple mechanisms like conveyor belts or simple shooters.

#### Common FRC Motor Controllers:

* **Talon SRX** and **Victor SPX**: Support PWM and can be configured for closed-loop control with external sensors like encoders.
* **Spark MAX**: Designed for brushless NEO motors.
* **Falcon 500/Kraken x60**: An integrated brushless motor with a built-in controller that uses FOC.

Here’s a comparison table of motor control techniques commonly used in FRC, based on key factors like accuracy, response time, complexity, adaptability, and cost:

<table data-header-hidden><thead><tr><th width="180"></th><th width="156"></th><th width="155"></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Control Method</strong></td><td><strong>Accuracy</strong></td><td><strong>Response Time</strong></td><td><strong>Complexity</strong></td><td><strong>Cost</strong></td></tr><tr><td><strong>Closed-Loop Control</strong></td><td>High</td><td>Moderate</td><td>Moderate</td><td>Moderate</td></tr><tr><td><strong>PID Control</strong></td><td>Very High</td><td>High</td><td>High</td><td>Moderate to High</td></tr><tr><td><strong>PWM Control</strong></td><td>Moderate</td><td>Moderate</td><td>Low</td><td>Low</td></tr><tr><td><strong>Field-Oriented Control (FOC)</strong></td><td>Very High</td><td>Very High</td><td>Very High</td><td>High</td></tr></tbody></table>

###
