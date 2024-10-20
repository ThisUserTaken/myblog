# Control the motor

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



### Class based&#x20;

```arduino
#include <Arduino.h>

// Pin definitions
#define ENCA 4  // Encoder A pin
#define ENCB 5  // Encoder B pin
#define PH_A 6  // Motor A phase pin (controls direction)
#define EN_A 7  // Motor A enable pin (controls speed)

// EncoderMotorController class to encapsulate the PID motor control logic
class EncoderMotorController {
  private:
    int pinA;  // Pin for ENCA
    int pinB;  // Pin for ENCB
    int pinPhase;  // Pin for motor direction control (PH_A)
    int pinEnable; // Pin for motor speed control (EN_A)
    volatile int position;  // Current encoder position
    long previousTime;  // Previous time for PID calculation
    float previousError;  // Previous error for derivative calculation
    float errorIntegral;  // Accumulated error for integral calculation

    // PID parameters
    float kp, ki, kd;  // Proportional, Integral, Derivative gains

  public:
    // Constructor to initialize the pins and variables
    EncoderMotorController(int encoderPinA, int encoderPinB, int motorPhasePin, int motorEnablePin) {
      pinA = encoderPinA;
      pinB = encoderPinB;
      pinPhase = motorPhasePin;
      pinEnable = motorEnablePin;

      position = 0;
      previousTime = 0;
      previousError = 0;
      errorIntegral = 0;

      // Set default PID parameters (can be changed later)
      kp = 1.0;
      ki = 0.0;
      kd = 0.0;

      // Setup pins
      pinMode(pinA, INPUT);
      pinMode(pinB, INPUT);
      pinMode(pinPhase, OUTPUT);
      pinMode(pinEnable, OUTPUT);
    }

    // Attach interrupt to the encoder pin
    void attachInterrupts(void (*isr)()) {
      attachInterrupt(digitalPinToInterrupt(pinA), isr, RISING);
    }

    // Function to update the encoder position (to be called in ISR)
    void updatePosition() {
      position += digitalRead(pinB) ? -1 : 1;  // Increment or decrement based on ENCB
    }

    // Function to calculate and apply PID control
    void controlMotor(int targetPosition) {
      long currentTime = micros();
      float deltaTime = (currentTime - previousTime) / 1.0e6;  // Convert time to seconds
      previousTime = currentTime;

      // Safely read the position
      noInterrupts();
      int currentPosition = position;
      interrupts();

      // Calculate error and its derivative
      int error = currentPosition - targetPosition;
      float errorDerivative = (error - previousError) / deltaTime;

      // Accumulate the integral of the error
      errorIntegral += error * deltaTime;

      // Calculate PID control signal
      float controlSignal = kp * error + kd * errorDerivative + ki * errorIntegral;

      // Constrain the control signal to allowable motor power range (0-255)
      float motorPower = constrain(fabs(controlSignal), 0, 255);

      // Set motor direction based on control signal
      digitalWrite(pinPhase, controlSignal > 0 ? LOW : HIGH);  // LOW = reverse, HIGH = forward

      // Apply motor speed using PWM
      analogWrite(pinEnable, motorPower);

      // Store current error for the next iteration
      previousError = error;

      // Debug output
      Serial.print("Target: ");
      Serial.print(targetPosition);
      Serial.print(" | Position: ");
      Serial.print(currentPosition);
      Serial.print(" | Power: ");
      Serial.println(motorPower);
    }

    // Function to set PID parameters (for tuning)
    void setPID(float p, float i, float d) {
      kp = p;
      ki = i;
      kd = d;
    }

    // Get the current position (optional if needed externally)
    int getPosition() {
      return position;
    }
};

// Global instance of EncoderMotorController class
EncoderMotorController motorController(ENCA, ENCB, PH_A, EN_A);

// Interrupt Service Routine (ISR)
void readEncoderISR() {
  motorController.updatePosition();  // Update the encoder's position in the ISR
}

void setup() {
  Serial.begin(9600);  // Initialize serial communication
  motorController.attachInterrupts(readEncoderISR);  // Attach the interrupt for the encoder
}

void loop() {
  // Target position for the motor
  int targetPosition = 1200;

  // Control the motor to reach the target position
  motorController.controlMotor(targetPosition);

  // Add a delay for readability in the serial monitor
  delay(100);
}

```

