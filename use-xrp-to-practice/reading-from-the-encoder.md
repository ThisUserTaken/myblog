# Reading from the Encoder

### The connections

* Encoder A output → DO pin 4
* Encoder B output → DO pin 5

```arduino
#define ENCA 4 // Yellow
#define ENCB 5 // White

void setup() {
  Serial.begin(9600);
  pinMode(ENCA,INPUT);
  pinMode(ENCB,INPUT);
}

void loop() {
  int a = digitalRead(ENCA);
  int b = digitalRead(ENCB);
  Serial.print(a*5); 
  Serial.print(" ");
  Serial.print(b*5);
  Serial.println();
}
```

output of this will be like

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Measure position

To implement the encoder position tracking using interrupts on the RP2040 (e.g., on a Raspberry Pi Pico), you can follow the approach you described. This involves:

1. **Using interrupts**: To trigger a function when output A of the encoder rises.
2. **Volatile variable**: To ensure the `posi` variable is properly shared between the loop and interrupt service routines (ISRs).
3. **Atomic operations**: To safely read and modify the `posi` variable without interference from interrupts.

The `volatile` keyword ensures that the compiler doesn’t optimize away accesses to `posi`, and an atomic block (or its equivalent in RP2040) is used to prevent race conditions when reading or writing `posi` during an interrupt.

```arduino

#define ENCA 4  // GPIO pin for encoder output A (Yellow wire)
#define ENCB 5  // GPIO pin for encoder output B (White wire)

volatile int posi = 0;  // Position variable (shared between ISR and loop)

void readEncoder();  // Function prototype for the ISR

void setup() {
  Serial.begin(9600);  // Begin serial communication

  pinMode(ENCA, INPUT);  // Set encoder A pin as input
  pinMode(ENCB, INPUT);  // Set encoder B pin as input

  attachInterrupt(digitalPinToInterrupt(ENCA), readEncoder, RISING);  // Attach interrupt to ENCA rising edge
}

void loop() {
  // Safely read the position variable with atomic block to prevent interrupts during read
  noInterrupts();  // Disable interrupts
  int posiCopy = posi;  // Copy the volatile variable
  interrupts();  // Re-enable interrupts
  
  Serial.println(posiCopy);  // Print the current position
  delay(100);  // Small delay to avoid spamming the serial output
}

// Interrupt Service Routine (ISR) that triggers when ENCA rises
void readEncoder() {
  // Read ENCB to determine the direction
  if (digitalRead(ENCB) == HIGH) {
    posi++;  // If ENCB is HIGH, increment position
  } else {
    posi--;  // If ENCB is LOW, decrement position
  }
}
```



### Control the motor

To construct the feedback control loop, you must:

1. Define a target position
2. Compute the error e(t) as the difference between the measured position and the target
3. Compute a control signal u(t)

The code below shows the implementation I used for the Arduino.

```arduino
#include <Arduino.h>

// Pin definitions
#define ENCA 4  // Encoder A pin
#define ENCB 5  // Encoder B pin
#define PH_A 6  // Motor A phase pin (controls direction)
#define EN_A 7  // Motor A enable pin (controls speed)

// Variables to track motor position and time
volatile int position = 0;      // Current encoder position
long previousTime = 0;          // Time for previous PID calculation
float previousError = 0;        // Error from last loop for derivative calculation
float errorIntegral = 0;        // Accumulated error for integral term

void setup() {
  // Initialize serial communication
  Serial.begin(9600);

  // Set encoder pins as inputs
  pinMode(ENCA, INPUT);
  pinMode(ENCB, INPUT);

  // Attach interrupt to read encoder changes on rising edge
  attachInterrupt(digitalPinToInterrupt(ENCA), updateEncoder, RISING);

  // Set motor control pins as outputs
  pinMode(PH_A, OUTPUT);
  pinMode(EN_A, OUTPUT);
}

void loop() {
  // Target position for the motor
  int targetPosition = 1200;

  // Calculate time elapsed since last loop
  long currentTime = micros();
  float deltaTime = (currentTime - previousTime) / 1.0e6; // Convert microseconds to seconds
  previousTime = currentTime;

  // Safely read the encoder position
  noInterrupts();
  int currentPosition = position;
  interrupts();

  // Calculate error and its derivative
  int error = currentPosition - targetPosition;
  float errorDerivative = (error - previousError) / deltaTime;

  // Calculate the integral of the error
  errorIntegral += error * deltaTime;

  // PID control parameters
  float kp = 1.0;   // Proportional gain
  float ki = 0.0;   // Integral gain
  float kd = 0.0;   // Derivative gain

  // Calculate the control output (PID formula)
  float controlSignal = kp * error + kd * errorDerivative + ki * errorIntegral;

  // Constrain the control signal to allowable motor power range (0-255)
  float motorPower = constrain(fabs(controlSignal), 0, 255);

  // Set motor direction based on control signal
  digitalWrite(PH_A, controlSignal > 0 ? LOW : HIGH);

  // Apply motor speed (using PWM)
  analogWrite(EN_A, motorPower);

  // Store current error for the next iteration
  previousError = error;

  // Output for debugging
  Serial.print("Target: ");
  Serial.print(targetPosition);
  Serial.print(" | Position: ");
  Serial.print(currentPosition);
  Serial.print(" | Power: ");
  Serial.println(motorPower);
}

// Interrupt function to update encoder position
void updateEncoder() {
  // Update position based on direction of rotation
  position += digitalRead(ENCB) ? -1 : 1;
}
ard
```

