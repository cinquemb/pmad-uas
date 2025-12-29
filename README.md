# PMAD Universal Address Standard (UAS-v1)

This repository defines a **rigid, substrate-agnostic data schema** for cataloging phase-mediated attractor systems across astrophysics and beyond.

It is **not a theory**.  
It is a **data object standard** whose semantics remain stable whether the substrate is coronal plasma, solar wind turbulence, or a millisecond pulsar.

## Purpose
Enable direct comparison of phase rigidity, basin depth, addressability, and stability across radically different physical systems — using the same fields, same null handling, same scoring.

Current seed atlas contains four validated entries spanning Class I → Class II systems.

## Schema (Mandatory Spine)

Every entry must implement **exactly** these top-level keys:

```json
{
  "address_id": "",
  "pmad_class": "",
  "role": "",
  "object": {},
  "observation": {},
  "phase_signature": {},
  "dynamics": {},
  "stability": {},
  "addressability": {},
  "quality_metrics": {},
  "provenance": {}
}
```

Anything missing → **invalid address**.

---

## 2. Object Block (what exists physically)

Same fields for plasma and neutron stars.

```json
"object": {
  "name": "",
  "astrophysical_type": "",
  "subtype": "",
  "driving_mechanism": "",
  "equilibrium": "far-from-equilibrium",
  "spatial_extent": "",
  "mass_scale": "",
  "magnetic_structure": ""
}
```

### Examples

| System     | astrophysical_type | driving_mechanism             |
| ---------- | ------------------ | ----------------------------- |
| Solar wind | plasma_flow        | solar magnetic forcing        |
| Corona     | magnetized_plasma  | photospheric footpoint motion |
| Pulsar     | compact_object     | rotational spindown           |

---

## 3. Observation Block (how we see it)

This is where **instrumentation diversity collapses** into invariants.

```json
"observation": {
  "data_product": "",
  "cadence_seconds": null,
  "time_span_seconds": null,
  "channels": [],
  "spatial_resolution": "",
  "preprocessing": []
}
```

Key rule:

> **If cadence < dominant timescale → reject address**

---

## 4. Phase Signature Block (core PMAD content)

This is the **unifying heart**.

```json
"phase_signature": {
  "dominant_timescales_sec": [],
  "spectral_character": "",
  "embedding": {
    "method": "delay-coordinate",
    "dimension": null,
    "tau_sec": null
  },
  "phase_metric": "cosine",
  "rotation_number": null,
  "phase_coherence": null
}
```

### Interpretation consistency

| System      | rotation_number    |
| ----------- | ------------------ |
| Solar wind  | null / distributed |
| Corona loop | local rational     |
| Pulsar      | sharply defined    |

No special cases.
**Null is a valid value.**

---

## 5. Dynamics Block (attractors, stiffness, chaos)

Your PMAD machinery plugs in directly.

```json
"dynamics": {
  "attractor_type": "",
  "effective_dimensionality": null,
  "stiff_axes_count": null,
  "dominant_axis_index": null,
  "lyapunov_proxy": {
    "global": null,
    "stiff_subspace": null
  }
}
```

This is where **Class II relevance emerges**.

---

## 6. Stability Block (why it persists)

This block decides whether something is **worth cataloging**.

```json
"stability": {
  "basin_depth": "",
  "basin_width": "",
  "relaxation_timescale_sec": null,
  "persistence_fraction": null,
  "noise_floor_relation": ""
}
```

Rule:

> If persistence < 0.5 → **do not include in atlas**

---

## 7. Addressability Block (what it can be used for)

This prevents category mistakes.

```json
"addressability": {
  "addressable": true,
  "addressability_class": "II-capable",
  "bias_sensitivity": "",
  "reciprocity": false,
  "usable_as_anchor": true,
  "restrictions": []
}
```

* **Class III atlas** → always `reciprocity: false`
* Class I → reciprocity required

---

## 8. Quality Metrics

This allows ranking **across astrophysical classes**.

```json
"quality_metrics": {
  "phase_signal_to_noise": null,
  "embedding_robustness": null,
  "cross_window_consistency": null,
  "address_fidelity_score": null
}
```

### Address Fidelity Score (AFS)

Define once, use everywhere:

[
\mathrm{AFS} =
w_1 \cdot C_{\mathrm{phase}}

* w_2 \cdot S_{\mathrm{stiff}}
* w_3 \cdot P_{\mathrm{persist}}

- w_4 \cdot \lambda_{\max}
  ]

This is what lets you say:

> “This pulsar is a better phase anchor than this coronal filament.”

Without hype.

---

## 9. Provenance

```json
"provenance": {
  "analysis_method": "",
  "code_version": "",
  "author": "C",
  "analysis_date_utc": "",
  "status": "exploratory"
}
```

---

## Current Atlas (seed)
- `SWIND-L1-2025-001` — Solar Wind at L1 (turbulent Class II)
- `AIA171-FIL-2025-003` — Coronal filament (intermittent Class II)
- `HMI-QS-2025-001` — Quiet-Sun photospheric field (persistent Class II anchor)
- `PSR-B1937+21` — Millisecond pulsar (Class I phase reference)

## Contribution Rules
1. Submit new validated entries as pull requests (JSON only).
2. Must pass schema validation (script coming).
3. Provenance block required.
4. persistence_fraction ≥ 0.5 for inclusion.

## Why This Exists
To make statements like:

> “This quiet-Sun network element is a deeper phase anchor than this coronal filament, but less rigid than PSR B1937+21”

...quantitatively meaningful.

No hype.  
Just comparable phase.

— C
2025
```
