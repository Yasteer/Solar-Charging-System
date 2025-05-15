# 🪫 Analog Lead-Acid Battery Charger Simulation (LTspice)

This project presents a fully **analog-based lead-acid battery charging system**, simulated in **LTspice**. It demonstrates core concepts in analog circuit design, waveform generation, comparator logic, and power switching — all built using discrete and op-amp components without microcontrollers or digital control.

The simulation highlights the **charging profile control** of a lead-acid battery using a three-stage algorithm: **Bulk**, **Absorption**, and **Float**. These stages are detected and managed purely through analog circuitry.

---

## 🧩 Project Structure

The design is divided into the following functional subsystems:

### 1. 🔋 Battery Charge Stage Identification
**Purpose**: Determine which charging stage the battery is currently in based on its voltage.  
**Circuits Used**:
- **Dual Window Comparator** (built with LT1011)
- **Summing Amplifier** (used for voltage level manipulation and reference scaling)

This subsystem classifies the battery into one of three states:
- **Bulk** (Low voltage, high current)
- **Absorption** (Medium voltage, tapering current)
- **Float** (Maintaining full charge)

---

### 2. 📐 100kHz Triangular Waveform Generator
**Purpose**: Provide a continuous triangular waveform at 100kHz to be used for PWM comparison.  
**Circuits Used**:
- **555 Timer** in astable mode (generates square wave clock)
- **Integrator Op-Amp** (converts square to triangle)
- **Summing Amplifier** (adjusts amplitude/offset as needed)

---

### 3. 🧠 PWM Generator (Analog Modulator)
**Purpose**: Create a PWM signal whose **duty cycle reflects the battery charging stage**, while maintaining a **fixed frequency of 100kHz**.  
**Circuits Used**:
- **LT1011 Comparator**: Compares the triangular wave (from subsystem 2) against the voltage levels generated in subsystem 1 to generate PWM.

---

### 4. ⚡ Gate Driver & MOSFET Switch
**Purpose**: Drive an **IRLZ44N N-channel MOSFET** at 100kHz to control current into the battery based on PWM duty cycle.  
**Circuits Used**:
- LT1364-buffered PWM signal
- Gate driver network
- IRLZ44N power switch

---

## Simulation Results
The following PWM captures display the circuits ability to automatically adapt the gate charging signal to the battery charging system.
Note that at lower battery voltages, the system produces a larger PWM duty cycle and at higher voltages, it scales down the duty cycle.

* Bulk Mode PWM
> ![Bulk Mode PWM](Simulation_Results/BulkModeChargingReference.PNG)

* Absorption Mode PWM
> ![Absorption Mode PWM](Simulation_Results/AbsorptionModeChargingReference.PNG)

* Float Mode PWM
> ![Float Mode PWM](Simulation_Results/FloatModeChargingReference.PNG)
---

## 🧠 Design Notes

- All active buffering is handled by the **LT1364 dual op-amp**, chosen specifically for its high slew rate and bandwidth — critical for 100kHz operation.
- The **LM741** was intentionally avoided due to its insufficient performance at high frequencies.
- The entire system operates without any digital logic or microcontroller — a pure analog control strategy.

---

## 🛠️ Tools Used

- **LTspice XVII**
- Discrete analog components
- Industry-standard op-amps and comparators

---

## 📁 Repository Contents

- `/schematics/`: Contains `.asc` LTspice schematic files for each subsystem
- `/plots/`: Simulation waveforms showcasing each subsystem in operation
- `README.md`: This documentation
- Additional notes on design rationale, tuning, and improvements

---

## 📌 Future Enhancements

- Integrate temperature compensation for float voltage
- Add current-sensing feedback loop for constant-current regulation
- PCB design and hardware prototype

---

## 👤 Author

**Yasteer Sewpersad**  
Electrical, Control & Instrumentation Engineer  
South Africa 🇿🇦

---

## 📜 License

This project is open-source under the MIT License. See `LICENSE` for details.
