# Quick Reference Guide

**One-page reference for building the analog spectrum analyzer**

---

## IC Pin Connections (Quick Lookup)

### LM741 / TL071 (8-pin DIP) - Input Buffer (IC1)
```
Pin 1: NC (or offset null)
Pin 2: -IN (feedback from pin 6)
Pin 3: +IN (audio input)
Pin 4: V- (GND)
Pin 5: NC (or offset null)
Pin 6: OUT (buffered audio)
Pin 7: V+ (+12V)
Pin 8: NC
```

### LM358 (8-pin DIP) - Dual Op-Amp
```
Pin 1: OUT A
Pin 2: -IN A
Pin 3: +IN A
Pin 4: GND
Pin 5: +IN B
Pin 6: -IN B
Pin 7: OUT B
Pin 8: V+ (+12V)
```

**IC2: Filters Band 1 & 2**
**IC3: Filters Band 3 & 4**
**IC4: Envelope Band 1 & 2**
**IC5: Envelope Band 3 & 4**

### LM339 (14-pin DIP) - Quad Comparator (IC6)
```
Pin 1: OUT 1 (Band 1 peak LED)
Pin 2: -IN 1 (peak voltage)
Pin 3: V+ (+5V)
Pin 4: +IN 2 (Band 2 signal)
Pin 5: -IN 2 (peak voltage)
Pin 6: OUT 2 (Band 2 peak LED)
Pin 7: OUT 3 (Band 3 peak LED)
Pin 8: +IN 3 (Band 3 signal)
Pin 9: +IN 4 (Band 4 signal)
Pin 10: -IN 3 (peak voltage)
Pin 11: -IN 4 (peak voltage)
Pin 12: GND
Pin 13: OUT 4 (Band 4 peak LED)
Pin 14: NC
```

**NOTE: Pin 3 = +5V (not +12V!)**

### LM3915 (18-pin DIP) - LED Driver (IC7-IC10)
```
Pin 1: LED 2
Pin 2: GND
Pin 3: V+ (+5V)
Pin 4: RLO (reference low = GND)
Pin 5: SIG (DC input from envelope detector)
Pin 6: RHI (reference high = +5V)
Pin 7: REF OUT (1.25V reference)
Pin 8: REF ADJ (via 1.2kΩ to pin 7)
Pin 9: MODE (BAR = +5V, DOT = GND)
Pin 10: LED 3
Pin 11-17: Other LEDs (unused in 5-LED config)
Pin 18: LED 1 (lowest)
```

**Use pins 18, 1, 10, 9, 8 for 5 LEDs per band**

### LM386 (8-pin DIP) - Audio Amplifier (IC11)
```
Pin 1: GAIN (connect to pin 8 via 10µF for max gain)
Pin 2: -IN (GND)
Pin 3: +IN (audio input via 10µF + pot)
Pin 4: GND
Pin 5: OUT (to speaker via 100µF + 10Ω)
Pin 6: V+ (+12V)
Pin 7: BYPASS (10µF to GND)
Pin 8: GAIN (see pin 1)
```

### LM7812 / LM7805 (TO-220) - Voltage Regulators
```
Pin 1 (left): INPUT (unregulated DC)
Pin 2 (center): GND
Pin 3 (right): OUTPUT (regulated DC)

⚠️ Use heatsinks! Both ICs dissipate heat.
```

---

## Component Values by Band

### Filter Resistors (Stage 2)
| Band | Center Freq | R value | C value |
|------|-------------|---------|---------|
| 1 (Bass) | 100 Hz | 15kΩ | 100nF |
| 2 (Mid-Low) | 700 Hz | 2.2kΩ | 100nF |
| 3 (Mid-High) | 3 kHz | 560Ω | 100nF |
| 4 (Treble) | 10 kHz | 150Ω | 100nF |

**Each filter needs:** 3× R + 2× C (100nF)

### Envelope Detector (Stage 3)
**Per band:**
- 1× 1N4148 diode
- 1× 10kΩ input resistor
- 1× 10kΩ RC resistor
- 1× 10µF RC capacitor

### Peak Hold (Stage 4)
**Per band:**
- 1× 1N4148 diode
- 1× 10µF peak storage cap
- 1× 470kΩ discharge resistor
- 1× 470Ω LED resistor
- 1× Blue LED

### LED Driver (Stage 5)
**Per band:**
- 1× LM3915 IC
- 1× 1.2kΩ reference resistor (or 10kΩ pot)
- 5× 100Ω LED resistors
- 5× LEDs (color per band)

---

## Power Supply Voltages

| Rail | Voltage | Tolerance | Used By |
|------|---------|-----------|---------|
| +12V | 12.0V | ±0.3V | IC1, IC2, IC3, IC4, IC5, IC11 |
| +5V | 5.0V | ±0.2V | IC6, IC7, IC8, IC9, IC10 |
| GND | 0V | - | All ICs |

**Current Draw:**
- +12V rail: ~15-20 mA (quiescent) + 300 mA (LM386 peak)
- +5V rail: ~250-300 mA (with all LEDs on)

---

## Signal Levels at Test Points

| Test Point | Signal Type | Typical Level | What It Should Do |
|------------|-------------|---------------|-------------------|
| TP1 (IC1-6) | AC audio | ±0.5V to ±2V | Match input signal |
| TP2 (IC2-1) | AC filtered (100Hz) | ±0.2V to ±1V | Vary with bass |
| TP3 (IC2-7) | AC filtered (700Hz) | ±0.2V to ±1V | Vary with mids |
| TP4 (IC3-1) | AC filtered (3kHz) | ±0.2V to ±1V | Vary with highs |
| TP5 (IC3-7) | AC filtered (10kHz) | ±0.2V to ±1V | Vary with treble |
| TP6 (IC4-1) | DC envelope | 0-8V DC | Vary with bass intensity |
| TP7 (IC4-7) | DC envelope | 0-8V DC | Vary with mid intensity |
| TP8 (IC5-1) | DC envelope | 0-8V DC | Vary with high intensity |
| TP9 (IC5-7) | DC envelope | 0-8V DC | Vary with treble intensity |

---

## LED Color Coding

| Band | Frequency | LEDs | Peak LED | When Active |
|------|-----------|------|----------|-------------|
| 1 | Bass (100 Hz) | 5× Green | 1× Blue | Kick drums, bass guitar |
| 2 | Mid-Low (700 Hz) | 5× Yellow | 1× Blue | Vocals, rhythm guitar |
| 3 | Mid-High (3 kHz) | 5× Orange | 1× Blue | Snare, presence |
| 4 | Treble (10 kHz) | 5× Red | 1× Blue | Cymbals, hi-hats |

---

## Common Mistakes to Avoid

### ❌ DON'T:
1. **Power LM339 (IC6) with +12V** → Use +5V only!
2. **Reverse electrolytic capacitor polarity** → Check + marking!
3. **Forget heatsinks on LM7812/LM7805** → They overheat!
4. **Use wrong resistor values** → Double-check color codes!
5. **Connect LEDs backward** → Long leg = anode (+)
6. **Forget MODE pin on LM3915** → Must be +5V or GND, not floating
7. **Use 1.2MΩ instead of 1.2kΩ** → Check resistor value carefully!
8. **Short AC mains to DC circuit** → Keep AC far from DC!

### ✅ DO:
1. **Test power supply first** before connecting ICs
2. **Build one stage at a time** and test before proceeding
3. **Use IC sockets** for easy replacement if IC fails
4. **Measure voltages** at each test point with multimeter
5. **Double-check connections** before applying power
6. **Use decoupling capacitors** (0.1µF) near each IC
7. **Keep wires neat** to avoid shorts and interference
8. **Label everything** with tape/marker for easy debugging

---

## Troubleshooting Flowchart

```
No LEDs lighting?
    ↓
Check power supply
    ├─ +12V present? → NO → Check LM7812, input voltage
    └─ +5V present? → NO → Check LM7805, +12V rail
    ↓
Check LM3915 power
    ├─ Pin 3 = +5V? → NO → Fix power connection
    └─ Pin 2 = GND? → NO → Fix ground connection
    ↓
Check MODE pin
    └─ Pin 9 = +5V or GND? → Floating? → Connect to +5V (BAR mode)
    ↓
Check LED connections
    ├─ Correct polarity? → NO → Reverse LED
    └─ 100Ω resistor present? → NO → Add resistor
    ↓
Check DC signal at LM3915 pin 5
    └─ 0-8V varying with music? → NO → Check envelope detector
    ↓
If still not working → See detailed troubleshooting in ASSEMBLY_GUIDE.md
```

---

## Quick Resistor Color Code Reference

| Value | Band 1 | Band 2 | Band 3 | Band 4 |
|-------|--------|--------|--------|--------|
| 10Ω | Brown | Black | Black | Gold |
| 100Ω | Brown | Black | Brown | Gold |
| 150Ω | Brown | Green | Brown | Gold |
| 470Ω | Yellow | Violet | Brown | Gold |
| 560Ω | Green | Blue | Brown | Gold |
| 1.2kΩ | Brown | Red | Red | Gold |
| 2.2kΩ | Red | Red | Red | Gold |
| 10kΩ | Brown | Black | Orange | Gold |
| 15kΩ | Brown | Green | Orange | Gold |
| 47kΩ | Yellow | Violet | Orange | Gold |
| 470kΩ | Yellow | Violet | Yellow | Gold |

**Tolerance:** Gold = ±5%, Silver = ±10%

---

## Capacitor Quick Reference

### Electrolytic (Polarized - Watch polarity!)
| Value | Voltage | Typical Size | Purpose |
|-------|---------|--------------|---------|
| 10µF | 16V | 5mm × 11mm | Envelope RC, peak hold, LM386 |
| 100µF | 16V | 6.3mm × 11mm | LM386 output |
| 470µF | 16V | 8mm × 11.5mm | +5V regulator filter |
| 1000µF | 25V | 10mm × 16mm | Power supply main filter |

### Ceramic/Polyester (Non-polarized)
| Value | Marking | Purpose |
|-------|---------|---------|
| 0.1µF (100nF) | 104 | Filters, decoupling |

**104 = 10 × 10⁴ pF = 100,000 pF = 100nF = 0.1µF**

---

## Testing Music Tracks

**Quick test:** Use these specific sounds to verify each band

| Test | Play This | Watch For |
|------|-----------|-----------|
| Band 1 | 50 Hz sine wave or dubstep bass | Green LEDs |
| Band 2 | 700 Hz tone or male vocals | Yellow LEDs |
| Band 3 | 3 kHz tone or snare drum | Orange LEDs |
| Band 4 | 10 kHz tone or cymbals | Red LEDs |
| Full test | Full mix (rock/pop) | All bands active |

**Test tone apps (free):**
- Android: "Frequency Sound Generator"
- iOS: "Tone Generator"
- Web: onlinetonegenerator.com

---

## Minimum Viable Build (Get Something Working Fast)

**Phase 1: Just Power + One Band (2-3 hours)**
1. Build power supply (+12V, +5V)
2. Build input buffer (IC1)
3. Build ONE filter (IC2, Band 1)
4. Build ONE envelope detector (IC4, Band 1)
5. Build ONE LED driver (IC7, Band 1)
6. Test with bass-heavy music → 5 green LEDs should respond!

**Phase 2: Complete Display (4-6 hours)**
7. Add remaining 3 filters (IC2, IC3)
8. Add remaining 3 envelope detectors (IC4, IC5)
9. Add remaining 3 LED drivers (IC8, IC9, IC10)
10. Test all 4 bands

**Phase 3: Polish (2-3 hours)**
11. Add peak hold circuit (IC6)
12. Add audio output (IC11)
13. Add mode switch, gain pots
14. Final calibration

---

## Essential Tools

**Minimum:**
- ✅ Multimeter (voltage measurement)
- ✅ Soldering iron + solder
- ✅ Wire cutters/strippers
- ✅ Screwdriver set
- ✅ Breadboard OR PCB

**Helpful:**
- ⭐ Oscilloscope (for viewing waveforms)
- ⭐ Function generator (for filter testing)
- ⭐ Desoldering pump (for fixing mistakes)

**Can work without oscilloscope?** YES! Use your ears (speaker output) and eyes (LEDs).

---

## Emergency Contacts & Resources

**Where to Get Help:**
- Read documentation thoroughly first
- Check troubleshooting section in ASSEMBLY_GUIDE.md
- Post issues on GitHub repository
- Electronics forums: EEVblog, r/AskElectronics

**Datasheets (if you need them):**
- LM741: ti.com/lit/ds/symlink/lm741.pdf
- LM358: ti.com/lit/ds/symlink/lm358.pdf
- LM339: ti.com/lit/ds/symlink/lm339.pdf
- LM3915: ti.com/lit/ds/symlink/lm3915.pdf
- LM386: ti.com/lit/ds/symlink/lm386.pdf

**Component Substitution?**
- See COMPONENT_CALCULATIONS.md for alternatives
- When in doubt, stick with specified parts

---

## Success Criteria

**Your project works if:**
- ✅ All 4 bands light up with music
- ✅ Green (bass) responds to low frequencies
- ✅ Red (treble) responds to high frequencies
- ✅ Peak LEDs catch peaks and slowly fall
- ✅ BAR mode fills from bottom to top
- ✅ Speaker plays audio (if included)

**Bonus points:**
- ⭐ Clean, professional appearance
- ⭐ Smooth LED action (not flickering)
- ⭐ Good frequency separation
- ⭐ PCB instead of breadboard
- ⭐ Nice enclosure with labels

---

**GOOD LUCK! 🚀**

*This is pure analog engineering - you're building a real spectrum analyzer like the pros used before digital processing existed!*

**Print this page and keep it next to your workbench!**
