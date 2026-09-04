# AVR Assembly Buzzer Melody

A simple AVR Assembly project for the **ATmega328P / Arduino Uno** that plays musical notes using a passive buzzer.

The program generates different frequencies by switching an output pin HIGH and LOW with software delay loops. Each musical note is implemented as its own subroutine, which makes it easy to create melodies by calling the note names from the main program.

## Features

* Written entirely in AVR Assembly
* Designed for the ATmega328P
* Works with Arduino Uno
* Generates musical notes using a passive buzzer
* Uses nested loops to create software delays
* Each note is implemented as a separate subroutine
* Melodies can be created by calling note functions

## Supported Notes

The program currently supports the seven main notes of the **4th octave**:

| Note | Frequency |
| ---- | --------- |
| C4   | 261.63 Hz |
| D4   | 293.66 Hz |
| E4   | 329.63 Hz |
| F4   | 349.23 Hz |
| G4   | 392.00 Hz |
| A4   | 440.00 Hz |
| B4   | 493.88 Hz |

Each note uses a different software delay to generate an approximate square-wave frequency for the passive buzzer.

> **Note:** Since the frequencies are generated using software delay loops instead of hardware timers, the actual output frequency may be slightly different from the standard musical frequency.

## Example Melody

The program currently plays the beginning of **Twinkle Twinkle Little Star**:

```text
C C G G A A G
F F E E D D C
```

In Assembly, the melody can be written by simply calling each note:

```asm
repeat:
    rcall NOTE_C
    rcall NOTE_C
    rcall NOTE_G
    rcall NOTE_G
    rcall NOTE_A
    rcall NOTE_A
    rcall NOTE_G

    rcall NOTE_F
    rcall NOTE_F
    rcall NOTE_E
    rcall NOTE_E
    rcall NOTE_D
    rcall NOTE_D
    rcall NOTE_C

    rjmp repeat
```

## How It Works

A passive buzzer produces sound when the voltage applied to it changes repeatedly.

For every note, the program:

1. Sets the output pin HIGH.
2. Waits for a short delay.
3. Sets the output pin LOW.
4. Waits for the same delay.
5. Repeats the process multiple times.

This creates a square wave.

The frequency of the square wave determines the pitch of the sound:

```text
Higher frequency -> Higher pitch
Lower frequency  -> Lower pitch
```

Different delay values are used for C, D, E, F, G, A, and B.

## Note Subroutines

Each note is implemented as its own subroutine.

For example:

```asm
NOTE_C:
    ldi r20, 130

NOTE_C_LOOP:
    ldi r16, $10
    out PORTB, r16
    rcall NOTE_C_DELAY

    ldi r16, $00
    out PORTB, r16
    rcall NOTE_C_DELAY

    dec r20
    brne NOTE_C_LOOP

    ret
```

The `r20` register controls how many HIGH/LOW cycles are generated, which affects how long the note is played.

## Software Delay

Each note uses a three-level nested loop to create its delay.

Example for C:

```asm
NOTE_C_DELAY:
    ldi r17, 1

NOTE_C_1:
    ldi r18, 40

NOTE_C_2:
    ldi r19, 250

NOTE_C_3:
    dec r19
    brne NOTE_C_3

    dec r18
    brne NOTE_C_2

    dec r17
    brne NOTE_C_1

    ret
```

The registers are used as:

```text
r17 -> outer loop
r18 -> middle loop
r19 -> inner loop
```

Changing these counter values changes the delay and therefore changes the frequency of the buzzer.

## Hardware

* Arduino Uno
* ATmega328P microcontroller
* Passive buzzer
* Jumper wires

## Pin Configuration

The buzzer is connected to an output pin on PORTB.

For Arduino Uno digital pin 12:

```text
Arduino D12 = ATmega328P PB4
```

PB4 can be selected using:

```asm
ldi r16, $10
```

The Data Direction Register is configured so the pin works as an output:

```asm
ldi r16, $10
out DDRB, r16
```

## Project Structure

```text
avr-assembly-buzzer-melody/
│
├── main.asm
└── README.md
```

## What I Learned

This project was created to practice and understand:

* AVR Assembly programming
* ATmega328P registers
* Digital output
* Nested loops
* Software delays
* Subroutines
* `rcall`
* `ret`
* Square-wave generation
* Frequency
* Musical notes
* Basic buzzer control

## Possible Improvements

Future improvements could include:

* Adding sharps and flats
* Adding more octaves
* Adding pauses between notes
* Adding different note durations
* Creating more melodies
* Using a single reusable note-playing routine
* Using hardware timers instead of software delay loops
* Improving frequency accuracy

## Repository Name

`avr-assembly-buzzer-melody`

## Author

Amirmohammad Masoumi
