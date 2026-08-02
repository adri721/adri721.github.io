---
layout: default
title: Probe Localisation
---

# Probe Localisation

### Abstract
This project develops acoustic localization of a piezoelectric receiver relative to a clinical ultrasound probe, using only the time-of-flight of the probe's own imaging pulses — no imaging pipeline, no external tracker at deployment. The motivating application is sensorized needle-tip tracking during ultrasound-guided procedures, where needle tips bend, deviate, and slip out of the imaging plane, and existing tracking solutions add hardware, cost, and workflow friction.

---

### Approach
As the probe sweeps its 210-element aperture, each firing element produces a pulse whose arrival time at the receiver encodes a distance. Across a full frame, the resulting time-of-flight profile forms an acoustic fingerprint of the receiver's position in the probe frame.

**Acquisition.** A custom oscilloscope-based capture system records the probe drive signal and piezo receiver output simultaneously, synchronized with electromagnetic tracking for ground truth in the lab. A 6-axis IMU supplies orientation at deployment, where the electromagnetic tracker is absent.

**Signal processing.** Raw piezo signals are bandpass-filtered and envelope-detected; per-beam times of flight are extracted with anchored candidate tracking, dynamic-programming path selection, and residual filtering. Frame boundaries are detected from geometry-derived timing constraints rather than hardcoded thresholds.

**Localization.** Per-axis gradient-boosted models regress position from engineered TOF-profile features. Neural architectures were systematically evaluated and consistently underperformed at this data scale — the accuracy ceiling proved feature-informational, not architectural.

---

### Key Findings
Same-domain localization reaches centimeter-scale accuracy in both a water bath and a tissue-mimicking phantom. The pivotal negative result: a model trained in water fails structurally when tested through the phantom — it keys on absolute TOF values, which shift with sound speed and geometry across media. This is not a tuning problem but a structural limitation of any physics-blind regressor, motivating the current work on physics-grounded self-calibration and few-shot domain adaptation.

Tracker-free operation via IMU orientation incurs a modest accuracy penalty, concentrated on the axis where 6-axis IMU yaw is fundamentally unobservable.

---

### Technical Contributions
- Built a synchronized acoustic + electromagnetic acquisition system around a clinical curvilinear probe.
- Developed a robust TOF extraction pipeline that eliminated frame-detection failures on the hardest dataset.
- Established same-domain localization in water and tissue-mimicking media with systematic ablations and cross-validation.
- Diagnosed the structural failure mode of cross-domain transfer, reframing the problem toward physics-grounded self-calibration.

### Methods
Acoustic time-of-flight sensing, ultrasound instrumentation, signal processing, gradient-boosted regression, electromagnetic tracking, IMU integration, design of experiments.

### Status
Ongoing (June 2025 – present). Current work: self-calibrating physics-based localization, few-shot domain adaptation, and migration to a streaming acquisition platform for real-time inference.

---

[← Back to Projects](/projects)