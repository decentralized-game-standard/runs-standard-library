# RUNS Standard Library ("The Vocabulary")

The RUNS Standard Library provides semantic agreement on data shapes. Interoperability starts here.

## Core Schemas

**Schema**: `runs:root`
- **Definition**: The structural root of an entity logic tree.
- **Fields**: none (tag)

**Schema**: `runs:time`
- **Definition**: Time progression data.
- **Fields**: 
    - `delta_seconds` (f32)
    - `total_seconds` (f64)
    - `frame_count` (u64)

**Schema**: `runs:transform`
- **Definition**: 3D Euclidean spatial data.
- **Fields**:
    - `position` (Vec3)
    - `rotation` (Quat)
    - `scale` (Vec3)

**Schema**: `runs:input`
- **Definition**: HID state abstraction.
- **Fields**:
    - `axes` (Map<String, f32>)
    - `buttons` (Set<String>)

## Usage

Engines SHOULD implement these schemas to support the wider ecosystem of Processors. A "Physics Processor" expects `runs:transform`. If your engine calls it `PositionComponent`, the Processor won't work.

This library is not exhaustive. It is the minimal set required for basic interop.
