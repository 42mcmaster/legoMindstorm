# Unit 2 – Motors & Motion
### EV3 MicroPython Robotics

---

## What You'll Learn
By the end of this lesson you will:
- Connect and control a single EV3 motor
- Understand the difference between speed, angle, and time-based movement
- Know when to use `stop()`, `brake()`, and `hold()`
- Complete coding challenges using motor commands

**Time:** ~60 minutes  
**What you need:** EV3 Brick, Large Motor, cable, Mini-USB cable, Computer

---

## Background – How EV3 Motors Work

EV3 motors aren't like simple toy motors that just spin when you apply power. They have a built-in **rotation sensor** that tracks exactly how far the motor has turned, measured in **degrees**.

```
One full rotation = 360 degrees

         0°
         |
270° ----+---- 90°
         |
        180°
```

This means you can tell a motor to rotate to a precise angle, run for an exact amount of time, or maintain a specific speed — and the EV3 will use the sensor to make sure it actually does it accurately.

### Motor Ports
Motors plug into the **output ports** on the top of the EV3 brick, labeled **A, B, C, and D**.

```
Top of EV3 Brick:
[ A ]  [ B ]  [ C ]  [ D ]
```

For this lesson, plug your **Large Motor into Port B**.

---

## Part 1 – Setup

Create a new project in VS Code called `motors_intro`. Then copy this starting code into `main.py`. We'll build on it throughout the lesson.

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop
from pybricks.tools import wait

# Create the EV3 brick object.
ev3 = EV3Brick()

# Create a motor object connected to Port B.
# This is how we tell MicroPython which port the motor is plugged into.
motor = Motor(Port.B)

# Play a beep so we know the program started.
ev3.speaker.beep()
```

Save and run this (`F5`). You should hear a beep but the motor won't move yet — we haven't told it to do anything.

---

## Part 2 – Your First Motor Commands

There are several ways to make a motor move. Let's go through each one.

### `motor.run(speed)` — Run forever at a set speed

This tells the motor to spin continuously until you give it a new command. Speed is in **degrees per second (deg/s)**.

```python
# Run the motor at 360 degrees per second (one full rotation per second).
# A positive number spins the motor clockwise.
# A negative number spins it counterclockwise.
motor.run(360)

# Wait 2 seconds while the motor runs.
wait(2000)

# Stop the motor. (More on stop types in Part 4.)
motor.stop()
```

> **Try it:** What happens if you change `360` to `720`? What about `-360`?

---

### `motor.run_time(speed, duration)` — Run for a set amount of time

This is cleaner than `run()` + `wait()` because the motor handles its own timing. The program waits automatically until the motor is done.

```python
# Run at 500 deg/s for 2000 milliseconds (2 seconds), then stop.
# The program pauses here until the motor finishes — it doesn't move on early.
motor.run_time(500, 2000)

# This line won't execute until the motor has finished its 2 seconds.
ev3.speaker.beep()
```

The `then` parameter controls what happens when the motor finishes:

```python
# After 2 seconds, the motor holds its position (default behavior).
motor.run_time(500, 2000, then=Stop.HOLD)

# After 2 seconds, the motor coasts to a stop freely.
motor.run_time(500, 2000, then=Stop.COAST)

# After 2 seconds, the motor brakes (resists movement passively).
motor.run_time(500, 2000, then=Stop.BRAKE)
```

---

### `motor.run_angle(speed, angle)` — Rotate by a specific number of degrees

This rotates the motor by a relative amount from wherever it currently is.

```python
# Rotate 360 degrees (one full turn) at 500 deg/s.
motor.run_angle(500, 360)

wait(500)

# Rotate 180 degrees backward (negative angle = reverse direction).
motor.run_angle(500, -180)
```

---

### `motor.run_target(speed, target)` — Rotate to a specific angle

This moves the motor to an **absolute position**. The motor remembers its starting angle as 0°, so `run_target(500, 180)` always goes to the 180° position regardless of where the motor currently is.

```python
# Go to the 90-degree position.
motor.run_target(500, 90)

wait(500)

# Now go to the 180-degree position.
motor.run_target(500, 180)

wait(500)

# Go back to 0 degrees (the starting position).
motor.run_target(500, 0)
```

> **Notice:** The direction is chosen automatically based on where the motor needs to go. You don't have to worry about positive/negative speed for `run_target()`.

---

## Part 3 – Reading Motor Values

You can ask the motor what it's currently doing at any point in your program.

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

# Start the motor spinning.
motor.run(360)

# Read and print the motor's current angle and speed while it runs.
# We'll do this 5 times with a short pause between each reading.
for i in range(5):
    current_angle = motor.angle()    # How far it has rotated, in degrees
    current_speed = motor.speed()    # Current speed, in deg/s

    # Print to the EV3 screen and also to the VS Code output panel.
    ev3.screen.print("A:" + str(current_angle) + " S:" + str(current_speed))
    print("Angle:", current_angle, "Speed:", current_speed)

    wait(500)

motor.stop()
```

Save and run this. Watch both the EV3 screen and the **OUTPUT** panel at the bottom of VS Code. You should see the angle increasing as the motor spins.

---

## Part 4 – Stop Types

When you stop a motor, there are three different behaviors. The difference is small but matters when you need precision.

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

# --- COAST ---
# The motor cuts power and spins freely until friction stops it.
# The motor will drift a little after stopping.
motor.run(500)
wait(1000)
motor.stop()        # stop() always coasts

wait(1000)

# --- BRAKE ---
# The motor resists movement passively using electrical resistance.
# Stops faster than coast, but can still be pushed.
motor.run(500)
wait(1000)
motor.brake()

wait(1000)

# --- HOLD ---
# The motor actively fights to stay exactly where it stopped.
# Try pushing the motor shaft — it will push back.
motor.run(500)
wait(1000)
motor.hold()

wait(2000)
```

**Try this after running the program:** After `motor.hold()` activates, try turning the motor shaft by hand. Then try the same after `motor.stop()`. Feel the difference?

| Stop Type | Power Used | Resists Being Pushed? | Best Used When... |
|---|---|---|---|
| `stop()` / COAST | None | No | Precision doesn't matter |
| `brake()` | None | A little | You want it to stop faster |
| `hold()` | Yes | Strongly | You need it to stay exactly in place |

---

## Part 5 – Putting It Together

Here's a complete example that uses everything from this lesson. Read through it carefully before running it.

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.screen.clear()
ev3.screen.print("Motor Demo")
ev3.speaker.beep()

# --- Step 1: Spin for 2 seconds ---
ev3.screen.print("Running 2 sec...")
motor.run_time(360, 2000)      # 360 deg/s for 2 seconds

wait(500)

# --- Step 2: Rotate exactly 360 degrees ---
ev3.screen.print("One rotation")
motor.run_angle(360, 360)      # 360 deg/s, rotate 360 degrees

wait(500)

# --- Step 3: Go to specific positions ---
ev3.screen.print("To 90 degrees")
motor.run_target(360, 90)

wait(500)

ev3.screen.print("To 270 degrees")
motor.run_target(360, 270)

wait(500)

# --- Step 4: Return home ---
ev3.screen.print("Back to 0")
motor.run_target(360, 0)

wait(500)

# --- Done ---
motor.hold()                   # Hold position at the end
ev3.screen.print("Done!")
ev3.speaker.beep(880, 300)
```

---

## Challenges

Now it's your turn. For each challenge, the key information is given to you — you need to write the logic.

---

### Challenge 1 – Back and Forth

**Goal:** Make the motor rotate 180 degrees forward, pause, then rotate 180 degrees backward. Repeat this 3 times.

**What you need:**
- `motor.run_angle(speed, angle)` — use a negative angle to go backward
- `wait(ms)` — pause between movements
- A `for` loop — `for i in range(3):` repeats the indented block 3 times

**Starter code — fill in the loop body:**

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.speaker.beep()

for i in range(3):
    # YOUR CODE HERE
    # 1. Rotate the motor 180 degrees forward
    # 2. Pause for 500 ms
    # 3. Rotate the motor 180 degrees backward
    # 4. Pause for 500 ms
    pass    # remove this line when you add your code

ev3.speaker.beep(880, 500)
ev3.screen.print("Done!")
```

---

### Challenge 2 – Speed Ramp

**Goal:** Make the motor run through 4 different speeds in sequence — slow, medium, fast, then stop. Print the current speed to the screen before each change.

**What you need:**
- `motor.run_time(speed, duration)` — run at a speed for a set time
- `ev3.screen.print("text")` — display what's happening
- Run at these speeds in order: `200`, `400`, `600`, `800` deg/s, each for `1500` ms

**Starter code — fill in the sequence:**

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.screen.clear()
ev3.screen.print("Speed Ramp")
ev3.speaker.beep()

# YOUR CODE HERE
# Run the motor at each of these speeds for 1500 ms each.
# Print the speed to the screen before each run.
# Speeds: 200, 400, 600, 800

# When done, hold the motor in place and print "Done!"
```

---

### Challenge 3 – Clock Hand

**Goal:** Program the motor to move like a clock hand, stopping at 12, 3, 6, and 9 o'clock positions (0°, 90°, 180°, 270°). Pause at each position for 1 second and print which position you're at.

**What you need:**
- `motor.run_target(speed, angle)` — moves to an absolute angle
- `wait(ms)` — hold at each position
- `ev3.screen.print("text")` — label each stop
- Angles to hit: `0`, `90`, `180`, `270`, then back to `0`

**Starter code — fill in the movements:**

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.screen.clear()
ev3.screen.print("Clock Hand")
ev3.speaker.beep()

# YOUR CODE HERE
# Move the motor to each position in order: 0, 90, 180, 270, 0
# At each position:
#   - Print the position name (e.g., "12 oclock" or "3 oclock")
#   - Wait 1 second before moving to the next one
# Use run_target() so the motor always goes to the exact right spot.
# End with motor.hold() to lock in the final position.
```

---

### Challenge 4 – User Controlled (Stretch)

**Goal:** Use the EV3 brick's buttons to control the motor. Pressing the **right button** runs the motor forward. Pressing the **left button** runs it backward. Pressing the **center button** stops the program.

**What you need:**
- `ev3.buttons.pressed()` — returns a list of which buttons are currently held down
- `Button.RIGHT`, `Button.LEFT`, `Button.CENTER` — button constants
- `motor.run(speed)` — run continuously (call this inside a loop)
- `motor.stop()` — stop the motor
- A `while True:` loop that keeps checking buttons until center is pressed

**Starter code — fill in the button logic:**

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Button
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.screen.clear()
ev3.screen.print("Right = Forward")
ev3.screen.print("Left  = Backward")
ev3.screen.print("Center = Quit")
ev3.speaker.beep()

# YOUR CODE HERE
# Write a while True loop that:
#   - Checks which buttons are pressed using ev3.buttons.pressed()
#   - If RIGHT is pressed: run the motor forward at 360 deg/s
#   - If LEFT is pressed:  run the motor backward at 360 deg/s
#   - If neither is pressed: stop the motor
#   - If CENTER is pressed: break out of the loop
#
# Hint: ev3.buttons.pressed() returns a LIST of buttons.
#       To check if a button is in that list:
#           if Button.RIGHT in ev3.buttons.pressed():
#
# Add a short wait(10) at the bottom of the loop to avoid overloading the brick.

ev3.screen.print("Stopped.")
motor.stop()
```

---

## Lesson Summary

| Command | What it does |
|---|---|
| `Motor(Port.B)` | Creates a motor object on Port B |
| `motor.run(speed)` | Spins continuously at a set speed (deg/s) |
| `motor.run_time(speed, ms)` | Runs for a set number of milliseconds |
| `motor.run_angle(speed, deg)` | Rotates by a relative number of degrees |
| `motor.run_target(speed, deg)` | Rotates to an absolute angle |
| `motor.stop()` | Stops and coasts freely |
| `motor.brake()` | Stops with passive resistance |
| `motor.hold()` | Stops and actively holds position |
| `motor.angle()` | Returns current rotation angle in degrees |
| `motor.speed()` | Returns current speed in deg/s |

---

## ✅ Checklist – Before You Move On

- [ ] I can create a Motor object on Port B
- [ ] I understand the difference between `run_angle()` and `run_target()`
- [ ] I understand the difference between `stop()`, `brake()`, and `hold()`
- [ ] I completed Challenges 1, 2, and 3
- [ ] I attempted Challenge 4 (stretch)

---

*Next up → **Unit 3: Driving with Two Motors** — you'll build a robot that drives and turns.*
