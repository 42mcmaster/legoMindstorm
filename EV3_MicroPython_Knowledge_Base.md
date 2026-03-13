# EV3 MicroPython Curriculum Knowledge Base
**Source:** Pybricks EV3 MicroPython v2.0.0 Documentation  
**Reference:** https://pybricks.com/ev3-micropython/  
**Companion Guide:** Getting Started with LEGO® MINDSTORMS® Education EV3 MicroPython v2.0.0

---

## Table of Contents
1. [Setup & Installation](#1-setup--installation)
2. [Hardware Overview](#2-hardware-overview)
3. [EV3 Brick (Hub) – Core Features](#3-ev3-brick-hub--core-features)
4. [Motors](#4-motors)
5. [Sensors](#5-sensors)
6. [Parameters & Constants](#6-parameters--constants)
7. [Tools – Timing & Data Logging](#7-tools--timing--data-logging)
8. [Robotics – DriveBase](#8-robotics--drivebase)
9. [Example Programs](#9-example-programs)
10. [Suggested Curriculum Units](#10-suggested-curriculum-units)

---

## 1. Setup & Installation

### What You Need
| Item | Notes |
|---|---|
| Windows 10 or Mac OS computer | Admin access required only during install |
| microSD card | 4GB–32GB (microSDHC); Class A1 recommended |
| microSD card reader | Built-in or external USB |
| Mini-USB cable | Included with EV3 set |
| EV3 Brick | LEGO® MINDSTORMS® EV3 |

### Step-by-Step Setup
1. **Download** [Visual Studio Code](https://code.visualstudio.com/Download) and install it.
2. In VS Code, open the **Extensions** tab and install the **EV3 MicroPython** extension (search "EV3 MicroPython" by LEGO Education).
3. **Download** the EV3 MicroPython microSD card image (~360 MB, do NOT unzip).  
   → https://education.lego.com/en-us/support/mindstorms-ev3/python-for-ev3
4. **Download and install** [Etcher](https://www.balena.io/etcher/) (SD card flashing tool).
5. Insert the microSD card into your computer, open Etcher, select the image, select your card, and click **Flash!**
6. Turn the EV3 Brick **off**, insert the flashed microSD card (text side up), then power on.

### Turning the EV3 On/Off
- **On:** Press the dark gray center button. Boot takes ~1–2 minutes; status light turns **green** when ready.
- **Off:** Press the **back button** → select **Power Off** using the center button.

### Returning to Original LEGO Firmware
1. Turn the EV3 off and wait for all lights to turn off.
2. Remove the microSD card.
3. Turn the EV3 on — it boots the original LEGO firmware automatically.

---

## 2. Hardware Overview

### EV3 Brick Ports
| Port | Type | Label |
|---|---|---|
| Motor ports | Output | A, B, C, D |
| Sensor ports | Input | S1, S2, S3, S4 |

### Compatible Devices
**EV3 Motors:**
- Large Motor (medium torque, high speed)
- Medium Motor (fast, lower torque)

**EV3 Sensors:**
- Touch Sensor
- Color Sensor
- Infrared Sensor & Beacon
- Ultrasonic Sensor
- Gyroscopic (Gyro) Sensor

**NXT Devices** (also compatible): Touch, Light, Color, Ultrasonic, Sound, Temperature, Energy Meter, Vernier Adapter

---

## 3. EV3 Brick (Hub) – Core Features

### Import
```python
from pybricks.hubs import EV3Brick
ev3 = EV3Brick()
```

### Buttons
```python
from pybricks.parameters import Button

pressed = ev3.buttons.pressed()  # Returns list of Button constants
# Button constants: Button.LEFT, Button.RIGHT, Button.UP, Button.DOWN,
#                   Button.CENTER, Button.LEFT_UP, Button.LEFT_DOWN,
#                   Button.RIGHT_UP, Button.RIGHT_DOWN, Button.BEACON
```

### Status Light
```python
from pybricks.parameters import Color

ev3.light.on(Color.RED)    # Turn light on with color
ev3.light.on(Color.GREEN)
ev3.light.on(Color.ORANGE)
ev3.light.off()            # Turn light off
```

### Speaker
```python
# Basic beep: frequency (Hz), duration (ms)
ev3.speaker.beep()                  # Default: 500 Hz, 100 ms
ev3.speaker.beep(1000, 500)         # 1000 Hz for 500 ms

# Play musical notes
ev3.speaker.play_notes(['C4/4', 'E4/4', 'G4/4'], tempo=120)
# Note format: [Note Name][Octave]/[Division]
# Example: 'C4/4' = middle C, quarter note; 'R/4' = rest

# Play sound file
ev3.speaker.play_file('path/to/sound.wav')

# Text to speech
ev3.speaker.say("Hello, world!")
ev3.speaker.set_speech_options(language='en', voice='f1', speed=130, pitch=50)

# Volume
ev3.speaker.set_volume(80)  # 0–100%
```

### Screen
```python
ev3.screen.clear()                              # Clear screen
ev3.screen.print("Hello!")                      # Print text (auto-scrolls)
ev3.screen.draw_text(10, 20, "Hi", text_color=Color.BLACK)
ev3.screen.draw_line(0, 0, 100, 50)            # x1,y1,x2,y2
ev3.screen.draw_box(10, 10, 80, 50)            # x1,y1,x2,y2
ev3.screen.draw_circle(64, 64, 30)             # x_center, y_center, radius
ev3.screen.load_image('picture.png')           # Display image centered
ev3.screen.save('screenshot.png')             # Save screen to file
# Screen size: 178 x 128 pixels
```

### Battery
```python
voltage = ev3.battery.voltage()   # Returns mV
current = ev3.battery.current()   # Returns mA
```

---

## 4. Motors

### Import
```python
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Direction, Stop

motor = Motor(Port.B)                                        # Basic
motor = Motor(Port.B, positive_direction=Direction.COUNTERCLOCKWISE)
motor = Motor(Port.B, gears=[12, 36])                       # With gear train
```

### Measuring
```python
motor.speed()        # Current speed in deg/s
motor.angle()        # Accumulated rotation in degrees
motor.reset_angle(0) # Reset angle to 0 (or any value)
```

### Stopping
```python
motor.stop()   # Let motor spin freely (coast)
motor.brake()  # Passive resistance (back-EMF braking)
motor.hold()   # Actively hold current position
```

### Running Commands
```python
# Run at constant speed indefinitely
motor.run(500)                             # 500 deg/s

# Run for a set time
motor.run_time(500, 2000)                  # 500 deg/s for 2000 ms
motor.run_time(500, 2000, then=Stop.COAST) # Coast after stop

# Run by a relative angle
motor.run_angle(500, 90)                   # Turn 90° at 500 deg/s

# Run to an absolute target angle
motor.run_target(500, 180)                 # Go to 180° at 500 deg/s

# Run until stalled (useful for calibration)
angle = motor.run_until_stalled(200)       # Returns angle when stalled

# Raw DC (no speed control)
motor.dc(50)    # 50% power
```

### Stop Types (then= parameter)
| Constant | Behavior |
|---|---|
| `Stop.COAST` | Motor spins freely |
| `Stop.BRAKE` | Passive resistance |
| `Stop.HOLD` | Actively holds angle |

---

## 5. Sensors

### Touch Sensor
```python
from pybricks.ev3devices import TouchSensor

touch = TouchSensor(Port.S1)
if touch.pressed():
    print("Button pressed!")
```

### Color Sensor
```python
from pybricks.ev3devices import ColorSensor
from pybricks.parameters import Color

color_sensor = ColorSensor(Port.S3)

# Detect color
c = color_sensor.color()
# Returns: Color.BLACK, Color.BLUE, Color.GREEN, Color.YELLOW,
#          Color.RED, Color.WHITE, Color.BROWN, or None

# Measure ambient light (0–100%)
ambient = color_sensor.ambient()

# Measure surface reflection with red light (0–100%)
reflection = color_sensor.reflection()

# Measure RGB reflection
r, g, b = color_sensor.rgb()    # Each value: 0–100%
```

### Ultrasonic Sensor
```python
from pybricks.ev3devices import UltrasonicSensor

ultra = UltrasonicSensor(Port.S4)

distance_mm = ultra.distance()           # Distance in mm
distance_mm = ultra.distance(silent=True)  # Turns off emitter after reading

# Detect other ultrasonic sensors
heard = ultra.presence()    # True if another ultrasonic detected
```

### Infrared Sensor & Beacon
```python
from pybricks.ev3devices import InfraredSensor

ir = InfraredSensor(Port.S4)

dist = ir.distance()                  # Relative distance 0–100
dist, angle = ir.beacon(channel=1)   # Distance + angle to beacon (-75 to 75°)
buttons = ir.buttons(channel=1)      # List of Button constants pressed on remote
all_buttons = ir.keypad()            # Detect all 4 up/down buttons independently
```

### Gyro Sensor
```python
from pybricks.ev3devices import GyroSensor

gyro = GyroSensor(Port.S2)

angle = gyro.angle()          # Accumulated angle in degrees
speed = gyro.speed()          # Angular velocity in deg/s
gyro.reset_angle(0)           # Reset angle to 0

# WARNING: Do NOT use both angle() and speed() in the same program
# — reading speed() resets the angle to 0.
```

---

## 6. Parameters & Constants

### Port
```python
from pybricks.parameters import Port

# Motor ports
Port.A, Port.B, Port.C, Port.D

# Sensor ports
Port.S1, Port.S2, Port.S3, Port.S4
```

### Direction
```python
from pybricks.parameters import Direction

Direction.CLOCKWISE          # Default positive direction
Direction.COUNTERCLOCKWISE
```

### Stop
```python
from pybricks.parameters import Stop

Stop.COAST   # Let spin freely
Stop.BRAKE   # Passive resistance
Stop.HOLD    # Active position hold (encoders only)
```

### Color
```python
from pybricks.parameters import Color

Color.BLACK, Color.BLUE, Color.GREEN, Color.YELLOW
Color.RED, Color.WHITE, Color.BROWN, Color.ORANGE, Color.PURPLE
```

### Button
```python
from pybricks.parameters import Button

Button.LEFT_UP, Button.UP, Button.RIGHT_UP
Button.LEFT,    Button.CENTER, Button.RIGHT
Button.LEFT_DOWN, Button.DOWN, Button.RIGHT_DOWN
Button.BEACON   # IR remote beacon button
```

---

## 7. Tools – Timing & Data Logging

### Import
```python
from pybricks.tools import wait, StopWatch, DataLog
```

### wait()
```python
wait(1000)   # Pause program for 1000 ms (1 second)
```

### StopWatch
```python
timer = StopWatch()

ms = timer.time()   # Current elapsed time in ms
timer.pause()
timer.resume()
timer.reset()       # Reset to 0; keeps running/paused state
```

### DataLog (CSV Logger)
```python
# Create a log file with column headers
data = DataLog('time', 'angle', name='my_log', timestamp=True, extension='csv')

# Add a row of data
data.log(timer.time(), motor.angle())

# File is saved to EV3 brick; upload via VS Code device browser (right-click → Upload)
# Default filename: log_YYYY_MM_DD_HH_MM_SS.csv
```

---

## 8. Robotics – DriveBase

### Import & Setup
```python
from pybricks.robotics import DriveBase
from pybricks.ev3devices import Motor
from pybricks.parameters import Port

left_motor  = Motor(Port.B)
right_motor = Motor(Port.C)

robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)
# wheel_diameter: diameter of wheels in mm
# axle_track: distance between wheel ground contact points in mm
```

### Driving Commands
```python
# Drive straight (blocks until complete)
robot.straight(500)    # Forward 500 mm
robot.straight(-200)   # Backward 200 mm

# Turn in place (blocks until complete)
robot.turn(90)         # Right 90°
robot.turn(-45)        # Left 45°

# Drive continuously (non-blocking)
robot.drive(200, 0)    # 200 mm/s forward, no turn
robot.drive(100, 30)   # 100 mm/s, turning right at 30 deg/s
robot.stop()           # Stop and coast
```

### Settings
```python
robot.settings(
    straight_speed=200,          # mm/s
    straight_acceleration=100,   # mm/s²
    turn_rate=100,               # deg/s
    turn_acceleration=200        # deg/s²
)

# Get current settings (no arguments)
current = robot.settings()
```

### Measuring
```python
robot.distance()   # Estimated driven distance in mm
robot.angle()      # Estimated rotation angle in degrees
robot.state()      # Returns (distance, speed, angle, turn_rate)
robot.reset()      # Reset distance and angle to 0
```

### Calibration Tips
- Drive 1000 mm and measure actual distance.
  - Too short → decrease `wheel_diameter`
  - Too far → increase `wheel_diameter`
- Turn 360° and check if robot returns to start.
  - Under-rotates → increase `axle_track`
  - Over-rotates → decrease `axle_track`

---

## 9. Example Programs

### Boilerplate Template
```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor, TouchSensor, ColorSensor, \
    InfraredSensor, UltrasonicSensor, GyroSensor
from pybricks.parameters import Port, Stop, Direction, Button, Color
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.robotics import DriveBase

ev3 = EV3Brick()

# Write your program here
ev3.speaker.beep()
```

### Example 1: Hello World (Beep + Screen)
```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import EV3Brick
from pybricks.tools import wait

ev3 = EV3Brick()

ev3.screen.print("Hello, World!")
ev3.speaker.beep(440, 500)
wait(2000)
```

### Example 2: Move a Single Motor
```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port

ev3 = EV3Brick()
test_motor = Motor(Port.B)

ev3.speaker.beep()
test_motor.run_target(500, 90)   # Rotate to 90°
ev3.speaker.beep(1000, 500)
```

### Example 3: Basic Robot Movement (DriveBase)
```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port
from pybricks.robotics import DriveBase

ev3 = EV3Brick()
left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)

robot.straight(1000)    # Forward 1 meter
ev3.speaker.beep()
robot.straight(-1000)   # Back 1 meter
ev3.speaker.beep()
robot.turn(360)         # Full clockwise spin
ev3.speaker.beep()
robot.turn(-360)        # Full counterclockwise spin
ev3.speaker.beep()
```

### Example 4: Obstacle Avoidance (Ultrasonic Sensor)
```python
#!/usr/bin/env pybricks-micropython
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor, UltrasonicSensor
from pybricks.parameters import Port
from pybricks.tools import wait
from pybricks.robotics import DriveBase

ev3 = EV3Brick()
obstacle_sensor = UltrasonicSensor(Port.S4)
left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)

ev3.speaker.beep()

while True:
    robot.drive(200, 0)                        # Drive forward continuously

    while obstacle_sensor.distance() > 300:   # Wait until obstacle within 300 mm
        wait(10)

    robot.straight(-300)                       # Back up
    robot.turn(120)                            # Turn around
```

### Example 5: Line Following (Color Sensor)
```python
#!/usr/bin/env pybricks-micropython
from pybricks.ev3devices import Motor, ColorSensor
from pybricks.parameters import Port
from pybricks.tools import wait
from pybricks.robotics import DriveBase

left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
line_sensor = ColorSensor(Port.S3)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)

BLACK = 9
WHITE = 85
threshold = (BLACK + WHITE) / 2

DRIVE_SPEED = 100
PROPORTIONAL_GAIN = 1.2

while True:
    deviation = line_sensor.reflection() - threshold
    turn_rate = PROPORTIONAL_GAIN * deviation
    robot.drive(DRIVE_SPEED, turn_rate)
    wait(10)
```

---

## 10. Suggested Curriculum Units

### Unit 1 – Setup & First Program
**Learning Goals:** Install the environment, understand project structure, run a first program.  
**Activities:**
- Install VS Code + EV3 MicroPython extension
- Flash the microSD card with Etcher
- Create a new project in VS Code
- Edit `main.py` to make the EV3 beep and print "Hello" on screen
- Run the program via USB (F5)

**Key Concepts:** Project folders, `main.py`, boilerplate imports, `EV3Brick`, `speaker.beep()`, `screen.print()`

---

### Unit 2 – Motors & Motion
**Learning Goals:** Control individual motors; understand speed, angle, and stopping modes.  
**Activities:**
- Attach a Large Motor to Port B
- Run motor forward and backward at different speeds
- Use `run_target()` to move to precise angles
- Compare `stop()`, `brake()`, and `hold()`
- Log motor angle over time with `DataLog`

**Key Concepts:** `Motor`, `Port`, `run()`, `run_time()`, `run_angle()`, `run_target()`, `Stop.COAST/BRAKE/HOLD`, `Direction`

---

### Unit 3 – Driving with Two Motors (DriveBase)
**Learning Goals:** Build and calibrate a two-wheeled robot; drive precise distances and angles.  
**Activities:**
- Build the Robot Educator driving base
- Measure `wheel_diameter` and `axle_track`
- Program the robot to drive a square path
- Calibrate measurements using `straight(1000)` and `turn(360)`

**Key Concepts:** `DriveBase`, `straight()`, `turn()`, `drive()`, `stop()`, `settings()`, calibration

---

### Unit 4 – Sensors: Touch & Color
**Learning Goals:** Read sensor values; use conditionals to respond to environment.  
**Activities:**
- Wire a Touch Sensor and detect button presses
- Use the Color Sensor to detect surface colors
- Program the robot to stop when a button is pressed
- Sort objects by color using `color_sensor.color()`

**Key Concepts:** `TouchSensor`, `ColorSensor`, `pressed()`, `color()`, `reflection()`, `rgb()`, `if/else`, `while` loops

---

### Unit 5 – Sensors: Ultrasonic & Infrared
**Learning Goals:** Measure distance; build reactive behaviors.  
**Activities:**
- Measure distances with the Ultrasonic Sensor
- Build an obstacle avoidance robot
- Use the IR remote to control the robot manually
- Compare ultrasonic vs. infrared distance sensing

**Key Concepts:** `UltrasonicSensor`, `InfraredSensor`, `distance()`, `buttons()`, `while True` loop, threshold logic

---

### Unit 6 – Gyro Sensor & Precision Turning
**Learning Goals:** Use the Gyro Sensor for accurate rotation; understand drift.  
**Activities:**
- Mount a Gyro Sensor on the robot
- Turn exactly 90° using gyro feedback vs. DriveBase
- Measure gyro drift over time
- Build a program that drives a straight line using gyro correction

**Key Concepts:** `GyroSensor`, `angle()`, `speed()`, `reset_angle()`, control loops, sensor drift

---

### Unit 7 – Timing, Loops & Data Logging
**Learning Goals:** Use timers and loops; collect and analyze data.  
**Activities:**
- Use `wait()` to create timed sequences
- Use `StopWatch` to measure program execution time
- Use `DataLog` to record motor speed vs. time
- Upload the CSV log and graph it in a spreadsheet

**Key Concepts:** `wait()`, `StopWatch`, `DataLog`, `for`/`while` loops, CSV, data analysis

---

### Unit 8 – EV3 Output: Screen, Lights & Sound
**Learning Goals:** Create user-visible feedback using screen drawing, lights, and audio.  
**Activities:**
- Draw shapes and text on the EV3 screen
- Change the status light color based on sensor readings
- Play a musical melody with `play_notes()`
- Use `say()` for text-to-speech output

**Key Concepts:** `screen.draw_box()`, `screen.draw_circle()`, `light.on()`, `speaker.play_notes()`, `speaker.say()`

---

### Unit 9 – Capstone Project
**Learning Goals:** Combine all skills to build and program an autonomous robot.  
**Suggested Projects:**
- **Maze solver:** Navigate a maze using ultrasonic and touch sensors
- **Color sorter:** Sort colored blocks using the color sensor and a motor-driven arm
- **Line follower competition:** Optimize PID gain for fastest lap time
- **Remote-controlled vehicle:** Use IR remote + Bluetooth messaging to control a robot

---

## Quick Reference Card

### Common Imports
```python
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor, TouchSensor, ColorSensor, UltrasonicSensor, GyroSensor, InfraredSensor
from pybricks.parameters import Port, Stop, Direction, Button, Color
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.robotics import DriveBase
```

### Units Cheat Sheet
| Measurement | Unit |
|---|---|
| Time | milliseconds (ms) |
| Angle | degrees (°) |
| Rotational speed | degrees/second (deg/s) |
| Distance | millimeters (mm) |
| Linear speed | mm/s |
| Brightness/percentage | % (0–100) |
| Voltage | millivolts (mV) |
| Current | milliamps (mA) |
| Frequency (sound) | Hz |

### Useful Links
| Resource | URL |
|---|---|
| Full API docs | https://pybricks.com/ev3-micropython/ |
| Installation guide | https://pybricks.com/ev3-micropython/startinstall.html |
| Motors reference | https://pybricks.com/ev3-micropython/ev3devices.html |
| Robotics module | https://pybricks.com/ev3-micropython/robotics.html |
| Parameters | https://pybricks.com/ev3-micropython/parameters.html |
| Example programs | https://pybricks.com/ev3-micropython/examples/robot_educator_basic.html |
| Building instructions | https://education.lego.com/en-us/support/mindstorms-ev3/building-instructions |
