# Cadence Virtuoso Design & Simulation Project

## 🎯 Project Overview

This repository demonstrates a **complete analog circuit design and simulation workflow** using **Cadence Virtuoso**—industry-standard CAD tool for IC design. From schematic capture to detailed circuit analysis, this project showcases modern design methodology in semiconductor engineering.

**Perfect for**: IC design engineers, analog design learners, and anyone interested in circuit simulation workflows.

---

## 📊 What This Project Covers

✅ **Schematic Design** — Component placement & circuit topology using Virtuoso Schematic Editor  
✅ **DC Analysis** — Operating point analysis & steady-state behavior  
✅ **Transient Analysis** — Time-domain response & signal behavior  
✅ **Simulation & Verification** — Complete netlist generation & waveform analysis  
✅ **Design Documentation** — Detailed technical notes & specifications  

---



## 🚀 Getting Started

### Requirements
- **Cadence Virtuoso** (2021.09+) with Spectre or SPICE simulator
- **Process Design Kit (PDK)**(GPDK 45nm) — TSMC, Samsung, or your foundry's PDK
- Moderate familiarity with circuit design concepts

### Quick Setup
 $csh
 $cd Documents
 
# Launch Virtuoso
$virtuoso

##  Schematic Design

The schematic captures your circuit topology using Virtuoso's graphical editor:

1. **Place Components** — Instantiate transistors, resistors, capacitors from PDK library
2. **Define Connections** — Create nets and name signals (VDD, GND, input, output)
3. **Set Parameters** — Configure W/L ratios for transistors, component values
4. **Add Pins** — Export circuit inputs/outputs as cell pins
5. **Document** — Add design notes with specifications and operating conditions

💡 **Best Practice**: Use meaningful net names and organize hierarchically for complex circuits

---

## ⚡ DC Analysis — Steady-State Behavior

**DC Analysis** analyzes your circuit at steady-state, where capacitors block current and inductors short. This reveals:
- **Operating Point (Q-point)** — Bias voltages and currents at each node
- **Device Saturation** — Whether transistors are in active or saturation region
- **DC Gain** — Voltage amplification in linear region
- **Power Consumption** — Quiescent and bias currents

### Example DC Analysis Testbench
```spice
.include "technology.cir"
.include "models.cir"

// Circuit
M1 (out in VDD VDD) nch W=10u L=1u
M2 (out in GND GND) pch W=20u L=1u
R1 (VDD out) resistor R=10k

// Supply
VDD VDD 0 DC 5V
Vin in 0 DC 2.5V

// DC sweep: vary input 0-5V
.dc Vin 0 5 0.1

// Measure results
.print dc v(out) v(in) i(VDD)
```

### What to Look For
✓ All node voltages match design expectations  
✓ Power supply current is reasonable  
✓ Transistors operate in correct region (saturation for amplifiers)  
✓ No excessive current spikes or floating nodes

---

## 📈 Transient Analysis — Time-Domain Response

**Transient Analysis** simulates how your circuit responds to time-varying signals over time. It captures:
- **Propagation Delay** — Time from input change to output response
- **Rise/Fall Times** — Speed of signal transitions
- **Overshoot & Ringing** — Signal overshoots and oscillations
- **Settling Behavior** — Time to reach steady-state
- **Current Transients** — Dynamic power consumption

// Circuit
M1 (out in VDD VDD) nch W=10u L=1u
M2 (out in GND GND) pch W=20u L=1u
C1 (out GND) capacitor C=1p

// DC Supply
VDD VDD 0 DC 5V

// Input pulse: 0→5V, 1ns delay, 100ps rise/fall, 10ns period
Vin in 0 PULSE (0 5 1n 100p 100p 5n 10n)

// Simulate for 20 nanoseconds with 1ps steps
.tran 1p 20n

.save v(out) v(in) i(VDD)
```

### Key Measurements
📊 **Timing Metrics**
- Rise time (10%-90% transition)
- Propagation delay (input to output)
- Settling time (reaching final value)

📊 **Signal Quality**
- Overshoot: Peak exceeds expected final value
- Undershoot: Valley below expected final value  
- Ringing: Oscillations around final value

### Example Results
```
Propagation Delay: 1.1 ns
Rise Time: 150 ps
Overshoot: 10%
Settling Time: 3 ns
Power Spike: 8.2 mA @ transition
```

---

## 📊 Viewing & Analyzing Results

### Launch WaveView
```bash
virtuoso -nWaveView directoryName/psf &
# Or from Virtuoso: Simulation → WaveView
```

### Quick Analysis Checklist
✅ Add signals to plot (drag from hierarchy)  
✅ Use cursors to measure exact values  
✅ Mark reference points for timing analysis  
✅ Compare against theoretical predictions  
✅ Export waveforms as images or CSV  

### What to Look For

**DC Analysis Plots** — Shows input-output transfer curve
- Linear region reveals DC gain
- Saturation regions show limits
- Verify expected operating point

**Transient Plots** — Shows time-domain behavior
- Check propagation delay meets specs
- Verify rise/fall times acceptable
- Detect unwanted ringing or overshoot
- Confirm settling to final value

---

## 🎬 Quick Workflow

1. **Setup** — `virtuoso &`
2. **Create Schematic** — Library Manager → New Cell View
3. **Place Components** — Create → Instance, add nets and pins
4. **Run Simulation** — Simulation → Netlist → Run
5. **View Results** — WaveView, add signals, measure results
6. **Export** — Save waveforms as CSV or PDF

### Useful Commands
```bash
virtuoso &                    # Launch Virtuoso GUI
virtuoso -nographics -b script.il  # Batch mode simulation
spectre circuit.scs           # Direct SPICE simulation
waveview psf_directory        # View results in WaveView
```

---

## ⚠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| **Simulation won't start** | Check all components connected, verify power supplies defined, no floating nets |
| **DC won't converge** | Reduce step size, increase max iterations (`.option maxiter=1000`) |
| **Transient too slow** | Increase time step, reduce simulation duration, simplify circuit |
| **PDK not found** | Verify PDK path in `cds.lib`, check `PDKPATH` environment variable |
| **Inaccurate results** | Reduce tolerance, verify model parameters, check for parasitics |

---

## 🔄 Design Iteration Loop

1. **Run DC & Transient Analysis** → Check if specs met
2. **Identify Issues** → Overshoot? Slow? High power?
3. **Adjust Parameters** → Change W/L, resistors, capacitors
4. **Re-simulate** → Compare results
5. **Repeat** → Until specifications satisfied

---

## 📚 Resources

- **Cadence Documentation**: Virtuoso Schematic Composer, Spectre User Guide
- **Foundry PDK**: TSMC, Samsung, or your technology provider
- **References**: 
  - "Electronic Circuit Analysis and Design" — Neamen
  - "SPICE for Power Electronics" — Rashid
  - [Cadence.com](https://www.cadence.com)

---


## License

This project is licensed under my Institution Rajalakshmi Engineering college

---

## Contact & Support

For questions:
- Contact: rkhemant2005@gmail.com


---

Last Updated:  July 2026  
Maintained By: HEMANT R K   
Status: Active Learning