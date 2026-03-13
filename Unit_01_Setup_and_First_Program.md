# Unit 1 – Setup & Your First Program
### EV3 MicroPython Robotics

---

## What You'll Learn
By the end of this lesson you will:
- Have the EV3 MicroPython extension installed in VS Code
- Understand how your computer and EV3 brick work together
- Create and run your first MicroPython program on the EV3

**Time:** ~45 minutes  
**What you need:** EV3 Brick (SD card already inserted), Mini-USB cable, Computer with VS Code

---

## Background – How Does This Work?

Here's the big picture before we touch anything:

```
Your Computer                          EV3 Brick
--------------                         ----------
You write code  →  USB Cable  →  Code runs here
in VS Code
```

Your EV3 brick has a **microSD card** inserted in the side of it. That card replaces the normal LEGO software with **MicroPython** — a version of Python designed to run on small computers and robots. You write code on your computer in VS Code, send it to the brick over USB, and it runs.

> **The microSD card must stay in the EV3 at all times.** If it's removed, the brick reverts to the original LEGO firmware and MicroPython won't work.

---

## Part 1 – Install the EV3 MicroPython Extension

VS Code is already on your computer, but we need to add one extension so it knows how to talk to the EV3 brick.

1. Open **Visual Studio Code**
2. Click the **Extensions icon** in the left sidebar (looks like 4 squares)
3. In the search bar, type: `EV3 MicroPython`
4. Find **"LEGO® MINDSTORMS® EV3 MicroPython"** by LEGO Education
5. Click **Install**

When it's done, you'll see a new small brick icon appear in your left sidebar. That's the EV3 panel — you'll use it every time you start a project.

---

## Part 2 – Turn On and Connect the EV3 Brick

1. Press the **dark gray center button** to turn on the EV3
2. Wait for it to fully boot — this takes **1–2 minutes**
3. You'll know it's ready when the **status light turns green**
4. Plug the **mini-USB cable** into the top of the EV3, then into your computer

> **The slow boot is normal.** You'll see a lot of text scrolling on the EV3 screen while it starts up. Just wait for the green light before doing anything else.

---

## Part 3 – Create Your First Project

In MicroPython, your code lives inside a **project folder**. Every project has a file called `main.py` — that's the file the EV3 runs when you start your program.

```
getting_started/     ← your project folder
  └── main.py        ← your code goes here
```

### Steps:

1. In VS Code, click the **EV3 MicroPython icon** (the brick icon in the left sidebar)
2. Click **"Create a new project"**
3. Type `getting_started` as the project name and press **Enter**
4. Choose a location to save it (Desktop or Documents works fine) and click **Choose Folder**

VS Code creates the project and opens `main.py` automatically.

---

## Part 4 – Read the Starter Code

Before we write anything, let's read what's already in `main.py`. You've seen Python before, but some of this will look new — that's fine, we'll go through it line by line.

```python
#!/usr/bin/env pybricks-micropython
# This first line MUST be at the top of every program you write.
# It tells the EV3 brick that this file is a MicroPython program.

from pybricks.hubs import EV3Brick
# Loads the EV3Brick class — gives us control of the brick's
# screen, speaker, buttons, and status light.

from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,
                                  InfraredSensor, UltrasonicSensor, GyroSensor)
# Loads all the motors and sensors we might use.
# We won't use all of these today — they're just here and ready.

from pybricks.parameters import Port, Stop, Direction, Button, Color
# Loads constants — things like Port.B (which port a motor is plugged into)
# or Color.RED. Think of these as a shared vocabulary for the robot.

from pybricks.tools import wait, StopWatch, DataLog
# wait()     → pauses the program for a number of milliseconds
# StopWatch  → a timer
# DataLog    → saves data to a file (useful for experiments later)

from pybricks.robotics import DriveBase
# DriveBase makes it easy to control a two-wheeled robot as a unit.
# We'll use this a lot in later lessons.

# -------------------------------------------------------
# Create your objects here
# -------------------------------------------------------
ev3 = EV3Brick()
# This creates an EV3Brick object and stores it in a variable called ev3.
# From here on, ev3.speaker, ev3.screen, ev3.light all work.

# -------------------------------------------------------
# Write your program here
# -------------------------------------------------------
ev3.speaker.beep()
# Makes the EV3 play a short beep. That's it for the starter code!
```

A few things to notice:
- Lines starting with `#` are **comments** — the EV3 ignores them completely. They're notes for the programmer.
- `from pybricks.X import Y` is the same `import` syntax you've seen in Python — we're just pulling in tools specific to the EV3.
- `ev3 = EV3Brick()` creates an **object**. You'll use `ev3.something` to control almost everything on the brick.

---

## Part 5 – Connect and Run the Starter Program

Let's test that everything is working before we write our own code.

### Connect to the brick:

1. In VS Code, look at the bottom of the left sidebar for **"EV3DEV DEVICE BROWSER"**
2. Click **"Click here to connect to a device"**
3. Your EV3 should appear in the list — click it to connect
4. You'll see a green dot next to the brick name when connected

### Run it:

Press **F5** (or go to **Run → Start Debugging**).

VS Code will copy your files to the EV3 and start the program. You should hear a **short beep** from the brick.

**Nothing happened?** Check:
- USB is plugged in at both ends
- EV3 status light is green (not still booting)
- You clicked the EV3 in the device browser and see a green dot

> **To stop a program early:** Click the red **Stop** square in the VS Code toolbar, or press the **back button** on the EV3 brick.

---

## Part 6 – Your First Custom Program

Now let's write something real. Replace **everything** in `main.py` with the code below. Read every comment — they explain what each line does.

```python
#!/usr/bin/env pybricks-micropython

from pybricks.hubs import EV3Brick
from pybricks.parameters import Color
from pybricks.tools import wait

# Create the EV3Brick object. We need this in every program.
ev3 = EV3Brick()

# -------------------------------------------------------
# SCREEN
# -------------------------------------------------------

# Clear the screen so it's blank before we write anything.
ev3.screen.clear()

# Print lines of text on the EV3 display.
# Each call to print() adds a new line, just like Python's built-in print().
ev3.screen.print("Hello, World!")
ev3.screen.print("EV3 is ready.")

# -------------------------------------------------------
# STATUS LIGHT
# -------------------------------------------------------

# Change the status light color.
# Options: Color.GREEN, Color.RED, Color.ORANGE
ev3.light.on(Color.ORANGE)

# -------------------------------------------------------
# SPEAKER
# -------------------------------------------------------

# beep(frequency, duration)
#   frequency → pitch in Hz. Higher number = higher pitch.
#               440 Hz is the musical note A4 (standard tuning pitch).
#   duration  → how long the beep lasts, in milliseconds.
#               500 ms = half a second.
ev3.speaker.beep(440, 500)

# wait() pauses the program for a set number of milliseconds.
# 1000 ms = 1 second.
wait(1000)

# Play a second beep at a higher pitch.
ev3.speaker.beep(880, 500)   # 880 Hz is exactly one octave above 440 Hz

wait(1000)

# -------------------------------------------------------
# WRAP UP
# -------------------------------------------------------

# Turn the light green to signal the program finished successfully.
ev3.light.on(Color.GREEN)

ev3.screen.print("Done!")

# Wait 2 seconds so we can read the screen before the program exits.
wait(2000)
```

Save the file (`Ctrl+S` on Windows / `Cmd+S` on Mac) and press **F5**.

**What you should see and hear:**
- "Hello, World!" and "EV3 is ready." appear on the EV3 screen
- Status light turns **orange**
- Two beeps play — the second one is higher pitched
- Status light turns **green**
- "Done!" appears on the screen

---

## Lesson Summary

| Code | What it does |
|---|---|
| `#!/usr/bin/env pybricks-micropython` | Required first line — identifies the program type |
| `from pybricks.X import Y` | Loads tools from the pybricks library |
| `ev3 = EV3Brick()` | Creates the brick object so you can control it |
| `ev3.screen.clear()` | Clears the EV3 display |
| `ev3.screen.print("text")` | Prints a line of text on the display |
| `ev3.light.on(Color.X)` | Changes the status light color |
| `ev3.speaker.beep(freq, ms)` | Plays a beep at a given pitch and duration |
| `wait(ms)` | Pauses the program (1000 ms = 1 second) |
| `# comment` | A note in the code — ignored by the EV3 |

---

## Vocabulary

**MicroPython** – A version of Python built to run on small hardware like robots.

**Project** – A folder containing `main.py` and any other files your program needs.

**Object** – A variable that represents a real physical thing (like the EV3 brick). It has its own built-in actions called **methods**.

**Method** – An action that belongs to an object. `ev3.speaker.beep()` calls the `beep` method on the `speaker` that belongs to `ev3`.

**Milliseconds (ms)** – How we measure time in MicroPython. 1000 ms = 1 second.

---

## ✅ Checklist – Before You Move On

- [ ] The EV3 MicroPython extension is installed in VS Code
- [ ] I can turn on the EV3 and wait for the green light
- [ ] I can connect the EV3 to VS Code over USB
- [ ] I can create a new project
- [ ] I can run a program with F5
- [ ] My custom program ran — I saw text on the screen, heard two beeps, and saw the light change
- [ ] I understand what `wait(1000)` does
- [ ] I know what a comment is and why we use them

---

## 🔧 Troubleshooting

| Problem | Try this |
|---|---|
| EV3 won't turn on | Check the batteries |
| Status light is orange and blinking | Still booting — wait for green |
| VS Code can't find the EV3 | Check both ends of the USB cable; try a different USB port |
| Program runs but no sound | The EV3 volume may be turned down — check the brick's settings menu |
| Red underline in the code | Syntax error — look for a typo near the underline |
| Program crashes with an error | Read the message in the **OUTPUT** panel at the bottom of VS Code |

---

*Next up → **Unit 2: Motors & Motion** — you'll start making things move.*
