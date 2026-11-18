# IR Remote Smart Home Automation

A Tinkercad simulation of a microcontroller responding to an infrared remote and switching multiple relay outputs.

![Smart home automation simulation in Tinkercad](<Screenshot 2026-02-07 165350.png>)

## Signal flow

```text
IR remote → IR receiver → NEC command decoder → relay outputs
```

The [Arduino source](Home_Automated_Code) uses pin-change interrupts to capture the timing of incoming pulses. A state machine decodes the NEC command and makes it available to the automation logic.

## What to observe

A supported remote command changes one relay output and updates the LCD. The simulation provides separate on and off commands for the TV, light, fan, and motor outputs.

## Repository contents

- [Source code](Home_Automated_Code)
- [Tinkercad simulation screenshot](<Screenshot 2026-02-07 165350.png>)
