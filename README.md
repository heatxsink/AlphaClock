# AlphaClock
Software for Alpha Clock Five from Evil Mad Scientist Laboratories

Complete documentation here: http://wiki.evilmadscientist.com/Alpha_Clock_Five

## Building

The firmware was written for Arduino 1.0.3. It builds with current tools using
[arduino-cli](https://arduino.github.io/arduino-cli/) and the
[MightyCore](https://github.com/MCUdude/MightyCore) board package for the ATmega644:

```sh
arduino-cli config add board_manager.additional_urls \
  https://mcudude.github.io/MightyCore/package_MCUdude_MightyCore_index.json
arduino-cli core update-index
arduino-cli core install MightyCore:avr
arduino-cli lib install Time DS1307RTC

FQBN="MightyCore:avr:644:variant=modelA,pinout=sanguino,clock=16MHz_external"
arduino-cli compile -b "$FQBN" --library alphafive alphafive/examples/AlphaClock
arduino-cli upload  -b "$FQBN" -p /dev/ttyUSB0 alphafive/examples/AlphaClock
```

Upload goes over an FTDI cable to the clock's serial bootloader. The upload step
has not been verified against the bootloader that ships on the clock; if it fails,
check the bootloader baud rate against the Evil Mad Scientist wiki.

## Unix epoch mode

Displays the current Unix time (seconds since 1970-01-01 UTC). It takes ten digits,
so it is meant for two Alpha Clock Five units daisy-chained together: the first
unit computes the time and sends the other half to the second unit once per second.
The second unit only needs firmware v2.0 or newer and no configuration.

Setup, on the first unit (the one upstream in the daisy chain):

1. Hold `+` and `-` for two seconds to enter the settings menu; the time-set button
   steps forward through menu items, alarm-set steps back.
2. Go to `UTC` and set the offset of the clock's local time from UTC, in 15-minute
   steps (e.g. `-07:00` for PDT). The clock has no DST rules; adjust this when you
   change the clock for daylight saving, or keep the clock on UTC and leave it at `+00:00`.
3. Go to `TIME AND...` and choose:
   - `EPO L` if the first unit sits on the left (it shows the high five digits), or
   - `EPO R` if the first unit sits on the right (it shows the low five digits).

Selecting any other `TIME AND...` option returns the second unit to its own clock display.

While in epoch mode, the first unit also:

- pulses its rear night light once per second, and tells the second unit to pulse with it (`MP` command);
- sends its display brightness to the second unit, so changing brightness with `+`/`-` on the first unit changes both.

Leaving epoch mode restores both units' configured night light.
