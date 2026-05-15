# Smart Room Access Controller

A digital logic-based automated room management system designed in Logic Works 5 as part of the Digital Logic Design Lab project at FAST-NUCES Lahore.

## 📌 Overview

This project implements an automated lighting and access control system for a 16-person room using purely combinational and sequential logic — no microprocessors involved. It tracks occupancy in real time, controls zone lighting, manages door locks, and displays the current headcount on dual 7-segment displays.

## ✨ Features

- **Occupancy Tracking** — 5-bit bidirectional counter tracks room count from 0 to 16
- **Zone Lighting** — 6 independent light zones activate only when their seats are occupied
- **Smart Door Locks** — Entry locks at full capacity (16), exit locks when room is empty (0)
- **Live Display** — Dual 7-segment displays show current occupancy count (00–16)
- **Sequential Seating** — Enforces front-to-back seating order; departures cascade seats forward

## 🧱 System Architecture

The design is organized into four functional blocks:

| Block | Description |
|---|---|
| **5-Bit Counter** | Custom bidirectional up/down counter; increments on entry, decrements on exit |
| **Entry/Exit Logic** | Inverter and gate logic that locks doors at boundary states (0 and 16) |
| **Binary Input Decoder** | Converts 5-bit binary output to BCD for dual 7-segment display |
| **Light Decoder** | Combinational logic that evaluates zone occupancy and drives the 6 light outputs |

## 🔌 Inputs & Outputs

**Inputs**
- `CLK` — Global clock pulse
- `Person` — Toggle signal (1 = add person, 0 = remove person)

**Outputs**
- `L1–L6` — Zone light status signals
- `ENTRY` / `EXIT` — Door lock indicators
- `7-Seg Display` — Dual display showing occupancy (00–16)

## 💡 Light Zone Logic

Each zone light activates based on minimum seat thresholds derived from the 5-bit counter output (S4–S0):

| Light | Condition | Boolean Expression |
|---|---|---|
| L-1 | ≥ 1 person | `S4 + S3 + S2 + S1 + S0` |
| L-2 | ≥ 3 people | `S4 + S3 + S2 + S1·S0` |
| L-3 | ≥ 7 people | `S4 + S3 + S2·S1·S0` |
| L-4 | ≥ 13 people | `S4 + S3·S2·(S1 + S0)` |
| L-5 | ≥ 9 people | `S4 + S3·(S2 + S1·S0)` |
| L-6 | ≥ 15 people | `S4 + S3·S2·S1·S0` |

## ⚙️ Design Approach

Three solutions were evaluated:

- **Option A (Chosen)** — Bidirectional binary counter + combinational decoder. Most gate-efficient; a single 5-bit counter handles all 16 occupancy states.
- **Option B** — Cascaded 16-bit shift register (74LS164). Models individual seats natively but requires an external priority encoder for display output.
- **Option C** — Individual SR-latch array per seat. Finest granularity but highly gate-inefficient at scale.

## 🛠️ Tools Used

- Logic Works 5 (simulation and schematic)
- BCD-to-7-segment decoder ICs
- Standard logic gates (NOT, AND, OR, NOR, NAND)
- 5 D flip-flops (5-bit counter)

## 📊 Gate Count

| Component | Count |
|---|---|
| NOT gates | 5 |
| AND gates | 3 |
| OR / NOR gates | 2 |
| NAND gates | 1 |
| D Flip-Flops | 5 |
| Total gate equivalents | ~40–50 |

## ✅ Performance Summary

- 100% reliable state handling across all 17 states (0–16) with no counter overflow
- Clock-synchronized response — enclosure light activates within one clock cycle of entry
- Boundary state security confirmed via forced-input analysis at counts 0 and 16
- 09→10 and 15→16 display transitions verified for correct carry handling

## 📄 Course Info

**Course:** EE-223 Digital Logic Design Lab  
**Instructor:** Miss Khadija Farrukh  
**Department:** Electrical Engineering, FAST-NUCES Lahore
