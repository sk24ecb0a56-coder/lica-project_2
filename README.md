# Analog Audio Spectrum Analyzer

**A fully analog 4-band LED spectrum analyzer using only ICs - NO microcontrollers, NO programming, NO Arduino!**

![Status](https://img.shields.io/badge/status-design%20complete-success)
![Type](https://img.shields.io/badge/type-pure%20analog-blue)
![ICs](https://img.shields.io/badge/ICs-10%20active-orange)
![LEDs](https://img.shields.io/badge/LEDs-24%20total-brightgreen)

---

## 📋 Project Overview

This is a complete circuit design for an audio spectrum analyzer that:
- Splits audio into **4 frequency bands** (Bass, Mid-Low, Mid-High, Treble)
- Displays level on **20 bar graph LEDs** (5 per band)
- Shows peaks on **4 blue peak-hold LEDs**
- Uses **only analog ICs** - no digital processing whatsoever!
- Optionally passes audio to speaker for live monitoring

### Specifications

| Feature | Specification |
|---------|---------------|
| **Frequency Bands** | 4 (Bass 20-300Hz, Mid-Low 300Hz-1.5kHz, Mid-High 1.5-6kHz, Treble 6-20kHz) |
| **LEDs per Band** | 5 (bar graph) + 1 (peak hold) |
| **Total LEDs** | 24 (20 bar + 4 peak) |
| **Display Mode** | BAR or DOT (switchable) |
| **Response Time** | Attack: ~1ms, Decay: ~100ms |
| **Peak Hold Time** | ~5 seconds (adjustable) |
| **Power Supply** | 230V AC → +12V & +5V DC |
| **Total Power** | ~5-10W |
| **Audio Pass-through** | Yes (optional speaker output) |

---

## 🎯 What Makes This Project Special

### ✅ Pure Analog Design
- **No Arduino, ESP32, or any microcontroller**
- **No C/C++/Python code to write**
- **No OLED displays or digital processing**
- Everything done by analog ICs doing what they're designed for!

### ✅ Educational Value
Demonstrates real-world analog circuits:
- Sallen-Key bandpass filters
- Precision rectifiers
- Envelope detection
- Peak hold circuits
- Logarithmic LED drivers
- Voltage regulation

### ✅ Affordable Components
- All ICs available in India (SP Road, Lamington Road, online)
- Total cost: ₹500-800 for breadboard version, ₹1,000-1,200 for PCB version
- Each IC costs under ₹60

### ✅ Modified Design Using 2× LM358 Instead of 1× LM324
**Yes, it works!** The original design calls for 1× LM324 (quad op-amp) for the filter bank. This implementation uses **2× LM358 (dual op-amp)** instead, which is:
- More readily available in most Indian electronics shops
- Cheaper (₹10-15 each vs ₹15-20 for LM324)
- Electrically identical for this application
- Only requires one additional IC socket

---

## 📂 Documentation

This repository contains complete documentation for building the project:

### [1. CIRCUIT_DESIGN.md](CIRCUIT_DESIGN.md)
**Complete circuit diagrams for all stages:**
- Stage 1: Input Buffer (LM741/TL071)
- Stage 2: 4-Band Filter Bank (2× LM358 replacing LM324)
- Stage 3: Envelope Detector (2× LM358)
- Stage 4: Peak Hold Circuit (LM339)
- Stage 5: LED Bar Display (4× LM3915)
- Stage 6: Audio Pass-Through (LM386)
- Power Supply (Transformer, LM7812, LM7805)

**Includes:**
- Detailed ASCII circuit diagrams
- Pin connections for every IC
- Component values and calculations
- Analysis of LM358 vs LM324 substitution
- Testing procedures for each stage

### [2. COMPONENT_CALCULATIONS.md](COMPONENT_CALCULATIONS.md)
**Design formulas and calculations:**
- Filter frequency calculations (Sallen-Key bandpass)
- Time constant calculations (envelope detector, peak hold)
- LED current calculations
- Power supply requirements and dissipation
- Component tolerance analysis
- Alternative component substitutions

### [3. BOM_SHOPPING_LIST.md](BOM_SHOPPING_LIST.md)
**Complete bill of materials:**
- Detailed shopping list with part numbers
- Prices in Indian Rupees (₹)
- Where to buy in India (online and physical stores)
- IC sockets, resistors, capacitors, LEDs, etc.
- Power supply components
- Hardware and enclosure options
- Estimated total costs for different build options

### [4. ASSEMBLY_GUIDE.md](ASSEMBLY_GUIDE.md)
**Step-by-step build instructions:**
- Safety warnings and precautions
- Stage-by-stage assembly (build one stage at a time)
- Testing procedures for each stage
- Troubleshooting guide
- Calibration and tuning
- Final enclosure assembly
- Success criteria and test tracks

---

## 🔧 What You Need

### Active Components (ICs)
- 1× LM741 or TL071 (input buffer)
- 4× LM358 (filters + envelope detectors)
- 1× LM339 (peak hold comparators)
- 4× LM3915 (LED drivers) ← **Most important IC**
- 1× LM386 (audio amplifier, optional)
- 1× LM7812 (+12V regulator)
- 1× LM7805 (+5V regulator)

**Total: 13 ICs** (10 active + 3 power)

### Passive Components
- **Resistors:** ~65 total (various values)
- **Capacitors:** ~31 total (electrolytic and ceramic)
- **Diodes:** 12 total (8× 1N4148 + 4× 1N4007)
- **LEDs:** 24 total (5 green, 5 yellow, 5 orange, 5 red, 4 blue)

### Power Supply
- 230V to 12-0-12V transformer
- Bridge rectifier
- Filter capacitors
- Voltage regulators with heatsinks

### Hardware
- PCB or breadboard
- Enclosure (project box)
- 3.5mm audio jack
- Switches (power, mode)
- Potentiometers (optional, for gain/brightness)
- Speaker (8Ω, 0.5W, optional)

**See [BOM_SHOPPING_LIST.md](BOM_SHOPPING_LIST.md) for complete details**

---

## 💰 Cost Estimate

| Build Type | Components | Hardware | Total |
|------------|------------|----------|-------|
| **Basic Breadboard** | ₹600 | ₹150 | **₹750-850** |
| **PCB Version** | ₹600 | ₹250 | **₹850-1,000** |
| **Professional** | ₹600 | ₹500+ | **₹1,100-1,500** |

*Prices in Indian Rupees (₹), excluding tools*

---

## 🎓 How It Works

### Signal Flow (6 Stages)

```
Audio Input (3.5mm jack)
    ↓
[Stage 1] Input Buffer (LM741)
    ├─→ [Speaker Output (LM386)]
    ↓
[Stage 2] 4-Band Filter Bank (2× LM358)
    ├─→ Band 1: Bass (100 Hz)
    ├─→ Band 2: Mid-Low (700 Hz)
    ├─→ Band 3: Mid-High (3 kHz)
    └─→ Band 4: Treble (10 kHz)
    ↓
[Stage 3] Envelope Detectors (2× LM358)
    ├─→ AC → DC conversion
    └─→ ~100ms decay time
    ↓
[Stage 4] Peak Hold (LM339)
    ├─→ Blue LED peak indicators
    └─→ ~5 second hold time
    ↓
[Stage 5] LED Drivers (4× LM3915)
    └─→ 5 LEDs per band (logarithmic scale)
```

### Key Technologies

1. **Sallen-Key Bandpass Filters**
   - 2nd order active filters
   - Center frequency: f₀ = 1/(2πRC)
   - Q ≈ 1 for smooth overlap

2. **Precision Rectifiers**
   - Op-amp + diode for zero-threshold rectification
   - Converts AC audio to DC level

3. **RC Envelope Following**
   - Fast attack (~1ms)
   - Slow decay (~100ms)
   - Smooth LED action

4. **Peak Hold Capacitors**
   - Diode charges cap to peak
   - Slow discharge through 470kΩ
   - Creates "floating dot" effect

5. **LM3915 Logarithmic Display**
   - Built-in log converter (dB scale)
   - Matches human hearing
   - Drives 10 LEDs per IC

---

## 🚀 Getting Started

### Quick Start (3 Steps)

1. **Read the documentation:**
   - Start with [CIRCUIT_DESIGN.md](CIRCUIT_DESIGN.md)
   - Understand each stage before building

2. **Buy components:**
   - Use [BOM_SHOPPING_LIST.md](BOM_SHOPPING_LIST.md) as shopping list
   - Order from Robu.in, Evelta.com, or local shop

3. **Build and test:**
   - Follow [ASSEMBLY_GUIDE.md](ASSEMBLY_GUIDE.md) step-by-step
   - Build one stage at a time
   - Test each stage before proceeding

### Recommended Build Order

```
1. Power Supply (test first!)
    ↓
2. Input Buffer (verify audio input)
    ↓
3. Filter Bank (test with oscilloscope)
    ↓
4. Envelope Detectors (measure DC outputs)
    ↓
5. LED Drivers (see first lights!)
    ↓
6. Peak Hold (add floating dots)
    ↓
7. Audio Output (optional speaker)
```

---

## 📊 Testing & Calibration

### Test Tones

Use a tone generator app to test each band:

| Frequency | Expected Band | LED Color |
|-----------|---------------|-----------|
| 50-100 Hz | Band 1 (Bass) | Green |
| 500-1000 Hz | Band 2 (Mid-Low) | Yellow |
| 2-4 kHz | Band 3 (Mid-High) | Orange |
| 8-12 kHz | Band 4 (Treble) | Red |

### Recommended Test Tracks

1. **Dubstep/EDM** - Heavy bass → Green LEDs dominate
2. **Acoustic vocals** - Mid frequencies → Yellow/Orange active
3. **Jazz cymbals** - High frequencies → Red LEDs spike
4. **Rock/pop** - Full spectrum → All colors active

---

## 🔍 Troubleshooting

### Common Issues

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| No LEDs light | Power supply issue | Check +5V rail, MODE pin |
| All LEDs maxed | Too much gain | Reduce input gain pot |
| LEDs dim | Wrong reference resistor | Check 1.2kΩ on LM3915 |
| Peak hold doesn't work | Wrong LM339 power | Use +5V, not +12V |
| One band silent | Filter issue | Check resistor/cap values |

**See [ASSEMBLY_GUIDE.md](ASSEMBLY_GUIDE.md) for detailed troubleshooting**

---

## 📸 Expected Results

When working correctly:

✅ **Bass-heavy music:** Green LEDs (Band 1) dominate
✅ **Vocals/guitars:** Yellow/Orange LEDs (Bands 2-3) active
✅ **Cymbals/hi-hats:** Red LEDs (Band 4) spike
✅ **Full mix:** All bands respond with different patterns
✅ **Peak hold:** Blue LEDs catch peaks and slowly fall
✅ **BAR mode:** LEDs fill from bottom to top
✅ **DOT mode:** Single LED moves up/down

---

## 🎯 Learning Outcomes

By building this project, you'll understand:

- ✅ Op-amp circuits (followers, filters, rectifiers)
- ✅ Active filter design (Sallen-Key topology)
- ✅ Signal conditioning (buffering, biasing, coupling)
- ✅ AC to DC conversion (envelope detection)
- ✅ Analog memory (peak hold capacitors)
- ✅ LED driving (current sources, logarithmic scales)
- ✅ Power supply design (regulation, filtering)
- ✅ System integration (multi-stage analog systems)
- ✅ Debugging (systematic testing, signal tracing)

**This is professional-level analog circuit design!**

---

## 🌟 Improvements & Extensions

### Easy Upgrades
- Add more LEDs per band (10 instead of 5)
- Add adjustable peak hold time (replace resistor with pot)
- Add microphone input preamp
- Add stereo input (duplicate for left/right channels)

### Advanced Projects
- 8-band or 16-band analyzer
- VU meter mode (slower response)
- RGB LED matrix display
- Remote control (IR receiver)
- Logarithmic frequency spacing

---

## 🛒 Where to Buy in India

### Online Stores
- **Robu.in** - Wide selection, good prices
- **Evelta.com** - Reliable, fast shipping
- **Amazon.in** - Basic components
- **JLCPCB.com** - Custom PCBs (₹200 for 5 pieces)

### Physical Stores
- **SP Road, Bangalore** - Best electronics market in South India
- **Lamington Road, Mumbai** - Huge selection
- **Nehru Place, Delhi** - North India hub
- **Ritchie Street, Chennai** - Good prices

---

## 📜 License

This project documentation is provided as educational material for electronics enthusiasts and students.

**You are free to:**
- Build this circuit for personal use
- Modify and improve the design
- Use for educational/academic projects
- Share with attribution

**Attribution:**
If you build this or use the documentation, a link back to this repository would be appreciated!

---

## 🙏 Acknowledgments

- Original concept from classic analog spectrum analyzer designs
- Filter design based on Sallen-Key topology
- Thanks to the makers community for analog electronics knowledge

---

## 📞 Support

**Questions? Issues?**
- Read the documentation thoroughly first
- Check the troubleshooting section
- Measure voltages and signals systematically
- Post issues on GitHub (not email)

---

## 🎉 Success Stories

Built this project? Share your success!
- Post photos/videos
- Tag: `#AnalogSpectrumAnalyzer` `#PureAnalog` `#NoArduino`
- Link back to this repo

---

**Good luck with your build! 🚀🎵🔊**

*Remember: This is pure analog. No code to debug, just beautiful circuit design!*
