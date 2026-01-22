# RUNS Standard Library ("The Vocabulary")

🏠 **[Overview](https://github.com/decentralized-game-standard)** · 🔧 **[RUNS](https://github.com/decentralized-game-standard/runs-standard)** · 📦 **[AEMS](https://github.com/decentralized-game-standard/aems-standard)** · ⚡ **[WOCS](https://github.com/decentralized-game-standard/wocs-standard)** · ❓ **[FAQ](https://github.com/decentralized-game-standard/.github/blob/main/profile/FAQ.md)**

> **Status**: Draft / RFC  
> **Version**: 0.1.0

---

Interoperability in decentralized gaming begins with a shared language.

The **RUNS Standard Library** establishes semantic agreement on fundamental data shapes. While the RUNS Protocol defines *how* data flows through Records and Processors, the Standard Library defines *what* that data means. Without this shared vocabulary, a Physics Processor built by one developer cannot reliably drive a Rendering Processor from another—names mismatch, assumptions diverge, composition fails.

This library provides the minimal, neutral set of schemas required for basic cross-engine compatibility. It is deliberately small: just enough to bootstrap an ecosystem where specialized Processors can assume the presence of common concepts like "time progression," "spatial transform," or "input state."

Think of it as the common words in a constructed language—sufficient for basic communication, leaving room for rich dialects to emerge.

## Why a Shared Vocabulary?

Modern game engines force lock-in through proprietary component names and layouts. A "Transform" in Unity differs subtly from one in Unreal; input handling varies; time deltas are exposed differently. Mods and extensions must target one specific engine.

The RUNS Standard Library breaks this pattern:

- **Composable Processors** — A third-party "Gravity Processor" can safely read and write `runs:transform` without knowing which engine is running underneath.
- **Engine Diversity** — Minimal runtimes (e.g., for web or embedded) and high-performance ones can coexist, sharing the same Processor marketplace.
- **Future-Proofing** — As long as schemas remain stable, old Processors continue working on new runtimes decades later.

This library is **not** exhaustive. It avoids genre-specific assumptions (e.g., no "Health" or "Inventory" by default). Custom schemas remain fully supported—engines and games can define their own while still participating in the shared core.

## Core Schemas

These schemas form the foundation for basic interoperability.

| Schema            | Purpose                          | Fields                                      | Notes                          |
|-------------------|----------------------------------|---------------------------------------------|--------------------------------|
| `runs:root`       | Structural root of an entity logic tree | None (tag only)                             | Marks the entry point          |
| `runs:time`       | Time progression data            | `delta_seconds` (f32)<br>`total_seconds` (f64)<br>`frame_count` (u64) | Core simulation clock          |
| `runs:transform`  | 3D Euclidean spatial data        | `position` (Vec3)<br>`rotation` (Quat)<br>`scale` (Vec3) | Standard spatial representation |
| `runs:input`      | HID state abstraction            | `axes` (Map<String, f32>)<br>`buttons` (Set<String>) | Generic input mapping          |

### Detailed Definitions

**`runs:root`**  
- **Definition**: A tag schema indicating the structural root of an entity logic tree.  
- **Fields**: None.  
- **Rationale**: Provides a clear entry point for entity composition without carrying data.

**`runs:time`**  
- **Definition**: Represents simulation time progression.  
- **Fields**:  
  - `delta_seconds`: Time elapsed since the last frame (f32).  
  - `total_seconds`: Total elapsed simulation time (f64).  
  - `frame_count`: Cumulative frame counter (u64).  
- **Rationale**: Enables consistent time-stepping across diverse runtimes.

**`runs:transform`**  
- **Definition**: Standard 3D spatial transform.  
- **Fields**:  
  - `position`: Translation vector (Vec3).  
  - `rotation`: Orientation quaternion (Quat).  
  - `scale`: Non-uniform scaling (Vec3).  
- **Rationale**: The most common spatial data shape, expected by physics, rendering, and animation Processors.

**`runs:input`**  
- **Definition**: Abstracted human interface device state.  
- **Fields**:  
  - `axes`: Named analog inputs (e.g., "move_x", "look_y") mapped to normalized values.  
  - `buttons`: Set of active button identifiers (e.g., "jump", "fire").  
- **Rationale**: Decouples input from specific hardware, enabling remapping and cross-device compatibility.

## Usage Guidelines

Engines **SHOULD** implement these schemas exactly to participate fully in the ecosystem. A third-party "Physics Processor" will expect `runs:transform` on relevant Records. If your engine uses a different name (e.g., `PositionComponent`), the Processor cannot plug in seamlessly.

Example failure case:  
An engine renames the schema to `my:position`. A community-built "Orbit Camera Processor" that reads `runs:transform` will silently fail or require forking.

This library remains intentionally minimal. Extensions (e.g., `runs:velocity`, `runs:hierarchy`) may be proposed via RFC as the ecosystem matures.

## What the Standard Library Deliberately Excludes

| Excluded                  | Why                                      | Where It Belongs                  |
|---------------------------|------------------------------------------|-----------------------------------|
| Genre-specific schemas    | Avoids biasing toward spatial/FPS games  | Custom packages or future layers  |
| Implementation details    | Schemas define shape, not behavior       | Individual Processors             |
| Exhaustive coverage       | Keeps the core lightweight and neutral   | Community extensions              |

These exclusions ensure the library remains a true common foundation rather than a bloated framework.

## Integration with RUNS

The Standard Library is Layer 2 in the RUNS architecture—bridging the raw Protocol (Layer 1) and the diverse Ecosystem (Layer 3). Processors targeting these schemas become universally composable.

## Summary

The RUNS Standard Library is the shared vocabulary that turns isolated engines into a collaborative ecosystem. Implement it. Build on it. Extend beyond it.

Small agreements enable vast composition.

**MIT License** — Open for implementation, extension, critique.