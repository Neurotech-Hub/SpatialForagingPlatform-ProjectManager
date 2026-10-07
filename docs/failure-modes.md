# Failure Modes

What can go wrong on a foraging module, in plain language.

Two kinds of problem show up:

- **The module stops.** Both motors turn off, the status light on top stays
solid on, and the screen shows a fault. It will not deliver another pellet
until someone presses **Recover**. Recover only clears the error. It does
not move the plate back to a home position.
- **The module keeps going.** It tells you something looks off (a dome left
open, a calibration that did not take, a module that never checked in).
Nothing is stuck, and Recover is not required.

The name in parentheses is the label in the software, so you can match what
you read here to what the screen says.

## Errors that stop the module

### Out of pellets (`FeedTimeout`)

The wheel turned for **30 seconds** and no pellet settled on the plate.

A pellet has to sit in the sensor for about **2 seconds** before it counts.
A crumb that flashes through the beam is ignored, and the wheel keeps trying
inside that same 30 seconds.

**Usual cause.** The hopper is empty, a pellet is stuck in the wheel, or the
pellet sensor is unplugged.

**What to do.** Refill or clear the wheel, check that the pellet sensor sees
a pellet, then press Recover.

### Plate jammed on the way up (`Jam`)

The plate started to rise and, **5 seconds** later, it was still sitting on
the lower position sensor. It never got clear of that sensor.

This is not an empty hopper. An empty hopper is “Out of pellets.”

**Usual cause.** Something is blocking the plate, the lift motor is not
moving, or the lower sensor is stuck “on.”

**What to do.** Clear the path, make sure the plate can move up, then press
Recover.

### Pellet fell off on the way up (`PelletLost`)

A pellet was on the plate, the plate started to rise, and the pellet sensor
went empty for **half a second**. The module refuses to offer an empty plate
as if it still had a reward.

This check runs only when a pellet was actually loaded. An empty-plate trial
(no pellet on purpose) does not raise this error.

**Usual cause.** The pellet rolled off, the animal touched it during travel,
or the sensor cable is loose.

**Seen at Hengen Lab (2026-10-05).** A mouse waiting at an open dome can
grab the pellet while the plate is still rising. The sensor goes empty and
the module reports this fault. That is a taken pellet, not one that fell
off. See [the HLAB note](../meetings/20261005_hlab_meeting.md).

**Not built yet.** If the dome is already open, leave the pellet at the
loading position. If the dome opens during the rise, lower the plate
immediately so the animal cannot take the pellet early.

**What to do.** Look at the plate. Do not count this cycle as a pellet that
was offered. Press Recover, then dispense again.

### Plate did not finish its move (`ActuatorTimeout`)

The lift motor was given about **8 seconds** for one part of the move and
did not finish. The screen uses one name for all of these. The line just
before the fault tells you which part it was:


| What it was doing                  | What went wrong                                                                                                                                                                              |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Moving up off the lower sensor     | It did not get clear in time.                                                                                                                                                                |
| Moving down to pick up a pellet    | It never found the lower sensor. It tries once more only if it already knows the plate is below that sensor. A second miss stops the module, so it does not drive upward into the hard stop. |
| Dropping a little past that sensor | It found the sensor, then did not finish the short extra drop where the pellet lands.                                                                                                        |
| Lifting the plate to the animal    | It did not reach the top in time. If it never even leaves the lower sensor, you will usually see **Plate jammed** first.                                                                     |


**Usual cause.** The lift motor is stalled, unplugged, or blocked, or the
lower sensor never sees the plate.

**Seen at Hengen Lab (2026-10-05).** One node does not raise all the way to
the top. The cause is still open. See
[the HLAB note](../meetings/20261005_hlab_meeting.md).

**What to do.** Free the plate, check the lift motor and the lower sensor,
then press Recover.

### Waiting for the other module (not an error)

On a trial where this module is not supposed to give a pellet, it lowers,
then waits until the other module starts to rise, and rises with it. There
is no timer. If the other module never rises, this plate simply stays down
until someone presses Recover. The screen does not call this a fault.

## Messages that do not stop the module

### Dome left open (`DomeOpenWarning`)

The dome has been open for **30 seconds**. The module says so once, then
keeps working. Closing the dome and opening it again allows a new warning.

The top status light does not change. The small onboard light follows the
dome: lit means open.

**What to do.** Close the dome. If it will not close on its own, check the
spring and that nothing is holding it up.

### Module never appeared (`NotInitialized`)

The module could not start its network connection, so the base station never
hears from it. There is no fault message to clear, because the module is not
on the network. The status light stays in its fast boot blink.

**What to do.** Power-cycle the module. If it still never appears, the
network hardware on that board needs a look, or the firmware needs to be
loaded again.

### Odd fault code (`InvalidData`)

The software knows this name, but current modules never send it. If a log
shows it, that message did not come from this firmware.

### Presence calibration did not take

Calibrating the “is an animal here?” pad takes about **5 seconds** with an
empty cage. If it does not collect enough quiet readings, it fails and keeps
the old setting. The confirm blink does not play.

Starting a second calibration while one is already running does nothing. It
is not a failure.

**What to do.** Make sure the cage is empty and still, then calibrate again.

### A setting was rejected

If the base station sends a setting the module cannot use (for example a
blank or nonsense presence sensitivity), the module answers “not applied”
and keeps the old value. The module does not stop. A heartbeat that is set
too fast is slowed to a safe rate and still counts as applied.

### Module never got an ID

The status light blinks slowly (about once a second). The module is on, but
it is waiting for the base station to claim it. It will keep asking. It does
not call this a fault, and it will not pass power down the chain to the next
module until it is claimed.

**What to do.** Check that the first cable from the base station is seated,
and that the modules are chained in order. Holding the module button for
about **3 seconds** forgets its saved ID so it can be claimed again.

### A message was missed

If the network is briefly too busy, one update can be dropped. The module
does not stop. The next regular status update fills in the gap: whether it
is faulted, how many pellets it has offered, and how many were taken.

### Dispense did nothing

A dispense is ignored while the module is already moving, waiting, or
stopped on a fault. It does not announce the ignore. Press Recover if it is
faulted, then dispense again.

A pellet already sitting on the plate is not an error. The module skips
loading another one and offers the one that is already there.

## What you see


| What you see                                  | What it means                                                     |
| --------------------------------------------- | ----------------------------------------------------------------- |
| Small light blinking quickly                  | The module is turning on.                                         |
| Top light blinking slowly                     | It is on and waiting to be given an ID.                           |
| Top light off                                 | It is ready. Not faulted.                                         |
| Top light solid on                            | It has stopped on a fault. Press Recover after you fix the cause. |
| Top light blinks fast for a couple of seconds | Someone pinged this module, so you can see which box it is.       |
| Screen: “Out of pellets…”                     | The wheel never delivered a pellet.                               |
| Screen: “Jam — load sensor still blocked…”    | The plate did not get off the lower sensor.                       |
| Screen: “Pellet lost…”                        | The pellet left the plate while it was rising.                    |
| Screen: “Actuator fault…”                     | The lift did not finish its move.                                 |


## Module looks offline

The module does not decide this itself. The base station marks it offline
when it has been silent for about **three times** its usual check-in.

In normal use that check-in is every minute, so a module looks offline after
about **3 minutes** of silence. Right after power-up, before the base station
has set that pace, the window is shorter (about 15 seconds).

**What to do.** Check the network cable, that the module has power, and that
the end of the chain is terminated. A module that never started its network
connection (see above) will stay silent until it is power-cycled or reloaded.
When it speaks again, the base station marks it online.

## Cross-references

- `[dispense-cycle.md](dispense-cycle.md)` — how a normal pellet delivery works.
- `[function-checks.md](function-checks.md)` — how to confirm a module is healthy.
- `[maintenance.md](maintenance.md)` — checks that prevent these failures.
- Technical constants and message layouts live in the [SFM](https://github.com/Neurotech-Hub/SFM) firmware docs (`firmware/docs/`).

