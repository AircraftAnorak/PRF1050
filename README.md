# PRF1050

## Introduction
The **PRF10**, produced by Simoco/Philips, is a discontinued radio repeater that can operate at VHF or UHF frequencies depending on the model.

https://www.radiomuseum.org/r/philips_radio_repeater_prf10_prf1050.html

<img src="https://github.com/user-attachments/assets/4aa4dcfe-7574-46e8-ac96-3b84d2e3d602" width="50%">

This GitHub repository documents the restoration process for **PRF1050** repeater that was once used for emergency communications to amateur radio frequencies.

I would like to state that in no way am I an expert on this repeater system, but a hobbyist who has gone through the system which was once non-operational to operational again, listing all my troubles encountered along the way.

***

##### Table of Contents  
[Internals](#internals)  
[Power Issue](#12v-power-issue)  
[Programming Channels](#1programming-channels)  
<a name="headers"/>

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

## Control Board and Programming

Since this repeater once operated outside of the amateur band, it would need to be reprogrammed to suitable frequencies for testing, later to allocated frequencies for operation. For this, I found almost no information online apart from many people saying you need a programmer unit made for this repeater (something I do not have), so felt a bit lost and stuck on how to progress.

I am not to sure how I figured this out but I believe on one forum someone mentioned you can program it by flashing to a chip which holds all the channel data. This is something which I could attempt with my experience in ICs and after a bit of scavenging on the board, I came across the 24C16 flash chip which holds this data.

<img src="https://github.com/user-attachments/assets/7820ffe2-3cb3-4a2f-a779-2fd028cfe08a" width="50%">

*This chip is located on the **Control** board*

<img src="https://github.com/user-attachments/assets/0e29a5c6-e7ae-4b4b-838d-ee3ddae2c18d" width="30%">

Thankfully it uses I2C which is very familiar to me so in my excitement, I straight away ended up soldering some jumpers on the SDA and SCL pins to see if I could dump the chip 

_(**NOTE**: Do not do this as it will not work... I will explain why)_

<img src="https://github.com/user-attachments/assets/fb952add-f123-4214-8970-688a4d270800" width="50%">

*Connecting the repeater with jumper wires to an IC programmer*

What I forgot to remember is that I2C lines are usually paired with other ICs (Duh!!) so when trying to read the chip, I was in fact trying to read multiple so my HEX dump was returning nothing,

To get around this I ended up desoldering the whole chip and then soldering jumpers to isolate it 



24C16 Desoldered             |  Jumpers attached to VCC, GND, SDA and SCL
:-------------------------:|:-------------------------:
![20240423_115343-min](https://github.com/user-attachments/assets/ae7da1a6-4a5f-48f7-86cf-d66221edd0bc)  |  ![20240423_114845-min](https://github.com/user-attachments/assets/f8276294-2eac-42b6-ac5d-5478463b0f14)

_(**NOTE**: This is also a bad idea, again I am a radio nerd that was too excited to dump the memory - during the process of this I accidentally snapped off one of the legs which was thankfully able to be fixed - in hindsight it would be best to have solder points directly on the board which is what I did eventually)_

<img src="https://github.com/user-attachments/assets/219d619a-0e1e-405c-b6c1-36706f01ee1c" width="50%">

You can see here cut traces and soldered jumpers to be able to program the chip while seated in the board.

So this worked! When running it through the IC programmer, reading the chip gave a promising HEX dump that was consistent every time I read it instead of noise like before.

<img src="https://github.com/user-attachments/assets/a141a8e2-257e-410a-b538-ac185fc951e5" width="50%">

### Programming Channels

This HEX dump can then be run through an amazing program called [QFM1000](https://github.com/sardylan/qfm1000), which made programming the repeater a straightforward job without having to inspect the .h file manually.

<img src="https://github.com/user-attachments/assets/1692603f-d49c-4411-866f-77ccae5e0fb0" width="50%">

Here you can see the previous frequencies the repeater operated on - these can be changed to our values which we desire
