# Measure position

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



### with classes

````arduino
#define ENCA 4  // GPIO pin for encoder output A (Yellow wire)
#define ENCB 5  // GPIO pin for encoder output B (White wire)

// Encoder class to encapsulate encoder functionality
class Encoder {
  private:
    int pinA;  // Pin for ENCA
    int pinB;  // Pin for ENCB
    volatile int position;  // Position variable to track the encoder's position

  public:
    // Constructor to initialize pins
    Encoder(int encoderPinA, int encoderPinB) {
      pinA = encoderPinA;
      pinB = encoderPinB;
      position = 0;  // Initialize position to 0

      // Set encoder pins as input
      pinMode(pinA, INPUT);
      pinMode(pinB, INPUT);
    }

    // Attach the interrupt to the encoder pin
    void attachInterrupts(void (*isr)()) {
      attachInterrupt(digitalPinToInterrupt(pinA), isr, RISING);  // Interrupt on rising edge of ENCA
    }

    // Function to update the position (called in the ISR)
    void updatePosition() {
      if (digitalRead(pinB) == HIGH) {
        position++;  // Increment position if ENCB is HIGH
      } else {
        position--;  // Decrement position if ENCB is LOW
      }
    }

    // Safely read the position value
    int getPosition() {
      noInterrupts();  // Disable interrupts to safely read the volatile variable
      int posCopy = position;  // Copy the current position
      interrupts();  // Re-enable interrupts
      return posCopy;  // Return the copied position
    }
};

// Global instance of Encoder class
Encoder encoder(ENCA, ENCB);

// Interrupt Service Routine (ISR)
void readEncoderISR() {
  encoder.updatePosition();  // Call the encoder's position update method
}

void setup() {
  Serial.begin(9600);  // Initialize serial communication
  encoder.attachInterrupts(readEncoderISR);  // Attach the interrupt for the encoder
}

void loop() {
  // Read and print the encoder position
  int currentPosition = encoder.getPosition();  // Safely read the encoder's position
  Serial.println(currentPosition);  // Print the current position to the serial monitor
  delay(100);  // Delay to avoid spamming the serial monitor
}

```
````

