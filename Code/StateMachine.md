#State Machine

##Inputs & Outputs
Two Inputs

* ARM (active low)
* CUT (active high)

One Output

* RELAY (active low)

##States
###1. Initialization
```
Set RELAY to HIGH (!cut). Transition to state 2
```

###2. Fault
```
IF CUT or ARM
    Reset Timer
    Goto 2
ELSE
    IF TIMER < CooldownTimer
        Increment Timer
        Goto 2
    ELSE
        Reset TIMER
        Goto 3
```

###3.To Disarm
```
IF CUT or ARM
    Reset Timer
    Goto 2
ELSE
    IF TIMER < ToDisarmTimer
        Increment Timer
        Goto 3
    ELSE
        Reset Timer
        Goto 4
```

###4. Disarmed
```
IF CUT
    Reset Timer
    Goto 2
ELSE
    IF ARM
        Goto 5
    ELSE
        Goto 4
```

###5.To Arm
```
IF CUT
    Reset Timer
    Goto 2
ELSE
    IF ARM
        IF TIMER < ToArmTimer
        Increment Timer
        Goto 5
    ELSE
        Reset Timer
        Goto 4
```

###6.ARM
```
IF ARM
    IF CUT
        Goto 7
    ELSE
        Goto 6
ELSE
    Goto 4
```

###7.CUT
```
    SET RELAY to LOW (cut)
    DELAY CUT_TIME
    SET RELAY to HIGH (!cut)
    Goto 8
```

###8.Cooldown
```
    DELAY COOLDOWN_TIME
    GOTO 4
```
