# Build the body → leg check (~30 min)

Everything's in this kit. No app, no account, no internet needed for this part.

## 1. Flash MicroPython
Hold **BOOTSEL**, plug the Pico into USB → a drive appears. Drag
`firmware/RPI_PICO2_W-*.uf2` onto it. It reboots.

## 2. Copy the firmware on
Paste in a terminal (or use the free **Thonny** app to drag these on):
```
mpremote cp firmware/PicoRobotics.py :PicoRobotics.py
mpremote cp firmware/act_engine.py   :act_engine.py
mpremote cp firmware/hello_legs.py   :main.py
mpremote reset
```
`PicoRobotics.py` **auto-detects your board** — a carrier board (Kitronik /
PCA9685) is found over I2C; direct-wired servos run on **GP0 (left) /
GP1 (right)**. Nothing to configure.

## 3. Wiring (direct-wire only — skip if you have a carrier board)
| servo | signal (orange) → | red → | brown → |
|---|---|---|---|
| **left**  | **GP0** (pin 1) | battery **+** rail | shared **GND** |
| **right** | **GP1** (pin 2) | battery **+** rail | shared **GND** |

Servos draw power from the **battery rail, never a Pico pin**. Carrier board?
Legs plug into **ports 1 & 3** (port 2 is the dead socket).

## 4. Run on battery → center & glue the legs
**Unplug USB, power on the battery.** `main.py` runs the 4-check wiggle **twice**,
then **holds both servos at 90°** (slow LED blink):

> 1 CENTER · 2 RIGHT leg · 3 LEFT leg · 4 BOTH sweep → then holds at 90

While it holds at 90, **super-glue each leg on pointing straight out** (keep glue
off the servo shaft + case seam). Power-cycle and run again to confirm both legs
sweep evenly and sit straight.

**Both legs sweep + straight at rest = ✅ done. Send a clip!**

**Power:** ~5–6V. Easiest = 4×AA. Rechargeable = 1S LiPo + a 5V boost/charge
module. (A bare 3.7V LiPo works short-term, but don't let it drain too low or
you'll damage the cell.)

---
*Direct-wire (GP0/GP1) is **experimental — not yet hardware-tested**; please report
back. Other controller (ESP32, Pi, …)? See `PROTOCOL.md` and run `conformance.html`
against your board. The phone-brain web app is the next drop — then `main.py`
becomes `robot-server.py`.*
