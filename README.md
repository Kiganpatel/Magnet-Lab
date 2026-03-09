# Magnet-Lab
2D magnetic physics sandbox — place bar, disc, ring and sphere magnets in four real materials, watch them snap and stick with dipole force physics. Iron filings, compass, eddy current braking, diamagnetics, and animated field-line streamers.

*[Open magnet-lab.html to play](magnet-lab.html)**

---

## Overview

An interactive browser simulation built from real dipole equations. Every magnet is two point charges. Force between any two poles = `K × q₁ × q₂ / (r² + SOFT)`. The 1/r⁴ dipole-dipole falloff at long range emerges naturally — no special formula needed.

The simulation includes four real magnet materials with strength ratios derived from actual BHmax values, ferromagnetic gradient attraction, diamagnetic repulsion (bismuth), and eddy current braking (copper).

---

## Physics

### Why magnets now actually stick

Three root causes of the previous "flying everywhere" bug:

```js
// 1. Angular damping was too low — confirmed fix from Box2D/Bullet research:
//    "Angular damping is crucial — even small penetration generates
//     significant angular momentum change." — gamedev.net/topic/712061
DAMP_W = 0.70   // kills spin in ~3 frames (was 0.90 = runaway torque)

// 2. Spring collision = elastic bouncing forever
//    Fixed with impulse-based collision + COR = 0.05 (nearly inelastic)
//    95% of approach kinetic energy absorbed on contact

// 3. Inertia was too low — fast angular acceleration from any asymmetric force
INERT = { bar: 65, horseshoe: 130 }   // was 8, 12
```

### Core parameters

```js
K          = 2800   // Force constant
SOFT       = 100    // Softening — no singularity at r=0
DAMP_V     = 0.86   // Surface friction (linear, per frame)
DAMP_W     = 0.70   // Angular friction (per frame) — CRITICAL
MAX_V      = 12     // Max velocity px/frame
MAX_OM     = 0.08   // Max angular velocity rad/frame
SUBSTEPS   = 4      // Integration substeps
COR        = 0.05   // Coefficient of restitution
PAIR_CLAMP = 10     // Max force per pole pair
TORQ_CLAMP = 3.5    // Max torque per pole pair
```

### Materials

| Material | Pole Charge | Real BHmax | Notes |
|----------|-------------|-----------|-------|
| Ferrite | 2 | ~3.5 MGOe | Common fridge magnet. Cheap, stable. |
| Alnico 5 | 4.5 | ~5.5 MGOe | Survives 500°C. Easy to demagnetize. |
| Neodymium N52 | 8 | 52 MGOe | Strongest ever. Brittle, fails above 80°C. |
| Samarium-Cobalt | 6.5 | ~30 MGOe | Rated 300°C+. Aerospace grade. |

Force between two neo poles = K × 64 / r² — 16× stronger than ferrite-ferrite at same distance.

---

## Controls

| Input | Action |
|-------|--------|
| Click toolbar → click canvas | Place object |
| Drag | Move (throw on release) |
| Right-click | Delete |
| Double-click coil | Flip electromagnet polarity |
| Pin → click | Lock / unlock object position |
| Force slider | Scale all forces ×0.2–×3.0 |
| Hover toolbar | Material science info |

---

## Files

```
magnet-lab.html      Game + docs merged — single self-contained file
magnet-devlog.html   Full development history with per-version notes
README.md            This file
```

---

## Tech

Vanilla JavaScript, HTML5 Canvas 2D, Google Fonts. No build step, no frameworks, no dependencies. Single file — copy anywhere and open in a browser.

---

Built by [Kigan Patel](https://github.com/kiganpatel)
