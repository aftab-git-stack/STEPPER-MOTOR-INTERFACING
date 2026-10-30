# STEPPER MOTOR INTERFACING

## AIM
To write an assembly language program in 8086 to rotate the motor at different speeds.

---

## APPARATUS REQUIRED

| S. No | Item                        | Specification   | Quantity |
|-------|-----------------------------|-----------------|----------|
| 1     | Microprocessor kit          | 8086            | 1        |
| 2     | Power Supply                | +5 V DC, +12 V DC | 1      |
| 3     | Stepper Motor Interface board | -             | 1        |
| 4     | Stepper Motor               | -               | 1        |

---

## THEORY
A motor in which the rotor is able to assume only discrete stationary angular positions is a **stepper motor**. The rotary motion occurs in a stepwise manner from one equilibrium position to the next.  

**Two-phase scheme:** Any two adjacent stator windings are energized. There are two magnetic fields active in quadrature and none of the rotor pole faces can be in direct alignment with the stator poles. A partial but symmetric alignment of the rotor poles is of course possible.

---

## ALGORITHM
For running the stepper motor in clockwise and anticlockwise directions:

1. Get the first data from the lookup table.  
2. Initialize the counter and move data into the accumulator.  
3. Drive the stepper motor circuitry and introduce delay.  
4. Decrement the counter. If not zero, repeat from step (iii).  
5. Repeat the above procedure both for backward and forward directions.  

---

## SWITCHING SEQUENCE OF STEPPER MOTOR

| Memory Location | A1 | A2 | B1 | B2 | Hex Code |
|-----------------|----|----|----|----|----------|
| 1200            | 1  | 0  | 0  | 0  | 09H      |
| 1201            | 0  | 1  | 0  | 1  | 05H      |
| 1202            | 0  | 1  | 1  | 0  | 06H      |
| 1203            | 1  | 0  | 1  | 0  | 0AH      |

---

## PROGRAM
```asm
; Stepper Motor Interfacing Program in 8086 Assembly

.MODEL SMALL
.STACK 100H

.DATA
    DATA1 DB 09H, 05H, 06H, 0AH

.CODE
START:
    MOV AX, @DATA
    MOV DS, AX

    MOV DX, 00C0H        ; Output port address

; CLOCKWISE ROTATION
CLOCKWISE:
    LEA SI, DATA1
    MOV CX, 0004H

UP:
    MOV AL, [SI]
    OUT DX, AL

    CALL DELAY

    INC SI
    LOOP UP

; ANTICLOCKWISE ROTATION
ANTICLOCKWISE:
    LEA SI, DATA1
    ADD SI, 0003H
    MOV CX, 0004H

DOWN:
    MOV AL, [SI]
    OUT DX, AL

    CALL DELAY

    DEC SI
    LOOP DOWN

    JMP CLOCKWISE

; DELAY SUBROUTINE
DELAY PROC
    MOV BX, 1010H

L1:
    DEC BX
    JNZ L1
    RET
DELAY ENDP

END START

```

## OUTPUT OF THE PROGRAM:
clockwise:
```
09H → 05H → 06H → 0AH
```
anti-clockwise
```
0AH → 06H → 05H → 09H
```

## RESULT

Thus, the assembly language program for rotating the stepper motor in both clockwise and anticlockwise directions was written and verified.
