# IR Remote Smart Home Automation

An Arduino Uno and Tinkercad project that translates infrared remote commands into digital output changes. The sketch demonstrates how an embedded application can decode a timed signal in the background, dispatch the resulting command, and show feedback on a 16×2 LCD.

![Tinkercad reference showing an IR remote, Arduino Uno, breadboard, and motor](<Screenshot 2026-02-07 165350.png>)

The screenshot shows the remote, receiver circuit, and one motor path. The source describes the fuller application, including four logical appliance channels and LCD prompts.

## System architecture

```text
IR remote
    |
    v
IR receiver on D9
    |
    v
AVR pin-change interrupt -> NEC timing decoder -> command queue
                                                    |
                                                    v
                                               Arduino loop
                                                /         \
                                      Digital outputs    16x2 LCD
```

1. The IR receiver presents an active-low signal to D9. The pin-change interrupt handler timestamps signal edges using `micros()`.
2. The decoder state machine interprets the start pulse and subsequent durations. A complete 32-bit frame supplies the address, command, and inverse command byte. The receiver also recognizes a repeat frame.
3. Decoded frames enter a fixed-size ring buffer. The main loop reads a queued frame, skips repeat frames, and dispatches the command byte.
4. Each on or off case writes a digital level to the mapped channel. The LCD displays the action and, for several on commands, prompts for the corresponding off command.

## Pin and command map

| Function | Arduino pin | Notes |
| --- | --- | --- |
| IR receiver signal | D9 | NEC pulse input |
| TV channel | D13 | Command values 16 and 17 |
| Light channel | D7 | Command values 18 and 20 |
| Fan channel | D6 | Command values 21 and 22 |
| Motor channel | D10 | Command values 25 and 24 |
| LCD RS and enable | D12 and D11 | Order follows the `LiquidCrystal` constructor |
| LCD data D4–D7 | D5, D4, D3, D2 | Four-bit data interface |

| Remote button label in the sketch | Expected action |
| --- | --- |
| 1 / 2 | TV on / off |
| 3 / 4 | Light on / off |
| 5 / 6 | Fan on / off |
| 8 / 7 | Motor on / off |
| 9 | Set all four output channels low |

The button labels are the prompts in the sketch. The decimal command values come from its `switch` statement and depend on the remote mapping.

## Review or reproduce the simulation

1. Open the [Tinkercad reference screenshot](<Screenshot 2026-02-07 165350.png>) and locate the Uno, IR receiver, and motor path.
2. Read the [Arduino sketch](Home_Automated_Code). The decoder and application code are both included in this text file, and the display uses Arduino's `LiquidCrystal` library.
3. To recreate the demonstration, use a new Arduino Uno circuit in Tinkercad, paste the sketch into the text editor, and use the pin map above for the signals you want to exercise. Use simulator indicators to observe the logical outputs.
4. Start with one channel. Send its on command, observe the pin and LCD, and send its off command. For example, commands 21 and 22 should set the fan channel on D6 high and low.
5. Try the all-off command after turning on multiple channels. Check that D13, D7, D6, and D10 all return low.

## Scope and follow-up work

The repository contains a screenshot and a source snapshot. The editable Tinkercad circuit and physical appliance test results are outside the available artifacts. The sketch uses fixed delays for LCD prompts, so rapid successive input may wait in the command queue. Useful firmware follow-ups are nonblocking display updates and rejecting frames flagged with a decoding error before command dispatch.
