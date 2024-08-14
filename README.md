# PRF1050

## Introduction
The **PRF10**, produced by Simoco/Philips, is a discontinued radio repeater that can operate at VHF or UHF frequencies depending on the model.

<img src="https://github.com/user-attachments/assets/4aa4dcfe-7574-46e8-ac96-3b84d2e3d602" width="50%">

This GitHub repository documents the restoration process for **PRF1050** repeater that was once used for emergency communications to amateur radio frequencies.

I would like to state that in no way am I an expert on this repeater system, but a hobbyist who has gone through the system which was once non-operational to operational again, listing all my troubles encountered along the way.

***

## Internals

<img src="https://github.com/user-attachments/assets/448bc7d6-9afe-4a40-8477-10b34b66ff43" width="50%">

*Internals of the PRF10 when powered on*

<br />

<img src="https://github.com/user-attachments/assets/d7516fdb-bc83-4ba6-b16e-4477a53067db" width="50%">

*PRF1050 block diagram*

### The repeater consists of 4 main blocks that provide different functions:

- **CIU**: Microprocessor converting parallel wire control input to external Interface Unit message bus signals, Tx key processing, audio gating and routeing
- **Analouge**: Consisting of the Tx synthesizer (reference oscillator, comparator and dividers, pre-scaler, synthesizer IC and loop filter), Transmitter (Audio amplifiers, limiter
  - Tx synthesizer (reference oscillator, comparator and dividers, pre-scaler, synthesizer IC and loop filter)
  - Transmitter/Reciever (Audio amplifiers, limiter, filters, gating and power control circuits. 1st and 2nd IFs, 2nd oscillator and mixer, IF demodulation circuits including squelch gate, AF amplifiers, filters and gating
- **Control**: Main transciever microprocessor with clock oscillator, EPROM, E2PROM, RAM, shift registers, timers, 30V generation and optional CTCSS encode circuits
- **Rx Synthesizer**: Reciever synthesizer IC, pre-scaler, Rx reference oscillator and optional CTCSS decode circuits

### 12V Power issue

For my model, all blocks were working apart from the **CIU** which was not getting the required 12V to power the board, which is most likely not having the DC connector on the rear of the unit. It was discovered however that this was not required and 12V can be easily tapped into from the motherboard itself, with a jumper from this source soldered onto a regulator in the CIU.

<img src="https://github.com/user-attachments/assets/8b15f2d6-cc94-4c7c-a95a-2067f64d38cb" width="50%">
