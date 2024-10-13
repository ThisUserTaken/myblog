# Brushless Motor

### How Brushed Motors Work

The brushless DC (BLDC) motor operates on the same principles as a brushed-type DC motor; however, the permanent magnet and electromagnet parts of the motor are swapped, as shown in figure 4 below.  In contrast to a brushed DC motor, a BLDC motor has one or more sets of permanent magnets that rotate (called the _rotor_) and a stationary [armature](http://en.wikipedia.org/wiki/Armature\_\(electrical\_engineering\)) (called the _stator_).  Since the electromagnet part of the BLDC motor is fixed, the problem (existing in brushed-type motors) of connecting current to a moving armature is eliminated.

External circuitry monitors the current position of the rotating magnet and energizes the external (stator) magnets to keep the motor turning.  This circuitry is part of the brushless electronic speed control (ESC).  The brushless ESC monitors rotor position using magnetic sensors and the Hall-effect.  These sensors report back to the ESC.  Stated another way, the brush/commutator assembly of the brushed DC motor is replaced by an electronic controller which continually switches the phase to the windings to keep the motor turning.  The controller performs similar timed power distribution by using a solid-state circuit rather than the brush/commutator system.

<figure><img src="../../.gitbook/assets/brushlessmotor_4.gif" alt=""><figcaption></figcaption></figure>

### Advantages of Brushless Motors

There are many advantages brushless motors have over their brushed counterparts. First, understand that brushed motors have some problems related to their construction:

* **Brush Lifespan** - Brushes will wear out over time leading to degraded performance and eventual failure. This cannot be avoided and can put FIRST teams at a significant disadvantage, as their motors may not perform the same as the season progresses. This is why many teams use new brushed motors every year and some have even started replacing them mid-season to ensure peak performance
* **Heat** - The friction of the brushes rubbing on the rotor produces heat. Another source of heat is electrical arcing that occurs during normal operation
* **Efficiency** - The brushes rubbing on the rotor also creates a torque load on the motor. This reduces a brushed motor’s efficiency. Brushed motors are typically less than 75% efficient
* **Electrical Noise** - Brushed motors produce a lot of electrical noise which affects the analog signals on the sensor cable that runs close to the motor

### **Inrunner & Outrunner Motors**

The construction of brushless motors supports two main designs: **Inrunner** and **Outrunner** motors.

* **Inrunner Motors** have a spinning center shaft, similar to a brushed motor. The rotor (with permanent magnets) is inside the stator, which is fixed. These motors are well-suited for **high-speed, low-torque** applications, such as RC cars or drones, where rapid rotation is more critical than torque.
* **Outrunner Motors**, on the other hand, have a rotor on the outside, meaning the outer shell of the motor spins while the inner part (stator) remains stationary. This design allows for a larger radius, increasing torque output. As a result, outrunner motors are ideal for **low-speed, high-torque** applications, such as electric bikes or gimbals, where strong rotational force is needed at lower speeds.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Advantages Outrunner Motor

In summary, the advantages of an outrunner motor include:

* Maximum torque achieved for minimal build volume.
* No need for mechanical transmission or gearboxes.
* The rotor can directly drive the application.
* Highly accurate.
* No risk of contamination.

**Sensored vs. Sensorless**

Brushless controllers need to know, or at least make an educated guess about the location of the shaft. This way, the controller knows what phases need to be energized or set neutral. There are two ways this can be done:

* **Sensored Commutation** - This is where a sensor, like a hall effect or magnetic encoder, is used to track the location of the shaft. Sensored commutation is great at producing torque at extremely low speeds and handling high dynamic loads
* **Sensorless Commutation** - This is where the controller reads the motor’s back-EMF (Electromagnetic Frequency) to know the position of the shaft. Sensorless commutation is great at high speed but performs poorly at low speed

Sensorless performance is _so_ much better at high-speed applications, that many controllers will use sensored commutation at low speed and then transition to sensorless as the motor accelerates.

**Trapezoidal Commutation**

Trapezoidal Commutation is the most basic form of brushless commutation. With trapezoidal commutation, rotation is split into six steps of 60 degrees. With each step, the controller energizes two of the three phases of the motor. An example of a full revolution would look like:

<table data-header-hidden data-full-width="false"><thead><tr><th width="95"></th><th width="79"></th><th width="93"></th><th width="78"></th><th width="87"></th><th width="91"></th><th></th></tr></thead><tbody><tr><td>Phase</td><td>0-60°</td><td>60-120°</td><td>120-180°</td><td>180-240°</td><td>240-300°</td><td>300-360°</td></tr><tr><td><strong>A</strong></td><td><strong>+</strong></td><td><strong>+</strong></td><td><strong>N</strong></td><td><strong>N</strong></td><td><strong>-</strong></td><td><strong>-</strong></td></tr><tr><td><strong>B</strong></td><td><strong>-</strong></td><td><strong>N</strong></td><td><strong>+</strong></td><td><strong>-</strong></td><td><strong>+</strong></td><td><strong>N</strong></td></tr><tr><td><strong>C</strong></td><td><strong>N</strong></td><td><strong>-</strong></td><td><strong>-</strong></td><td><strong>+</strong></td><td><strong>N</strong></td><td><strong>+</strong></td></tr></tbody></table>

The name trapezoidal commutation comes from the shape of the graph when you plot the motor’s back-EMF.

<figure><img src="https://motors.vex.com/media/wysiwyg/Trapezoidal.png" alt=""><figcaption></figcaption></figure>

One significant drawback of **trapezoidal commutation** is the presence of **torque ripple** during phase transitions. Torque ripple occurs every time the commutation steps change because the motor phases cannot energize instantaneously during these transitions. This interruption in smooth power delivery causes fluctuations in torque, resulting in a lower average torque output and producing audible noise as the motor operates. These issues make trapezoidal commutation less ideal for applications requiring smooth, quiet, and consistent motor performance, especially at low speeds.

\
**Field Oriented Control**

Brushless motors deliver their maximum torque when the motor’s magnetic field is orthogonal (at a 90° angle) to the rotor's permanent magnets.

**Field Oriented Control (FOC)** is a modern and sophisticated method of controlling brushless motors that employs **sinusoidal commutation**. Unlike the traditional trapezoidal commutation, which applies voltage to only two of the motor's three phases at a time, FOC applies a continuously varying voltage to all three phases simultaneously. By dynamically adjusting the voltage in real-time as the motor rotates, FOC keeps the motor’s magnetic field optimally aligned with the rotor, ensuring smooth operation, improved efficiency, and consistent torque output across a wide range of speeds.

4o

<figure><img src="https://motors.vex.com/media/wysiwyg/FOC.png" alt=""><figcaption></figcaption></figure>

The advantages of FOC are:

* No torque ripple, the motor transitions between steps since the phases always have a voltage applied to it
* Better control of the motor’s phases. This allows the controller to make sure that the motor’s phases are always orthogonal to the permanent magnets
* More torque at a lower speed
* Since more torque is being produced, there’s an increase in the amount of mechanical power the motor is producing
* More mechanical power means an increase in the motor’s efficiency
* Less vibration and noise during operation. This also extends the life of the motor

### FRC Approved Brushed Motors

1. West Coast Products Kraken x60: FOC available with a license
2. CTR Electronics/VEX Robotics Falcon 500
3. REV Robotics NEO Brushless
4. REV Robotics NEO 550
5. REV Robotics NEO Vortex

In the world of FRC, there’s a huge advantage to using brushless motors since they’re smaller, lighter, and more efficient. FRC teams building robots using only brushless motors will have a big advantage on weight over teams using no brushless motor or a mix of brushed and brushless motors.

Additionally, an FRC robot has a fixed amount of power since teams are limited to a single battery that has a limit on how much current it can produce at any given moment. For teams to get more power out of their robots, the only option is to increase the efficiency of the motors on their robot.
