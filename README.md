# Arduino GamePi

An Arduino Uno game console that uses a Raspberry Pi as its networked game loader.

The Pi hosts a small Flask web app, stores precompiled `.hex` game binaries, and flashes the selected game onto the Arduino over USB with `avrdude`. The Arduino handles the actual gameplay through an OLED display, joystick, button, buzzer, and RGB LED.

<p align="center">
  <img src="Videos/Images_for_readme/img5(1).png" alt="Arduino GamePi web loader" width="900">
</p>

## Games

| Astral Attack 2 | Snake |
| --- | --- |
| <img src="Videos/AstralAttack.gif" alt="Astral Attack gameplay" width="420"> | <img src="Videos/Snake.gif" alt="Snake gameplay" width="420"> |
| Lane-based space shooter with bullets, enemies, scoring, health feedback, and a final sequence. | Classic Snake on a 128x64 OLED grid with food, growth, collision, score, buzzer feedback, and LED brightness scaling. |

## Hardware

<p align="center">
  <img src="Videos/Images_for_readme/img4(1).jpg" alt="Finished Arduino console enclosure" width="360">
  <img src="Videos/Images_for_readme/img1(2).jpg" alt="Internal Arduino wiring" width="360">
</p>

- Arduino Uno
- Raspberry Pi 4
- 1.3-inch I2C SH1106 OLED
- joystick module
- push button
- passive buzzer
- RGB LED
- breadboard, jumper wires, and USB A-to-B cable

## How It Works

```text
Browser
  -> Flask server on Raspberry Pi
  -> selected .hex file
  -> avrdude over USB
  -> Arduino Uno firmware
  -> OLED game runtime
```

The Arduino Uno cannot keep multiple games loaded at once, so the Raspberry Pi acts as the storage and deployment layer. Pick a game in the browser, flash it, and the Arduino restarts directly into that game.

## Repository

```text
Arduino/
  AstralAttack2_130/
  Snake/

Raspberry Pi/
  app.py
  games/
  static/thumbnails/

Videos/
  gameplay videos, GIFs, and README images
```

## Run The Loader

Install Flask on the Raspberry Pi:

```bash
pip install flask
```

Place the repo contents where the loader can find them, or set `GAMECONSOLE_DIR`:

```bash
export GAMECONSOLE_DIR=/home/pi/gameconsole
python3 "Raspberry Pi/app.py"
```

Then open:

```text
http://<raspberry-pi-ip>:5000
```

The app expects the Arduino Uno on `/dev/ttyACM0` and flashes with:

```bash
avrdude -v -patmega328p -carduino -P/dev/ttyACM0 -b115200 -D -Uflash:w:<game>.hex:i
```

## Wiring

<p align="center">
  <img src="wiring_diagram.png" alt="Arduino wiring diagram" width="760">
</p>

| Module | Arduino Pin |
| --- | --- |
| Joystick X | A0 |
| Joystick Y | A1 |
| Joystick switch | D2 |
| Push button | D4 |
| OLED SDA | A4 |
| OLED SCL | A5 |
| Buzzer | D3 |
| RGB LED red | D9 |
| RGB LED green | D10 |
| RGB LED blue | D11 |

## Notes

- Demo videos are in [Videos/](Videos/)
- Precompiled Arduino binaries are in [Raspberry Pi/games/](Raspberry%20Pi/games/)
