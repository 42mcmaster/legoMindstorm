# Unit 2 – Motors & Motion
### EV3 MicroPython — Step-by-Step Walkthrough

**What you need:** EV3 Brick + Large Motor plugged into **Port B**, USB cable, VS Code

---

## 🔧 Setup — Do this first

Create a new project folder in VS Code called `motors_intro`. Open `main.py` and paste this **exactly**:

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.ev3devices import Motor
from pybricks.parameters import Port, Stop, Button
from pybricks.tools import wait

ev3 = EV3Brick()
motor = Motor(Port.B)

ev3.speaker.beep()
```

▶ **Run it (F5).** You should hear a beep. The motor won't move yet — that's expected.

> **Show Mr. M:** EV3 beeped and program exited cleanly.

---

## Step 1 — `run()` — Spin the motor

`motor.run(speed)` spins the motor until you tell it to stop.
Speed is in **degrees per second**. Positive = forward. Negative = backward.

**Replace everything below `ev3.speaker.beep()` with this:**

```python
motor.run(360)   # spin at 360 deg/s (one full rotation per second)
wait(2000)       # wait 2 seconds while it spins
motor.stop()     # cut power, motor coasts to a stop
```

▶ **Run it.** The motor should spin for 2 seconds and stop.

**Try both of these changes — one at a time:**
1. Change `360` to `720` — what's different?
2. Change `360` to `-360` — what happens?

> **Show Mr. M:** Motor spins, you can explain what changing the number does.

---

## Step 2 — `run_time()` — Run for a set duration

`motor.run_time(speed, milliseconds)` is cleaner than `run()` + `wait()`.
The program **pauses automatically** until the motor finishes — no separate `wait()` needed.

**Replace the motor lines with this:**

```python
motor.run_time(500, 2000)    # 500 deg/s for 2000 ms (2 seconds)

ev3.speaker.beep()           # this only runs AFTER the motor finishes
```

▶ **Run it.** Motor runs 2 seconds, then you hear the beep.

**Try this:** Change `2000` to `500`. Notice how the beep comes sooner?

> **Show Mr. M:** Beep happens only after motor stops.

---

## Step 3 — `run_angle()` — Rotate a specific amount

`motor.run_angle(speed, degrees)` rotates the motor by a **relative** amount from wherever it currently is.

**Replace the motor lines with this:**

```python
motor.run_angle(360, 360)    # rotate forward exactly 360 degrees (one full turn)
wait(500)
motor.run_angle(360, -180)   # rotate backward 180 degrees
wait(500)
motor.run_angle(360, 90)     # rotate forward 90 degrees
```

▶ **Run it.** Watch the motor make distinct, measured movements.

**Try this:** Change the middle line to `-360`. Does it go back to where it started?

> **Show Mr. M::** Three distinct movements visible, explain what each line does.

---

## Step 4 — `run_target()` — Go to an exact position

`motor.run_target(speed, angle)` moves to an **absolute angle** — like positions on a clock.
The motor remembers its starting point as `0°` and always navigates from there.

**Replace the motor lines with this:**

```python
motor.run_target(360, 90)    # go to 90-degree position
wait(1000)
motor.run_target(360, 270)   # go to 270-degree position
wait(1000)
motor.run_target(360, 0)     # return to start
wait(500)
motor.hold()                 # lock in place at the end
```

▶ **Run it.** The motor should snap to 3 different positions, then return home.

**Try this:** After the program runs, try pushing the motor shaft by hand. It fights back — that's `motor.hold()`.

> **Show Mr. M:** Three distinct positions, can explain difference between `run_angle` and `run_target`.

---

## New Concept — `for` loops

Before the challenges, you need one new idea.

A `for` loop repeats a block of code a set number of times:

```python
for i in range(3):
    print("This prints 3 times")
    wait(500)
```

- `range(3)` means "repeat 3 times" (counts 0, 1, 2 — but you usually don't need `i` for anything)
- Everything **indented** under the loop repeats
- The loop finishes and the program continues normally

**Quick test — try this in a new file:**

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.tools import wait

ev3 = EV3Brick()

for i in range(4):
    ev3.speaker.beep()
    wait(500)
```

▶ **Run it.** Should beep exactly 4 times. Change `4` to `2` — only 2 beeps?

> **Show Mr. M:** Beeps the right number of times, can explain what `range()` does.

---

## Challenge 1 — Back and Forth

**Goal:** Motor rotates 180° forward, pauses, 180° backward, pauses. Do this 3 times.

**Fill in the loop body — do NOT change anything outside the loop:**

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
    # 1. Rotate 180 degrees forward
    # 2. Wait 500 ms
    # 3. Rotate 180 degrees backward
    # 4. Wait 500 ms
    pass   # delete this line when you add your code

ev3.speaker.beep(880, 500)
```

> **Show Mr. M:** Motor does exactly 3 back-and-forth cycles, then beeps at the end.

---

## Challenge 2 — Clock Hand

**Goal:** Move the motor to 12, 3, 6, and 9 o'clock positions (0°, 90°, 180°, 270°), pause 1 second at each, print which position you're at, then return to 0°.

**Fill in the movements:**

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
# Move to each position in order: 0, 90, 180, 270, back to 0
# At each position:
#   - Print the position name (e.g., "12 oclock", "3 oclock", etc.)
#   - Wait 1000 ms before moving to the next one
# Use run_target() for each move
# End with motor.hold()

```

> **Show Mr. M:** Motor hits all 4 positions cleanly, screen prints the right labels, ends held at 0°.

---

## ✅ Checklist

- [ ] Setup — heard the startup beep
- [ ] Step 1 — `run()` works, tested positive/negative speeds
- [ ] Step 2 — `run_time()` works, beep fires only after motor stops
- [ ] Step 3 — `run_angle()` makes 3 distinct moves
- [ ] Step 4 — `run_target()` hits exact positions, `hold()` resists being pushed
- [ ] For loops — beep test works, can explain `range()`
- [ ] Challenge 1 — Back and forth, exactly 3 times
- [ ] Challenge 2 — Clock hand hits all 4 positions with labels

---

*Next → **Unit 3: Driving with Two Motors***
