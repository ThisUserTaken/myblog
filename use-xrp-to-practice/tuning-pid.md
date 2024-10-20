# Tuning PID

Simples program

```arduino
#include <Arduino.h>

// Pin definitions
#define ENCA 4  // Encoder A pin
#define ENCB 5  // Encoder B pin
#define PH_A 6  // Motor A phase pin (controls direction)
#define EN_A 7  // Motor A enable pin (controls speed)

// Global variables to track motor position and PID control
volatile int position = 0;    // Current encoder position
long previousTime = 0;        // Previous time for PID calculations
float previousError = 0;      // Error from last iteration for derivative calculation
float errorIntegral = 0;      // Integral of error over time
long delayStart = 5000;       // Time delay for switching target positions
long startTime = 0;           // Time when the motor starts moving toward the target
bool targetReached = false;   // Flag to check if the target is reached
const int positionThreshold = 3;  // Threshold to consider the target reached
long timeToReachTarget = 0;   // Time taken to reach the target
bool targetLock = false;      // Locks the target to avoid resetting time on oscillations
int currentTarget = 120;      // Current target position

// PID parameters (tunable via serial input)
float kp = 1.0;   // Proportional gain
float ki = 0.0;   // Integral gain
float kd = 0.5;   // Derivative gain

// Function prototypes
void setupPins();
void setupEncoder();
void motorControl(int targetPosition);
float calculatePID(int targetPosition);
void handleTarget();
void updatePIDValues();
void updateEncoder();

void setup() {
  Serial.begin(9600);  // Initialize serial communication
  setupPins();         // Setup the motor control and encoder pins
  setupEncoder();      // Initialize the encoder interrupt

  // Print instructions for entering PID values
  Serial.println("Enter kp, ki, and kd values separated by spaces (e.g., 1.0 0.5 0.1):");
  delayStart = millis();  // Initialize the delay timer
  startTime = millis();   // Set initial start time
}

// Main loop
void loop() {
  updatePIDValues();  // Check for new PID values via serial input
  handleTarget();     // Switch between target positions every 5 seconds
}

// Setup motor control and encoder pins
void setupPins() {
  pinMode(ENCA, INPUT);
  pinMode(ENCB, INPUT);
  pinMode(PH_A, OUTPUT);
  pinMode(EN_A, OUTPUT);
}

// Setup encoder interrupt
void setupEncoder() {
  attachInterrupt(digitalPinToInterrupt(ENCA), updateEncoder, RISING);
}

// Motor control function using PID output
void motorControl(int targetPosition) {
  float controlSignal = calculatePID(targetPosition);

  // Constrain control signal to 0-255 for motor power
  float motorPower = constrain(fabs(controlSignal)+12 , 0, 255);

  // Set motor direction
  digitalWrite(PH_A, controlSignal > 0 ? LOW : HIGH);  // LOW = reverse, HIGH = forward

  // Set motor speed
  analogWrite(EN_A, motorPower);
}

// Calculate PID control output
float calculatePID(int targetPosition) {
  long currentTime = micros();
  float deltaTime = (currentTime - previousTime) / 1.0e6;  // Convert to seconds
  previousTime = currentTime;

  // Read the current motor position
  noInterrupts();
  int currentPosition = position;
  interrupts();

  // Calculate error and its derivative
  int error = currentPosition - targetPosition;
  float errorDerivative = (error - previousError) / deltaTime;

  // Calculate integral of error
  errorIntegral += error * deltaTime;

  // Compute PID control signal
  float controlSignal = kp * error + kd * errorDerivative + ki * errorIntegral;

  // Update previous error
  previousError = error;
 // Track time to reach target
  if (abs(error) <= positionThreshold ) {
    //startTime = millis();
    targetReached = false;
  }

  if (abs(error) > positionThreshold ) {
    timeToReachTarget = millis() - startTime;
    targetReached = true;

    // Debug output
    Serial.print("Target: ");
    Serial.print(targetPosition);
    Serial.print(" Position: ");
    Serial.print(currentPosition);
    Serial.print(" Time to reach: ");
    Serial.println(timeToReachTarget);
  }
  return controlSignal;
}

// Handle target switching between positions every 5 seconds
void handleTarget() {
  int newTarget;
  
  // Toggle between target positions every 5 seconds
  if (millis() < delayStart + 5000) {
    newTarget = 120;  // Move to position 120
  } else {
    newTarget = -120;  // Move to position -120
  }

  // If the target position has changed, reset the startTime and lock flags
  if (newTarget != currentTarget) {
    currentTarget = newTarget;  // Update current target
    startTime = millis();       // Reset startTime when switching target
    targetReached = false;      // Reset target reached flag
    targetLock = false;         // Unlock the target to allow timing
    timeToReachTarget = 0;      // Reset time to reach target

    //Serial.print("Switched to new target: ");
    //Serial.println(currentTarget);
  }

  motorControl(currentTarget);  // Control motor to reach the current target

  // Reset delay timer every 10 seconds (to create a 5-second loop)
  if (millis() > delayStart + 10000) {
    delayStart = millis();
  }
}

// Update PID values from serial input
void updatePIDValues() {
  if (Serial.available()) {
    float newKp = Serial.parseFloat();
    float newKi = Serial.parseFloat();
    float newKd = Serial.parseFloat();

    if (newKp != 0 || newKi != 0 || newKd != 0) {
      kp = newKp;
      ki = newKi;
      kd = newKd;

      // Debug output for new PID values
      Serial.print("Updated PID values: kp=");
      Serial.print(kp);
      Serial.print(", ki=");
      Serial.print(ki);
      Serial.print(", kd=");
      Serial.println(kd);
    }
  }
}

// Interrupt function to update encoder position
void updateEncoder() {
  // Update position based on encoder input
  position += digitalRead(ENCB) ? -1 : 1;
}

```

Class based program

```arduino
#include <Arduino.h>

// Pin definitions
#define ENCA 4  // Encoder A pin
#define ENCB 5  // Encoder B pin
#define PH_A 6  // Motor A phase pin (controls direction)
#define EN_A 7  // Motor A enable pin (controls speed)

// MotorController class to handle encoder, motor control, and PID
class MotorController {
  private:
    int pinA, pinB, pinPhase, pinEnable;  // Pins for encoder and motor control
    volatile int position;  // Encoder position
    long previousTime;      // For PID calculations (time tracking)
    float previousError;    // For derivative calculation in PID
    float errorIntegral;    // For integral calculation in PID
    long delayStart;        // For timing target switches
    long startTime;         // Start time when switching to new target
    long timeToReachTarget; // Time taken to reach the target
    bool targetReached;     // Flag to check if target is reached
    bool targetLock;        // Locks target once reached to prevent oscillation issues
    int currentTarget;      // The current target position
    const int positionThreshold; // Threshold to determine if the target is reached
    
    // PID parameters (Proportional, Integral, Derivative gains)
    float kp, ki, kd;

  public:
    // Constructor to initialize pins, variables, and default PID values
    MotorController(int encoderPinA, int encoderPinB, int motorPhasePin, int motorEnablePin)
      : positionThreshold(3), position(0), previousTime(0), previousError(0), errorIntegral(0), 
        delayStart(5000), startTime(0), timeToReachTarget(0), targetReached(false), 
        targetLock(false), currentTarget(120), kp(1.0), ki(0.0), kd(0.5) {
      pinA = encoderPinA;
      pinB = encoderPinB;
      pinPhase = motorPhasePin;
      pinEnable = motorEnablePin;

      // Setup pins
      pinMode(pinA, INPUT);
      pinMode(pinB, INPUT);
      pinMode(pinPhase, OUTPUT);
      pinMode(pinEnable, OUTPUT);
    }

    // Attach interrupts to read encoder data
    void attachInterrupts(void (*isr)()) {
      attachInterrupt(digitalPinToInterrupt(pinA), isr, RISING);
    }

    // Interrupt handler to update the encoder position
    void updatePosition() {
      position += digitalRead(pinB) ? -1 : 1;  // Increment or decrement based on ENCB
    }

    // Motor control function to calculate PID and adjust motor speed and direction
    void controlMotor(int targetPosition) {
      float controlSignal = calculatePID(targetPosition);

      // Constrain control signal to 0-255 for motor power
      float motorPower = constrain(fabs(controlSignal) + 15, 0, 255);

      // Set motor direction based on control signal
      digitalWrite(pinPhase, controlSignal > 0 ? LOW : HIGH);  // LOW = reverse, HIGH = forward

      // Apply motor speed (using PWM)
      analogWrite(pinEnable, motorPower);
    }

    // Calculate PID control signal
    float calculatePID(int targetPosition) {
      long currentTime = micros();
      float deltaTime = (currentTime - previousTime) / 1.0e6;  // Convert to seconds
      previousTime = currentTime;

      // Safely read the encoder position
      noInterrupts();
      int currentPosition = position;
      interrupts();

      // Calculate error and its derivative
      int error = currentPosition - targetPosition;
      float errorDerivative = (error - previousError) / deltaTime;

      // Accumulate the integral of the error
      errorIntegral += error * deltaTime;

      // Compute PID control signal
      float controlSignal = kp * error + kd * errorDerivative + ki * errorIntegral;

      // Update previous error for next iteration
      previousError = error;

      // Track time to reach target
      if (abs(error) <= positionThreshold && !targetReached) {
        timeToReachTarget = millis() - startTime;
        targetReached = true;
        Serial.print("Target: ");
        Serial.print(targetPosition);
        Serial.print(" Position: ");
        Serial.print(currentPosition);
        Serial.print(" Time to reach: ");
        Serial.println(timeToReachTarget);
      }

      return controlSignal;
    }

    // Function to switch between target positions every 5 seconds
    void handleTarget() {
      int newTarget;

      // Switch target every 5 seconds
      if (millis() < delayStart + 5000) {
        newTarget = 120;  // Move to target 120
      } else {
        newTarget = -120;  // Move to target -120
      }

      // Check if the target position has changed and reset variables accordingly
      if (newTarget != currentTarget) {
        currentTarget = newTarget;  // Update target position
        startTime = millis();       // Reset start time
        targetReached = false;      // Reset target reached flag
        targetLock = false;         // Unlock target for timing
        timeToReachTarget = 0;      // Reset time to reach target
      }

      // Control the motor to reach the target
      controlMotor(currentTarget);

      // Reset delay timer every 10 seconds
      if (millis() > delayStart + 10000) {
        delayStart = millis();
      }
    }

    // Update PID values via serial input
    void updatePIDValues() {
      if (Serial.available()) {
        float newKp = Serial.parseFloat();
        float newKi = Serial.parseFloat();
        float newKd = Serial.parseFloat();

        if (newKp != 0 || newKi != 0 || newKd != 0) {
          kp = newKp;
          ki = newKi;
          kd = newKd;
          
          // Print updated PID values
          Serial.print("Updated PID values: kp=");
          Serial.print(kp);
          Serial.print(", ki=");
          Serial.print(ki);
          Serial.print(", kd=");
          Serial.println(kd);
        }
      }
    }
};

// Global instance of MotorController
MotorController motorController(ENCA, ENCB, PH_A, EN_A);

// Interrupt Service Routine (ISR) for reading encoder
void readEncoderISR() {
  motorController.updatePosition();  // Update encoder position
}

void setup() {
  Serial.begin(9600);  // Initialize serial communication
  motorController.attachInterrupts(readEncoderISR);  // Attach interrupts for the encoder
}

void loop() {
  motorController.updatePIDValues();  // Update PID values if provided via serial input
  motorController.handleTarget();     // Switch between target positions and control motor
}

```



Tuning a PID controller involves adjusting the three PID parameters—**Proportional (Kp)**, **Integral (Ki)**, and **Derivative (Kd)**—so that your system behaves as desired. The goal is to get the motor (or system) to reach the target position or speed quickly, smoothly, and without too much overshoot or oscillation.

#### 1. **What do Kp, Ki, and Kd do?**

* **Kp (Proportional)**: Controls how hard the motor tries to correct the error. The larger the error, the harder the motor pushes to reach the target. If `Kp` is too high, the motor may overshoot the target (move past it) or oscillate (keep bouncing around the target). If `Kp` is too low, the motor will be slow to reach the target.
* **Ki (Integral)**: Compensates for small, steady errors by adding up the errors over time. It helps eliminate a situation where the motor doesn't quite reach the target (this is called a _steady-state error_). However, if `Ki` is too high, the motor can become unstable and oscillate.
* **Kd (Derivative)**: Helps the motor slow down as it approaches the target by responding to how fast the error is changing. It reduces overshoot and helps the motor stop more smoothly. If `Kd` is too high, it can cause the motor to become too slow to respond or jittery.

#### 2. **Steps to Tune PID**

Tuning the PID controller is usually done through trial and error. Here’s a common method called **manual tuning**:

**Step 1: Set Ki and Kd to 0**

Start by turning off the **integral** and **derivative** actions.

* Set **Ki** = 0
* Set **Kd** = 0
* Only work with **Kp** at first.

**Step 2: Increase Kp until the system starts oscillating**

* Gradually increase **Kp** (Proportional gain) from a low value.
* Observe how the motor responds when it tries to reach the target position.
  * If **Kp** is too low, the motor will respond very slowly.
  * As you increase **Kp**, the motor will reach the target faster.
  * Keep increasing **Kp** until the motor starts oscillating (bouncing back and forth) around the target.

**Step 3: Back off Kp slightly**

* Once the motor starts oscillating, reduce **Kp** slightly until the oscillations stop.
* Now, the motor should move quickly to the target without overshooting too much or oscillating.

**Step 4: Increase Kd to reduce overshoot**

* Now that the system is responding well with **Kp**, add some **Kd** (Derivative gain).
* Slowly increase **Kd** and observe how it affects the motor.
  * **Kd** should help the motor approach the target more smoothly, reducing any overshoot.
  * If **Kd** is too high, the motor will slow down too much before reaching the target.

**Step 5: Add Ki to eliminate steady-state error**

* If the motor reaches the target but doesn’t stay exactly at the target (i.e., it's off by a small amount), increase **Ki** (Integral gain) to help eliminate this steady-state error.
* Increase **Ki** very slowly because high values can cause instability and oscillation.

#### 3. **Example of Manual Tuning**

Let’s say you’re tuning a motor to reach a position using the following steps:

1. **Set Kp = 1.0, Ki = 0.0, Kd = 0.0** and observe the behavior.
   * The motor moves toward the target but takes too long.
2. **Increase Kp to 5.0**, and the motor reaches the target much faster.
   * However, it starts to overshoot the target a little.
3. **Back Kp off to 4.0**, and the motor now reaches the target with little to no overshoot.
4. **Increase Kd to 1.0**, and the motor approaches the target more smoothly without overshoot.
5. If the motor doesn't quite reach the target, **increase Ki** to a small value like 0.1 to help it settle perfectly at the target.

#### 4. **Common Issues**

* **Too much Kp**: Causes the motor to oscillate or overshoot.
* **Too much Ki**: Can make the motor unstable and cause oscillation or "hunting" (constant adjustment without settling).
* **Too much Kd**: Can make the motor respond too slowly or cause jittery movements.

#### 5. **Visualizing the Tuning Process**

Imagine you're driving a car:

* **Kp**: This is how hard you press the gas when you're far from the destination. Too much gas and you overshoot.
* **Ki**: This helps you keep pushing the gas a little longer if you're close but not quite at the destination.
* **Kd**: This is like braking as you approach the destination to avoid overshooting.

#### 6. **Fine-Tuning Tips**

* Start with **Kp**, then add **Kd**, and finally adjust **Ki**.
* If the system becomes unstable, reduce the gains.
* Always use small increments when adjusting **Ki** and **Kd**.

#### 7. **Tools**

If you're working with real hardware, you can use the **Serial Monitor** in Arduino to observe and adjust the values of `Kp`, `Ki`, and `Kd` in real-time as you test the system. This allows you to experiment and see how the system reacts without having to stop the program.
