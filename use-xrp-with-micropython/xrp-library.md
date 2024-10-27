# XRP library

The micro-python git is&#x20;

{% @github-files/github-code-block url="https://github.com/micropython" %}

The XRP miropython git&#x20;

{% @github-files/github-code-block url="https://github.com/Open-STEM/XRP_MicroPython" %}

Examples in order from XRP\_MicroPython/Examples/drive\_examples.py

straight motion:

```python
def drive_straight(drive_time: float = 1): # default parameter with default of 1 second
    drivetrain.set_effort(0.8, 0.8) # sets same effort to both wheels to go straight
    time.sleep(drive_time)
    drivetrain.stop()
```

circular motion:

```python
def arc_turn(turn_time: float = 1):
    drivetrain.set_effort(0.5, 0.8) # sets diff effort to both wheels for circular motion
    time.sleep(turn_time)
    drivetrain.stop()
```

turn:

```python
def swing_turn(turn_time: float = 1):
    drivetrain.set_effort(0, 0.8) # sets effort to only wheel so the robot turns in one place
    time.sleep(turn_time)
    drivetrain.stop()

```

circle:

```python
def circle():
    while True:
        drivetrain.set_effort(0.8, 1) # applies arc turn infinitely so it moves in a circle
```

square:

```python
def square(sidelength):
    for sides in range(4): # iterates through loop 4 times
        drivetrain.straight(sidelength, 0.8) # draws one side
        drivetrain.turn(90) # turns 90 degrees
        
    # could also use polygon(sidelength, 4)
```

polygon

```python
def polygon(side_length, number_of_sides): # basically math of any polyon 
    for s in range(number_of_sides):
        drivetrain.straight(side_length)
        drivetrain.turn(360/number_of_sides)
```

log of data from the ultrasonic sensor&#x20;

```python
def ultrasonic_test():
    while True:
        print(rangefinder.distance()) # continuously prints distance reported from sensor
        time.sleep(0.1)
```

drive till reaching wall

```python
def drive_till_close(target_distance: float = 10.0):
    speed = 0.6
    while rangefinder.distance() > target_distance:
        drivetrain.set_effort(speed, speed) # just drives forward till target distance reached
        time.sleep(0.01)
    drivetrain.set_effort(0, 0)
```

maintain same distance from wall

```python
def standoff(target_distance: float = 10.0):
    KP = 0.2
    while True:
        distance = rangefinder.distance() 
        error = distance - target_distance
        drivetrain.set_effort(error * KP, error*KP) # proportional control to adjust for error
        time.sleep(0.01)
```

