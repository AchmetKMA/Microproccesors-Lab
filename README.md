# Advanced Microprocessors – Embedded Systems Laboratory Projects

A collection of four AVR-based embedded-systems exercises developed for the **Advanced Microprocessors Laboratory** at the Department of Computer Engineering and Informatics, University of Patras (academic years 2022–2024).

The work explores low-level microcontroller programming through GPIO, timers, interrupts, ADC conversion, PWM generation, and event-driven control. Each exercise addresses a different practical system, from traffic-light coordination to environmental monitoring.

## Projects at a Glance

| # | Project | Main concepts |
|---|---|---|
| 1 | Traffic Light Controller | GPIO, TCA0 Split Mode, timer interrupts, pedestrian button, event priority |
| 2 | Obstacle-Aware Moving Device | ADC, Free-Running Mode, Window Comparator, GPIO, timer interrupts, direction control |
| 3 | PWM Fan Controller | PWM, TCA0 Split Mode, period and duty-cycle configuration, button-controlled modes |
| 4 | Smart Greenhouse Controller | ADC thresholds, sensor-based decisions, watering control, PWM ventilation, switch handling |

## 1. Traffic Light Controller

An interrupt-driven traffic-light system coordinates cars, pedestrians, and a tram at a crossing.

### Functionality
- Starts with the vehicle traffic light enabled.
- Uses a timer event (`T1`) to represent the tram's scheduled passage.
- Gives the tram and pedestrian crossing phase priority over vehicle traffic.
- Allows a pedestrian to request a crossing using a push button.
- Uses additional timer intervals (`T2` and `T3`) to control the crossing phase and the period before the pedestrian button can be used again.

### Technical implementation
- Configures GPIO pins for the traffic-light outputs and pedestrian button.
- Uses the **TCA0 timer in Split Mode**.
- Handles timer events through interrupt service routines (ISRs).
- Uses software flags to track whether the pedestrian button is enabled and which timing phase has completed.

## 2. Obstacle-Aware Moving Device

A small moving-device control exercise models navigation through a room using directional indicators and an ADC-based obstacle check.

### Functionality
- Represents forward, left, and right movement using three LEDs.
- Uses an ADC reading to detect an obstacle or boundary condition.
- Changes direction when the measured value crosses a configured limit.
- Supports a reverse/inverse operating mode activated by a switch.
- Uses a counter to identify completion of the navigation sequence.

### Technical implementation
- Configures the ADC for 10-bit conversion and uses input channel `AIN7`.
- Uses **Free-Running ADC Mode** and the ADC Window Comparator.
- Uses the TCA0 timer in Normal Mode to time turning actions.
- Uses a `PORTF` interrupt for the switch input.
- Uses flags such as `turn_left`, `turn_right`, `forward`, and `inverse` to represent the current movement state.

## 3. PWM Fan Controller

This exercise controls two fan-related outputs—described in the report as the blades and the base—and changes their operating behaviour through successive button presses.

### Operating modes

1. **First press:** starts both PWM outputs with their initial period and duty-cycle settings.
2. **Second press:** changes the blade PWM period and duty cycle while keeping the base settings unchanged.
3. **Third press:** disables the timer and stops the operation. The button counter is reset for another cycle.

### Technical implementation
- Uses **TCA0 Split Mode** to configure separate low-byte and high-byte timer channels.
- Configures PWM periods and compare values independently for the two outputs.
- Uses timer underflow interrupts to toggle the corresponding output indicators.
- Uses a `PORTF` switch interrupt to move between operating modes.
- Uses a timer hard reset when switching from Single Mode to Split Mode.

The report specifies a prescaler of `DIV256` for the simulator, noting that `DIV1024` was intended for the real-time configuration but was too slow in the simulator.

## 4. Smart Greenhouse Controller

The final exercise combines ADC-based environmental monitoring with user-selected watering and ventilation operations.

### Functionality
- Continuously monitors an analog sensor input representing humidity.
- Compares ADC readings against lower and upper threshold values.
- Indicates the required action using LEDs:
  - Low reading: suggests watering.
  - High reading: suggests ventilation.
- Uses Switch 5 to activate the watering operation.
- Uses Switch 6 to activate the ventilation operation.
- Warns the user when an inappropriate switch is selected by lighting all three LEDs.
- Returns to the monitoring state after the selected operation completes.

### Technical implementation
- Uses the ADC in **10-bit Free-Running Mode**.
- Uses the ADC Window Comparator to detect readings outside the configured thresholds.
- Uses `AIN7` as the ADC input.
- Uses the TCA0 timer in Normal Mode to time watering.
- Uses Single-Slope PWM for ventilation.
- Uses a timer overflow counter to stop ventilation after four rising-edge events.
- Uses `PORTF` interrupts to detect Switch 5 and Switch 6.

### Values documented in the report

| Parameter | Value |
|---|---:|
| Lower threshold | 20 |
| Upper threshold | 40 |
| ADC resolution | 10-bit |
| ADC input | AIN7 |
| Watering switch | PORTF pin 5 |
| Ventilation switch | PORTF pin 6 |
| Ventilation duration | 4 rising-edge events |

The report also includes illustrative comparisons using values such as `10` and `20` in parts of the explanatory code. The exact threshold configuration should therefore be checked against the final source code used for a build.

## Technologies and Concepts

- **Language:** C
- **Platform:** AVR microcontroller programming
- **Peripherals:** GPIO, ADC, TCA0 timer/counter
- **Timing and control:** Timer interrupts, compare-match events, overflow events
- **Signal generation:** PWM, period configuration, duty-cycle configuration
- **Event handling:** External pin interrupts, interrupt flags, bit masking
- **ADC modes:** 10-bit conversion, Free-Running Mode, Window Comparator

## Repository Contents

The accompanying technical report documents:
- Flowcharts describing system behaviour.
- Source-code listings for the four laboratory exercises.
- Explanations of the main functions and interrupt service routines.
- Answers to the individual exercise questions.
- Configuration details and selected implementation decisions.

A suggested repository structure is:

```text
.
├── README.md
├── traffic-lights/
│   └── main.c
├── obstacle-navigation/
│   └── main.c
├── pwm-fan-controller/
│   └── main.c
└── smart-greenhouse/
    └── main.c
```

The directory structure above is a suggested way to organize the source files; it does not imply that the uploaded report already contains these folders.

## Building and Running

The report provides AVR C source code and register-level configuration, but it does not specify a complete, reproducible build setup or a single target-board configuration for all exercises.

To build and run the programs:

1. Identify the exact AVR microcontroller and development board used by your project.
2. Open the corresponding source file in an AVR-compatible toolchain or IDE.
3. Configure the device, clock, and programmer/debugger settings for the target hardware.
4. Build and flash the selected exercise.
5. Connect the LEDs, switches, and analog input according to the pin configuration in the source code.
6. Test the interrupt and timer behaviour on the target board or in a compatible simulator.

**Important:** The traffic-light listing contains `asm("break");` as a debugging aid. The report notes that this instruction should be removed or commented out in the final non-debug execution.

## Learning Outcomes

Through these exercises, the project demonstrates practical work with:
- Register-level embedded C programming.
- Configuring and controlling microcontroller peripherals.
- Designing interrupt-driven state transitions.
- Measuring analog inputs and reacting to threshold conditions.
- Generating PWM signals with configurable timing.
- Combining multiple peripherals into a small control system.
- Debugging and documenting embedded software behaviour.

## Academic Context

**Course:** Advanced Microprocessors Laboratory  
**Department:** Computer Engineering and Informatics  
**Institution:** University of Patras  
**Academic years covered:** 2022–2023 and 2023–2024  
**Laboratory group:** B4

---

*This repository is an academic laboratory project intended to demonstrate microcontroller programming and peripheral control. Hardware-specific behaviour and timing should be verified on the actual target board.*
