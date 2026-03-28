# Component Calculations & Design Formulas

## Filter Bank Design (Sallen-Key Bandpass)

### Formula:
```
Center Frequency: f₀ = 1 / (2π × R × C)

Rearranging for R:
R = 1 / (2π × f₀ × C)
```

### Design Calculations:

**Fixed parameter:** C = 100nF = 100 × 10⁻⁹ F

#### Band 1 - Bass (f₀ = 100 Hz):
```
R = 1 / (2π × 100 × 100×10⁻⁹)
R = 1 / (6.2832 × 10⁻⁵)
R = 15,915 Ω
≈ 15kΩ (standard value)

Actual f₀ with 15kΩ:
f₀ = 1 / (2π × 15,000 × 100×10⁻⁹) = 106.1 Hz ✓
```

#### Band 2 - Mid-Low (f₀ = 700 Hz):
```
R = 1 / (2π × 700 × 100×10⁻⁹)
R = 1 / (4.398 × 10⁻⁴)
R = 2,274 Ω
≈ 2.2kΩ (standard value)

Actual f₀ with 2.2kΩ:
f₀ = 1 / (2π × 2,200 × 100×10⁻⁹) = 723.4 Hz ✓
```

#### Band 3 - Mid-High (f₀ = 3 kHz):
```
R = 1 / (2π × 3,000 × 100×10⁻⁹)
R = 1 / (1.885 × 10⁻³)
R = 531 Ω
≈ 560Ω (standard value)

Actual f₀ with 560Ω:
f₀ = 1 / (2π × 560 × 100×10⁻⁹) = 2,842 Hz ✓
```

#### Band 4 - Treble (f₀ = 10 kHz):
```
R = 1 / (2π × 10,000 × 100×10⁻⁹)
R = 1 / (6.283 × 10⁻³)
R = 159.2 Ω
≈ 150Ω (standard value)

Actual f₀ with 150Ω:
f₀ = 1 / (2π × 150 × 100×10⁻⁹) = 10,610 Hz ✓
```

### Summary Table:

| Band | Target f₀ | Calculated R | Standard R | Actual f₀ | Error |
|------|-----------|--------------|------------|-----------|-------|
| 1 | 100 Hz | 15.9 kΩ | 15 kΩ | 106.1 Hz | +6.1% |
| 2 | 700 Hz | 2.27 kΩ | 2.2 kΩ | 723.4 Hz | +3.3% |
| 3 | 3 kHz | 531 Ω | 560 Ω | 2,842 Hz | -5.3% |
| 4 | 10 kHz | 159 Ω | 150 Ω | 10,610 Hz | +6.1% |

**All errors < 7%, which is excellent for audio applications!**

---

## Envelope Detector Time Constants

### RC Time Constant Formula:
```
τ = R × C
```

### Attack Time (Rise):
- Determined by diode forward conduction and op-amp slew rate
- Practically: **~1 ms** (very fast response to transients)

### Decay Time (Fall):
```
R = 10kΩ = 10,000 Ω
C = 10µF = 10 × 10⁻⁶ F

τ = 10,000 × 10×10⁻⁶
τ = 0.1 seconds = 100 ms
```

**Decay time: 100 ms** (smooth LED action, not too flickery)

For 63% decay: t = τ = 100 ms
For 95% decay: t ≈ 3τ = 300 ms

This gives a natural "bouncing" look to the LEDs.

### Adjusting Decay Time:

**Faster decay (more responsive):**
- Use R = 4.7kΩ → τ = 47 ms

**Slower decay (smoother):**
- Use R = 22kΩ → τ = 220 ms

---

## Peak Hold Time Constant

### Discharge Time:
```
R = 470kΩ = 470,000 Ω
C = 10µF = 10 × 10⁻⁶ F

τ = 470,000 × 10×10⁻⁶
τ = 4.7 seconds
```

**Peak hold time: ~5 seconds**

The peak LED will stay lit for about 5 seconds after the peak occurs, then slowly fade.

### Adjusting Peak Hold Time:

**Faster fall (1 second hold):**
- Use R = 100kΩ → τ = 1 second

**Longer hold (10 seconds):**
- Use R = 1MΩ → τ = 10 seconds

---

## LED Current Calculations

### Bar Graph LEDs (LM3915):

**LM3915 Reference Current Setting:**
```
Pin 7 (REF OUT) to Pin 8 (REF ADJ): R_ref

REF voltage = 1.25V (internal reference)
REF current = 1.25V / R_ref

LED current = 10 × REF current

For R_ref = 1.2kΩ:
I_ref = 1.25 / 1,200 = 1.04 mA
I_LED = 10 × 1.04 = 10.4 mA ✓ (safe for standard LEDs)

For R_ref = 1kΩ:
I_LED = 12.5 mA (brighter)

For R_ref = 1.5kΩ:
I_LED = 8.3 mA (dimmer)
```

**Additional Current Limiting (100Ω per LED):**
```
With +5V supply and 2V LED forward voltage:
I_additional_limit = (5 - 2) / 100 = 30 mA max

Actual current: limited by LM3915 to ~10-12 mA ✓
```

### Peak Hold LEDs (driven by LM339):

**Current through blue LED:**
```
Supply: +5V
LED forward voltage: V_f ≈ 3.2V (blue LED)
Current limiting resistor: R = 470Ω

I_LED = (5 - 3.2) / 470
I_LED = 1.8 / 470
I_LED = 3.83 mA ✓ (dim but visible)

For brighter peak LED:
Use R = 220Ω → I = 8.2 mA (brighter)
```

---

## Power Supply Calculations

### Current Requirements:

**+12V Rail:**
- IC1 (LM741): 2 mA
- IC2 (LM358): 2 mA
- IC3 (LM358): 2 mA
- IC4 (LM358): 2 mA
- IC5 (LM358): 2 mA
- IC11 (LM386): 4 mA quiescent + up to 300 mA (audio)
- **Total +12V: ~320 mA** (with audio), ~15 mA (no audio)

**+5V Rail:**
- IC6 (LM339): 2 mA
- IC7 (LM3915): 10 mA + 5 LEDs × 10 mA = 60 mA
- IC8 (LM3915): 60 mA
- IC9 (LM3915): 60 mA
- IC10 (LM3915): 60 mA
- Peak LEDs: 4 × 4 mA = 16 mA
- **Total +5V: ~256 mA**

### Voltage Regulator Selection:

**LM7812:**
- Input: 15-20V DC (from rectified AC)
- Output: +12V @ 1A max
- **Actual load: 320 mA**
- Dropout voltage: ~2V
- **Minimum input: 14V DC required**

**Transformer selection:**
- For 14V DC minimum after rectification and filtering
- Need AC RMS voltage: V_AC = 14 / (1.414 × 0.9) = 11V
- **Use 12-0-12V transformer** (12V RMS → 12 × 1.414 = 17V peak → ~15V DC after diode drop and filter)

**LM7805:**
- Input: 12V DC (from LM7812 output)
- Output: +5V @ 1A max
- **Actual load: 256 mA**
- Dropout voltage: ~2V
- **Input of 12V is perfect** ✓

### Power Dissipation:

**LM7812:**
```
Input: 15V DC
Output: 12V @ 320 mA
Power dissipation: (15 - 12) × 0.32 = 0.96W

With audio peaks (LM386 drawing 500mA):
P = (15 - 12) × 0.5 = 1.5W
```
**Heatsink required!** (TO-220 heatsink, thermal resistance < 25°C/W)

**LM7805:**
```
Input: 12V
Output: 5V @ 256 mA
Power dissipation: (12 - 5) × 0.256 = 1.79W
```
**Heatsink required!** (TO-220 heatsink)

**Total power consumption:**
- 12V rail: 320 mA × 12V = 3.84W
- 5V rail: 256 mA × 5V = 1.28W
- **Total: ~5.1W** (without audio)
- **Total: ~8-10W** (with audio at moderate volume)

This is very reasonable for a mini project!

---

## Filter Bandwidth Calculations

### Sallen-Key Bandpass Filter:

**Quality Factor Q:**
```
For our design: Q ≈ 1 (set by component ratios)
```

**3dB Bandwidth:**
```
BW = f₀ / Q

For Q = 1:
BW = f₀
```

### Actual Bandwidths:

| Band | Center f₀ | Q | BW (-3dB) | Lower f_L | Upper f_H |
|------|-----------|---|-----------|-----------|-----------|
| 1 | 106 Hz | 1 | 106 Hz | 53 Hz | 159 Hz |
| 2 | 723 Hz | 1 | 723 Hz | 362 Hz | 1,085 Hz |
| 3 | 2,842 Hz | 1 | 2,842 Hz | 1,421 Hz | 4,263 Hz |
| 4 | 10,610 Hz | 1 | 10,610 Hz | 5,305 Hz | 15,915 Hz |

**Note:** Bands overlap significantly (Q = 1 gives broad response). This is **intentional and good** for audio spectrum display because:
1. Real music has energy spread across frequencies
2. Smooth transitions between bands look better
3. No "gaps" in frequency coverage

For **sharper separation**, increase Q by adding more filter stages or using active filter with adjustable Q (requires more components).

---

## Component Tolerances

### Impact of Component Tolerances:

**Resistors (5% tolerance):**
- 15kΩ ± 5% = 14.25kΩ to 15.75kΩ
- Band 1 center frequency: 101 Hz to 112 Hz
- **Impact: ±5% frequency shift** (acceptable!)

**Capacitors (10% tolerance for ceramics, 20% for electrolytics):**
- 100nF ± 10% = 90nF to 110nF
- Band 1 center frequency: 96 Hz to 118 Hz
- **Impact: ±10% frequency shift** (still acceptable for audio display)

### Recommendations:

1. **Use 1% or 2% metal film resistors for filters** if you want precise frequency response (adds ₹20-30 to cost)

2. **Use 5% polyester/polypropylene capacitors for filters** (better tolerance than ceramics for audio)

3. **Electrolytics for power supply and envelope detector:** 20% tolerance is fine (not frequency-critical)

4. **Measure actual values** with multimeter if available and select matched components for symmetry

---

## Oscilloscope Test Points

For debugging with an oscilloscope:

1. **TP1 (IC1 pin 6):** Input buffer output - should see clean audio waveform
2. **TP2 (IC2 pin 1):** Band 1 filtered output - should see only low frequencies
3. **TP3 (IC2 pin 7):** Band 2 filtered output
4. **TP4 (IC3 pin 1):** Band 3 filtered output
5. **TP5 (IC3 pin 7):** Band 4 filtered output
6. **TP6 (IC4 pin 1):** Band 1 envelope (DC) - should vary with bass content
7. **TP7 (IC4 pin 7):** Band 2 envelope (DC)
8. **TP8 (IC5 pin 1):** Band 3 envelope (DC)
9. **TP9 (IC5 pin 7):** Band 4 envelope (DC)
10. **TP10 (+12V rail):** Should measure steady 12V
11. **TP11 (+5V rail):** Should measure steady 5V

Expected waveforms detailed in main CIRCUIT_DESIGN.md testing section.

---

## Alternative Component Substitutions

### Op-Amp Alternatives:

| Original | Alternative | Notes |
|----------|-------------|-------|
| LM741 | TL071, TL081, CA3140 | TL071 is better (lower noise, higher bandwidth) |
| LM358 | TL072 (dual), LM833 (audio dual) | TL072 better for audio, costs ₹25-30 |
| LM324 | TL074 (quad), LM348 | TL074 much better for audio (low noise) |

### Comparator Alternatives:

| Original | Alternative | Notes |
|----------|-------------|-------|
| LM339 | LM393 (dual, need 2x), LM311 (single, need 4x) | LM339 is cheapest for 4 channels |

### LED Driver Alternatives:

| Original | Alternative | Notes |
|----------|-------------|-------|
| LM3915 | LM3914 (linear instead of log) | LM3915 better for audio (log scale matches human hearing) |
| - | LM3916 (VU meter, not bar/dot) | Specialized for VU meter, not recommended |

### Power Amplifier Alternatives:

| Original | Alternative | Notes |
|----------|-------------|-------|
| LM386 | TDA2003, TDA7052, TPA311 | Higher power options if you want louder speaker |

### Voltage Regulator Alternatives:

| Original | Alternative | Notes |
|----------|-------------|-------|
| LM7812 | 7812 (any manufacturer), L7812CV | All pin-compatible |
| LM7805 | 7805 (any manufacturer), L7805CV | All pin-compatible |
| - | LM317 (adjustable, use with resistor divider) | More flexible but needs calculation |

**For this project, stick with the specified components unless you can't find them locally.**
