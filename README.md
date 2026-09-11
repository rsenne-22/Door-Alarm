# ESP32 Door Alarm

A simple door-monitoring alarm built on an ESP32. A reed switch detects when the door is opened, and if the door opens **without** the disarm button being pressed, the buzzer sounds and the LED lights up until the door is closed (or the button is pressed to silence it).

# About
This is a personal hobby project built to explore embedded systems and IoT fundamentals. This project idea came to me as a device to alert me and my family if my little brother opens the front door and tries to leave by himself. 

# Skills Demonstrated
1. Embedded C/C++ programming on ESP32 (Arduino framework)
2. Digital I/O: reading sensors (reed switch, button) and driving outputs (buzzer, LED)
3. Basic state machine design (armed / disarmed / triggered)
4. Circuit design and wiring (pull-up resistors, current limiting, switch debouncing)
5. Debugging and iterative hardware testing

## How It Works

1. The **door reed sensor** detects the open/closed state of the door.
2. Pressing the **button** disarms the alarm for a set window (when you want to open the door normally).
3. If the door opens while the system is **armed** (button not pressed), the **active buzzer** and **LED** turn on.
4. Closing the door and/or pressing the button resets the alarm back to idle state.

## Hardware

| Component | Purpose |
| ESP32 dev board | Main microcontroller |
| Reed switch + magnet | Detects door open/closed |
| Push button | Disarms the alarm before opening the door |
| Active buzzer | Audible alert |
| LED | Visual alert |
| Resistors | Pull-up/pull-down for button and reed switch, current-limiting for LED |

## Wiring

| Component | ESP32 Pin |
| Reed switch | 6 |
| Button | 42 |
| Buzzer | 41 |
| LED | 40 |

- Reed switch: one leg to the GPIO pin, other leg to GND. Used INPUT_PULLUP in software so the pin reads LOW when the door is closed (switch closed) and HIGH when open (switch open).
- Button: same pull-up approach — pin reads LOW when pressed.
- Buzzer: connect directly to a GPIO pin and GND.
- LED: GPIO pin → resistor → LED → GND.

## Behavior / Logic

- **Idle/Armed:** Door closed, system waiting.
- **Disarm:** Button press starts a grace period during which opening the door does *not* trigger the alarm.
- **Triggered:** Door opens while armed → buzzer + LED turn on.
- **Reset:** Door closes again, and/or button is pressed, silences the buzzer/LED and returns to idle.

## Possible Improvements

- Add a delay/timeout so the buzzer auto-silences after a set time.
- Add Wi-Fi notifications (e.g., push notification)) when triggered.
- Add a status LED for "armed" vs "disarmed" state.
- Battery-power the unit with a low-power sleep mode between checks.
