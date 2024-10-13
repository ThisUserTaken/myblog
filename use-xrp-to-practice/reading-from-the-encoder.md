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
