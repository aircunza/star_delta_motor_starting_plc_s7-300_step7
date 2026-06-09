# Star-Delta Motor Starter (STEP 7 Classic)

## Features

- Start pushbutton command
- Emergency Stop (E-Stop)
- Motor Protection Circuit Breaker (MPCB) monitoring
- Main contactor control
- Automatic Star → Delta transition
- Configurable transition timer
- Electrical interlocking between Star and Delta contactors
- Run indicator lamp
- Encapsulated logic inside a reusable Function Block (FB)

---

## Hardware Mapping

### Inputs

| Address | Name             | Description                                    |
| ------- | ---------------- | ---------------------------------------------- |
| I0.0    | START_PB         | Start pushbutton or motor start command        |
| I0.1    | MOTOR_PROTECTION | Motor Protection Circuit Breaker (MPCB) status |
| I0.2    | E_STOP           | Emergency Stop                                 |

### Outputs

| Address | Name            | Description             |
| ------- | --------------- | ----------------------- |
| Q4.0    | MAIN_CONTACTOR  | Main motor contactor    |
| Q4.1    | STAR_CONTACTOR  | Star contactor          |
| Q4.2    | DELTA_CONTACTOR | Delta contactor         |
| Q4.3    | RUN_LAMP        | Motor running indicator |

### Timer

| Resource | Name | Description                                 |
| -------- | ---- | ------------------------------------------- |
| T0       | KT   | Star running time before switching to Delta |

---

# Sequence of Operation

1. The operator presses the **START** pushbutton.
2. If the **E-Stop** is released and the **MPCB** is healthy, the motor starts.
3. The **Main Contactor** energizes.
4. The **Star Contactor** energizes.
5. The transition timer starts.
6. When the timer expires:
   - The **Star Contactor** is de-energized.
   - The **Delta Contactor** is energized.
7. The motor continues running in **Delta mode** until stopped or a protection signal is activated.

---

## Interlocking

The program prevents the **Star** and **Delta** contactors from being energized simultaneously.

This is implemented using software interlocking in the ladder logic, following standard motor starter practices.

---

# Function Block

The entire control sequence is implemented inside the reusable **Motor_starting** Function Block.

### Ladder Logic

![image alt](https://github.com/aircunza/star_delta_motor_starting_plc_s7-300_step7/blob/d3045ad9eac53d0aba7eaaa5c9e1a56efce02438/docs/media_assets/img09062026_01.png)

---

# OB1 Integration

The Function Block is called from **OB1**, keeping the cyclic program simple and allowing the motor starter to be reused in larger projects.

![image alt](https://github.com/aircunza/star_delta_motor_starting_plc_s7-300_step7/blob/d3045ad9eac53d0aba7eaaa5c9e1a56efce02438/docs/media_assets/img09062026_02.png)

---

# Simulation

The following animation shows the complete operating sequence.

- Start command
- Star operation
- Timer countdown
- Automatic transition to Delta

![image alt](https://github.com/aircunza/star_delta_motor_starting_plc_s7-300_step7/blob/b06d322b27c4ae6fed166a409ad6c626b304ef1f/docs/media_assets/simulation2.gif)

---

# Project Structure

```
STEP7_Project/
│
├── OB1
├── FB1   (Motor_starting)
├── DB1   (Motor_starting_db)
└── README.md
```

---

# Development Environment

- Siemens STEP 7 Classic
- Ladder Logic (LAD)
- Windows 10
- SIMATIC Manager
- S7-300 PLC

---

# Characteristics

- Modular PLC programming using Function Blocks
- Motor starter sequencing
- Timer-based state transitions
- Safe interlocking logic
- Industrial ladder programming practices
- Clear I/O mapping and documentation
