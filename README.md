# AlphaClock
Software for Alpha Clock Five from Evil Mad Scientist Laboratories

Complete documentation here: http://wiki.evilmadscientist.com/Alpha_Clock_Five

## Unix epoch mode

Displays the current Unix time (seconds since 1970-01-01 UTC). It takes ten digits,
so it is meant for two Alpha Clock Five units daisy-chained together: the first
unit computes the time and sends the other half to the second unit once per second.
The second unit only needs firmware v2.0 or newer and no configuration.

Setup, on the first unit (the one upstream in the daisy chain):

1. Hold the time-set and alarm-set buttons to enter the settings menu.
2. Go to `UTC` and set the offset of the clock's local time from UTC, in 15-minute
   steps (e.g. `-07:00` for PDT). The clock has no DST rules; adjust this when you
   change the clock for daylight saving, or keep the clock on UTC and leave it at `+00:00`.
3. Go to `TIME AND...` and choose:
   - `EPO L` if the first unit sits on the left (it shows the high five digits), or
   - `EPO R` if the first unit sits on the right (it shows the low five digits).

Selecting any other `TIME AND...` option returns the second unit to its own clock display.
