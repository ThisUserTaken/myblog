# Key Characteristics of DC Motors

DC motors are popular for their simplicity, ease of control, and versatility in various applications. Here are the **key characteristics** that define DC motors:

### **Watts are a measurement of power**

Power is arguably the most important characteristic of a motor since it characterizes the amount of work a motor is capable of doing whereas speed and torque describe some of the work a motor is capable of doing.

When designing a mechanism you not only need it to do an action, but you want it done in a certain amount of time. Only using torque when selecting a motor will tell you that the motor is capable of doing that task, but not how fast the task can be done. Only using speed when selecting a motor will tell you the speed a task can be done, but not if the motor is capable of it.

A motor produces the most power at half of the stall torque or half of the free speed. This point is called peak power. If you look at the motor curves, you want the motor to stay on the right side of peak power. Once you go past peak power the motor is drawing more electrical power to produce less mechanical power.

### **Speed-Torque Characteristics**

The speed of a DC motor is inversely proportional to the load torque. As the load increases (higher torque demand), the speed decreases, and vice versa. This relationship can be depicted in a speed-torque curve, illustrating how the motor operates under different load conditions.

### **Voltage-Current Characteristics**

As a DC motor spins, it generates a back electromotive force (back EMF) that opposes the applied voltage. The back EMF increases with speed, which means the net voltage across the motor decreases as speed increases. The motor draws more current under heavier loads, leading to higher torque output. The current is also dependent on the resistance of the motor windings.

### **Efficiency**

Efficiency is the ratio of mechanical power output to electrical power input. It varies with load and operating conditions. Factors affecting efficiency include mechanical losses (friction, windage), electrical losses (resistive heating), and magnetic losses (hysteresis, eddy currents). Higher efficiency indicates better performance and lower energy consumption.

### **Control and Operation**

DC motors are easily controlled for speed and direction. They can be operated in both open-loop (without feedback) and closed-loop (with feedback) systems.

* **Control Methods**:
  * **Pulse Width Modulation (PWM)**: Used to adjust speed by varying the average voltage supplied to the motor.
  * **Feedback Systems (CAN bus-based)**: Using encoders or sensors to maintain precise control over position and speed.

### **Starting Torque**

DC motors can provide high starting torque, which makes them suitable for applications that require overcoming initial inertia, such as in electric vehicles or industrial machinery. This is the maximum torque the motor can produce when the rotor is stationary. It is an important factor in selecting a motor for specific applications.

### **Size and Weight**

DC motors can be relatively compact and lightweight compared to other motor types, making them suitable for applications where space is limited, such as in robotics and small appliances. They come in various sizes and power ratings, allowing for flexibility in design and application.

### **Maintenance Requirements**

Brushed DC motors require periodic maintenance due to brush wear, which can lead to decreased performance over time. Brushless DC motors, on the other hand, have no brushes and require less maintenance. The lifespan of a DC motor can be affected by operational conditions, including load, speed, and environmental factors.

### **Temperature Characteristics**

DC motors generate heat during operation, and their performance can be affected by temperature. Higher temperatures can reduce efficiency and increase the risk of damage.

Some applications may require additional cooling methods (like fans) to maintain optimal operating temperatures.



### FRC Drive Motor Comparison &#x20;

Here is the data from CTR study of different FRC brushless motors

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

