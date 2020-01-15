# Config Options
## AutonomousTime
The amount of time, in seconds, that the Autonomous period will last.

## TeleoperatedTime
The amount of time, in seconds, that the Teleoperated period will last.

## CountdownTime
The number of seconds of countdown given before the match starts.

## PauseTime
The amount of time, in seconds, between the Autonomous period ending and the Teleoperated period being enabled.

## GameStringOverride
See the table below for choosing a value:

| -1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Override disabled, randomly chosen | Red: "R" Blue: "R" | Red: "R" Blue: "G" | Red: "R" Blue: "B" | Red: "R" Blue: "Y" | Red: "G" Blue: "R" | Red: "G" Blue: "G" | Red: "G" Blue: "B" | Red: "G" Blue: "Y" | Red: "B" Blue: "R" | Red: "B" Blue: "G" | Red: "B" Blue: "B" | Red: "B" Blue: "Y" | Red: "Y" Blue: "R" | Red: "Y" Blue: "G" | Red: "Y" Blue: "B" | Red: "Y" Blue: "Y" |

## RedAllianceCount
The number of robots on the red alliance.

## BlueAllianceCount
The number of robots on the blue alliance.
  
# Default Config
>AutonomousTime:15\
TeleoperatedTime:135\
CountdownTime:3\
PauseTime:3\
GameStringOverride:-1\
RedAllianceCount:3\
BlueAllianceCount:3

[Return Home](index.md)