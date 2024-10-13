# Motor Control Theory

### Introduction

Motor control theories describe how motors are controlled and operated to achieve desired performance, ranging from simple open-loop control to advanced algorithms like Field-Oriented Control. These theories are essential in various applications, including robotics, electric vehicles, and industrial systems, where precision, efficiency, and adaptability are key. Here’s a detailed overview of the primary motor control methods:

Here is a table summarizing the motor control theories with key details on how they work, their advantages, disadvantages, and common applications:

This table organizes the key motor control theories with a clear overview of how each works, its pros and cons, and where it’s typically used.

<table data-header-hidden><thead><tr><th></th><th width="184"></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Control Method</strong></td><td><strong>How It Works</strong></td><td><strong>Advantages</strong></td><td><strong>Disadvantages</strong></td><td><strong>Applications</strong></td></tr><tr><td><mark style="color:red;"><strong>Open-Loop Control</strong></mark></td><td>Signal is sent to the motor without feedback; operates based solely on the input.</td><td>Simple and cost-effective</td><td>No error correction, susceptible to load changes, friction, or other factors.</td><td>Basic fans, pumps, low-cost devices</td></tr><tr><td><mark style="color:red;"><strong>Closed-Loop Control</strong></mark></td><td>Uses sensors to provide feedback on motor speed, position, or torque, and adjusts input accordingly.</td><td>More accurate, adaptable to varying loads and conditions.</td><td>More complex and costly compared to open-loop.</td><td>Robotics, industrial automation, precision drives</td></tr><tr><td><mark style="color:red;"><strong>PID Control</strong></mark></td><td>Adjusts input based on the error between desired and actual performance using proportional, integral, and derivative components.</td><td>High accuracy, smooth performance, eliminates steady-state error.</td><td>Requires careful tuning; can be difficult to set up for complex systems.</td><td>Robotics, CNC machines, conveyor systems, precise control in FRC robots</td></tr><tr><td><mark style="color:red;"><strong>Field-Oriented Control (FOC)</strong></mark></td><td>Decouples control of torque and flux, transforming three-phase currents into a rotating two-axis reference frame.</td><td>Maximizes torque, improves efficiency, smooth control even at low speeds.</td><td>Requires complex algorithms and more computational power.</td><td>Electric vehicles, drones, high-performance robotics, industrial drives</td></tr><tr><td><strong>Direct Torque Control (DTC)</strong></td><td>Directly controls motor torque and flux without transformations like in FOC; continuously adjusts voltage vectors.</td><td>Fast response, high efficiency, better control at low speeds.</td><td>Can produce some torque ripple, not as smooth as FOC.</td><td>High-performance industrial drives, electric vehicles</td></tr><tr><td><strong>Hysteresis Control</strong></td><td>Maintains motor current within a specific range (hysteresis band) by switching input voltage on and off.</td><td>Simple and fast control of current levels.</td><td>Generates noise, less precise than PWM.</td><td>Applications requiring strict current regulation, some motor drives</td></tr><tr><td><mark style="color:red;"><strong>Pulse Width Modulation (PWM)</strong></mark></td><td>Controls motor speed by switching input voltage on and off at high frequency; adjusts speed by varying the duty cycle.</td><td>High efficiency, smooth control, reduces energy loss as heat.</td><td>May produce electrical noise, risk of overheating at low speeds.</td><td>FRC motor controllers, consumer electronics, electric vehicles, robotics</td></tr><tr><td><strong>Sensorless Control</strong></td><td>Estimates motor speed and position using back EMF or electrical signals instead of physical sensors.</td><td>Reduces cost and complexity by eliminating sensors.</td><td>Less accurate at low speeds, depends on motor design.</td><td>Drones, e-bikes, low-cost motor-driven systems, hobbyist robotics</td></tr></tbody></table>





###

PID control (Proportional-Integral-Derivative control) is a fundamental control algorithm used to regulate the speed, position, or torque of electric motors, among many other applications. It’s widely used because it provides an efficient way to control systems by continuously adjusting its output to minimize errors between a desired target and the actual system response.

#### **High-Level Analogy: Controlling a Car's Speed**

Imagine you're driving a car, and your goal is to maintain a specific speed. However, external factors like wind resistance or changes in road slope can affect the car’s speed, requiring you to adjust the gas pedal.

* **Proportional control** is like pressing the gas pedal harder when you're farther from your target speed.
* **Integral control** is like remembering how long you’ve been off the target speed and compensating by pressing the gas pedal more firmly over time if you’ve been lagging for a while.
* **Derivative control** is like predicting how quickly the speed is changing, allowing you to ease off the pedal before you overshoot your target.

Now, let’s break down **PID control** in electric motors with more technical details, but we’ll still lean on this analogy for understanding.

***

#### **PID Control: The Basics**

In electric motors, PID control is used to regulate key parameters like:

1. **Speed** (e.g., keeping a motor running at 3000 RPM).
2. **Position** (e.g., moving a robotic arm to a specific angle).
3. **Torque** (e.g., maintaining a specific force on a load).

The PID controller works by calculating the **error**: the difference between the **desired value** (setpoint) and the **actual value** (what the motor is doing). Then it uses this error to compute how much to adjust the motor’s input (like voltage or current) to bring it closer to the desired target.

The output of the PID controller is the sum of three terms:

* The **Proportional (P)** term.
* The **Integral (I)** term.
* The **Derivative (D)** term.

#### **Step-by-Step Breakdown of PID Terms:**

**1. Proportional Control (P):**

This term is directly proportional to the current error. If the error is large, the control signal is large, which drives the motor harder to correct the error. It’s like pressing the gas pedal more when you notice you're farther from your desired speed.

* **Example**: Suppose the motor is running too slowly compared to the desired speed. The proportional term increases the voltage applied to the motor, causing it to accelerate.

Mathematically:

<div align="left">

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

However, **P alone** isn't enough. If you only use proportional control, the system may not fully reach the target or it might overshoot, causing oscillations.

**2. Integral Control (I):**

The integral term deals with the **accumulated error** over time. This term looks at how long you’ve been away from the target. If the system has been off-target for a long time, the integral term builds up and increases the control signal to eliminate this persistent error.

In our driving analogy, if your car has been below the speed limit for a while, integral control is like pressing the gas more and more until the accumulated slowness is made up for.

* **Example**: If the motor is consistently running slightly below the target speed, the integral term will keep increasing the control signal until this long-term error is corrected.

Mathematically:

<div align="left">

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

One potential problem with the integral term is **integral windup**. If the error accumulates for too long, the system can overshoot the target badly when the error finally corrects itself. Think of pressing the gas too hard for too long and then flying past the speed limit.

**3. Derivative Control (D):**

The derivative term predicts **how the error is changing** by calculating the rate of change of the error. If the error is decreasing quickly, it applies a brake to prevent overshooting. It’s like gently easing off the gas as you approach the speed limit, to avoid speeding past it.

* **Example**: If the motor is rapidly approaching the target speed, the derivative term will reduce the control signal to prevent the motor from overshooting the target.

Mathematically:

<div align="left">

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

#### **The Complete PID Controller Equation:**

The final control signal that adjusts the motor's input is the sum of all three terms:

<div align="left">

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

***

#### **How PID Control Works in Electric Motors**

In an electric motor, the PID controller continuously adjusts the voltage or current supplied to the motor to minimize the error between the desired and actual speed/position. Here’s how it applies:

1. **Proportional (P)** makes immediate adjustments based on the size of the current error. If the motor is too slow, it increases the voltage to make the motor spin faster.
2. **Integral (I)** deals with any sustained, long-term errors. If the motor is consistently off from the target speed by a small margin, the integral term compensates for this error over time.
3. **Derivative (D)** predicts future behavior by looking at the rate at which the motor’s speed is changing. If the motor is speeding up too quickly, it reduces the input to prevent overshooting.

This feedback loop operates in real-time, allowing the motor to follow the desired setpoint (speed, position, or torque) closely, despite any disturbances or changes in load.

***

#### **Tuning the PID Controller (Kp, Ki, Kd)**

The performance of a PID controller depends heavily on how the proportional, integral, and derivative gains are tuned. If the gains aren’t set properly, the motor may oscillate, respond too slowly, or overshoot the target.

<figure><img src="../.gitbook/assets/PID_Compensation_Animated.gif" alt=""><figcaption></figcaption></figure>

* **High Kp​**: The system responds aggressively to error, but it may cause overshoot or oscillation.
* **High Ki​**: The system corrects long-term errors but can cause overshoot or instability due to integral windup.
* **High Kd​**: The system becomes more stable and resists overshooting but may respond too slowly to changes in the target.

In practice, tuning a PID controller for an electric motor is often done through trial and error or using systematic methods like **Ziegler-Nichols tuning**, which helps set the values of Kp, Ki, and Kd​ based on the system’s response.
