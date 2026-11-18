# IR Remote Smart Home Automation

A Tinkercad simulation of a microcontroller responding to an infrared remote and switching multiple relay outputs.

![Smart home automation simulation in Tinkercad](<Screenshot 2026-02-07 165350.png>)

## Signal flow

```text
IR remote → IR receiver → NEC command decoder → relay outputs
```

The [Arduino source](Home_Automated_Code) uses pin-change interrupts to capture the timing of incoming pulses. A state machine decodes the NEC command and makes it available to the automation logic.

The sketch maps remote buttons to TV, light, fan, and motor outputs. Its LCD displays the selected action and prompts for the next command.

## Repository contents

- [Source code](Home_Automated_Code)
- [Tinkercad simulation screenshot](<Screenshot 2026-02-07 165350.png>)
