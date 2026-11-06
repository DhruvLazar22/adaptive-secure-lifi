# Adaptive Secure Li-Fi: AES-128-GCM + Dynamic Power Control

Design and simulation of an indoor **Li-Fi (visible-light) link** that is both **secure** (authenticated encryption) and **energy-efficient**. The LED automatically turns its power down when the receiver is close.

**Course:** Optical Fiber Communications, VIT Vellore
**Team:** Jevith Reddy, Dhruv Lazar, Aman Saini
**Guide:** Tangudu Ramji
**Tools:** MATLAB / ModelSim simulation, optical channel modelling

---

## The two problems

1. **Security:** light doesn't pass through walls, but an eavesdropper inside the room can still pick up the signal.
   - Fix: **AES-128-GCM** encryption with 96-bit nonces and 128-bit authentication tags. This protects against eavesdropping, tampering and replay attacks.
2. **Wasted power:** LEDs usually run at full power all the time, even when the receiver is right underneath.
   - Fix: **Dynamic Transmission Power Control (DTPC)**, a closed-loop controller that sets LED current to just what's needed for a target SNR (22 dB).

## Link design

| Block | Details |
|---|---|
| Framing | Preamble `0xAA55` + start byte `0x7E` + sequence number/length header + encrypted payload + **CRC-16-CCITT** |
| Modulation | On-Off Keying (OOK), 100 kbps |
| Transmitter | 1 W white LED, half-power angle 60° (Lambertian order m = 1) |
| Channel | Line-of-sight Lambertian model |
| Receiver | BPW34 PIN photodiode + optical concentrator (gain ≈ 2.55, 70° field of view) + transimpedance amplifier |
| Noise | Shot noise + thermal noise (thermal dominates) |

## Results

| Distance | LED current, fixed → adaptive | SNR | Energy saved |
|---|---|---|---|
| 0.5 m | 350 → 136 mA | 65.8 → 57.7 dB | **61%** |
| 1.0 to 3.5 m | 350 mA | 53.8 → 32.1 dB | 0% (full power needed) |

- Error-free delivery within a 3 m cell.
- About **18 °C cooler** LED junction at close range, which extends LED life.
- The controller automatically jumps to full power when the light path is blocked.
- 8 analysis figures:
  1. LED radiation pattern
  2. Concentrator gain
  3. BER vs SNR
  4. Energy per bit
  5. Mobile tracking
  6. Power savings over time
  7. Junction temperature
  8. Eye diagram

## Future work

- Hardware testbed
- Multipath / reflection channel model
- Multi-user Li-Fi
- Machine-learning channel prediction
