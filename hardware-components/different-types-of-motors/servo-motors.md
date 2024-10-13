# Servo Motors

A **servo motor** is a specialized motor designed for precise control of angular or linear position, speed, and torque. Unlike regular DC motors that spin continuously, servo motors can rotate to a specific position based on the control signal they receive, making them highly useful in applications where accuracy and repeatability are crucial.

### How Servo Motors Work

A servo motor system typically consists of three main components:

1. **Motor**: A small DC or AC motor that generates the rotational motion.
2. **Control Circuit**: A feedback mechanism that regulates the position of the motor by comparing its current position to the desired position.
3. **Position Sensor**: Typically a **potentiometer** or an **encoder** that provides real-time feedback on the motor's position to the control circuit.

The motor operates by receiving a **control signal** (usually a pulse-width modulation (PWM) signal), which instructs the motor to move to a specific angle. The feedback system continuously checks the current position, and the control circuit adjusts the motor's rotation to match the target position.

<figure><img src="../../.gitbook/assets/Servo_Animation.gif" alt=""><figcaption></figcaption></figure>

### Key Features of Servo Motors

1. **Precise Positioning**: Servo motors are known for their ability to move to a specific angle or position and hold that position.
2. **Feedback Loop**: A built-in sensor, such as an encoder or potentiometer, allows the motor to provide feedback to the controller, ensuring accurate positioning.
3. **Controlled by PWM**: The motor's position is often controlled by a PWM signal, where the width of the pulse determines the position. For example, a certain pulse width may correspond to a specific angle of rotation (0°, 90°, or 180°).

### Types of Servo Motors

1. **Positional Rotation Servo**: The most common type, these servos rotate within a limited range (e.g., 0° to 180°). They are used in applications where precise angular movement is needed.
2. **Continuous Rotation Servo**: These servos rotate continuously in either direction, depending on the control signal. Unlike positional servos, they do not stop at a specific angle but are controlled by speed and direction, making them useful for wheels or conveyors.
3. **Linear Servo**: This type of servo converts rotational motion into linear motion, often using a lead screw or similar mechanism. They are used in applications requiring linear displacement.

### Applications of Servo Motors

Servo motors are used in applications that require **precise control of motion**. Common uses include:

* **Robotics**: Controlling joints or arms that need accurate positioning.
* **RC Vehicles and Drones**: Steering, controlling flaps, or adjusting camera gimbals.
* **CNC Machines**: Ensuring precise cutting, engraving, or milling operations.
* **Industrial Automation**: Positioning sensors, controlling conveyors, or operating valves.
* **Camera Gimbals**: Maintaining stable camera positions in drones or handheld devices.

### Advantages of Servo Motors

* **High Precision**: Servos provide excellent position control and can maintain a fixed position with accuracy.
* **Fast Response**: Servo motors can respond quickly to control signals, making them ideal for real-time applications.
* **Feedback Control**: The built-in feedback loop ensures that the motor’s actual position matches the commanded position.
* **Low Power Consumption at Rest**: Servo motors only consume power when adjusting their position, unlike stepper motors which can draw constant power.

### Disadvantages of Servo Motors

* **Limited Range of Motion**: Positional servos typically have a constrained range of motion, often 180° or less, though continuous rotation servos exist.
* **Complex Control**: The feedback loop and control systems can add complexity to the design, requiring more sophisticated controllers compared to simpler DC motors.
* **Cost**: Servo motors can be more expensive than basic DC motors or stepper motors, especially for high-precision applications.

### Servo Motor vs. Stepper Motor

* **Precision**: Both motors offer precise control, but servos use feedback for accuracy, while stepper motors rely on precise input pulses without feedback.
* **Torque**: Servo motors typically provide higher torque at higher speeds compared to stepper motors.
* **Position Feedback**: Servo motors use feedback to ensure they reach the correct position, while stepper motors operate in open-loop control.
* **Efficiency**: Servo motors are generally more energy-efficient since they draw power only when adjusting position.

In summary, servo motors are widely used for applications requiring **accurate control of position and motion**, and their built-in feedback mechanisms make them ideal for tasks where precision is critical.



### FRC and Servo Motors

Servo motors are allowed but have **specific limitations** in the FIRST Robotics Competition (FRC) due to power, safety, and control considerations. These limitations ensure that servos are used appropriately within the scope of the competition, where more powerful and high-torque motors are typically needed for the robot's primary functions.

#### Key Reasons for Servo Motor Limitations in FRC:

#### 1. **Power and Torque Limitations**

* **Low Power Output**: Servo motors generally produce less torque compared to the larger DC and brushless motors commonly used in FRC (such as the CIM or Falcon 500 motors). While servos are excellent for precise, small-scale movements, they are not powerful enough to handle the high-torque demands of tasks like lifting heavy game elements, driving the robot, or manipulating large mechanisms.
* **Restricted by Rules**: FRC rules limit the cost, size, and power of servos. For example, in most seasons, servos are restricted to PWM COTS servos with a retail **cost of < $75**. This makes them suitable for small, precise tasks but not for high-power applications like driving robot arms or manipulators.

#### 2. **Limited Range of Motion**

* **Limited Rotation**: Most servo motors are designed for **positional rotation**, typically up to 180°, and not for continuous rotation. This restricts their usefulness for certain applications, such as drivetrain systems or mechanisms requiring full rotations or extended travel. While continuous rotation servos exist, they offer limited torque and speed compared to dedicated motors.
* **Application-Specific**: Servos are primarily used for **low-torque, high-precision tasks**, such as adjusting sensors, controlling small actuators, or triggering mechanisms (like a latch or release system). For broader applications requiring more movement and power, teams prefer other FRC-approved motors.

#### 3. **Control System Integration**

* **PWM Control**: Servos are typically controlled using **pulse-width modulation (PWM)** signals, which is simple but limits the scalability and power of the control system. While the FRC control system (roboRIO) can manage servos via PWM outputs, the majority of available motor controllers in FRC (such as the Talon SRX or Spark MAX) are designed for more powerful motors with broader control options (like CAN bus).
* **No Advanced Feedback**: Unlike larger motors that use encoders or other sensors to provide advanced feedback for closed-loop control (such as controlling velocity or distance), standard FRC servos rely only on basic PWM signals without additional feedback, making them less suited for high-performance, dynamic applications.

#### 4. **Safety Concerns**

* **Small but Unpredictable**: While servos are relatively low-power, they can still pose safety risks if improperly used. Because they are generally weaker than other motors, servos might **stall** or **overheat** under heavy loads, which could lead to mechanical failures during competition. FRC prioritizes safe and reliable robot operation, which is why servos are limited to certain applications that fall within their safe operating range.

#### 5. **Availability of Better Alternatives**

* **High-Power Alternatives**: FRC-approved motors, such as the CIM, Mini CIM, Falcon 500, and Neos, are much more powerful and versatile, making them the go-to choice for most applications. These motors can handle high torque, speed, and continuous operation, which are essential for competitive robots. As a result, servo motors are reserved for smaller, more specialized tasks.

#### Typical Uses of Servo Motors in FRC

Despite their limitations, servo motors are still useful for specific tasks that require **precise control but do not demand high torque**. Some common uses include:

* **Actuating small mechanisms**: Controlling a latch, releasing a mechanism, or actuating small arms.
* **Camera adjustments**: Positioning a camera for better vision tracking or targeting.
* **Adjusting sensors**: Moving sensors or other components that require fine-tuning.
* **Triggering mechanisms**: Activating switches or other small components that do not require significant force.

#### Conclusion

In FRC, servo motors are limited primarily because they are not designed for high-torque or continuous high-power tasks that are common in competitive robotics. While they excel in precision tasks and small-scale movements, the need for more powerful, durable motors for driving and manipulating large mechanisms makes servo motors less practical for broader use in FRC robots. Therefore, they are restricted to lower-power, specialized functions within the competition.
