# Task 3B – Failure Analysis

This section analyzes three critical failure modes observed during system evaluation. Since ambulance environments involve noisy sensors and high uncertainty, understanding these failures is essential for improving patient safety and alert reliability.

---

## Failure Case 1 – Motion Artifacts Causing False Alerts

### Scenario
During ambulance movement over speed bumps or rough roads, sudden spikes were observed in heart rate and temporary drops in SpO₂.

### What Failed
The anomaly detection model incorrectly classified these motion-induced fluctuations as physiological deterioration, triggering false alerts.

### Root Cause
The motion signal caused transient disturbances in multiple sensors, but the anomaly model treated them as true physiological changes.

### Impact
False alerts increase alarm fatigue for paramedics and may lead to reduced trust in the system, potentially causing real alerts to be ignored.

### Improvement Strategy
- Stronger motion artifact suppression filters
- Adaptive smoothing windows
- Context-aware filtering using the motion channel to suppress transient anomalies

---

## Failure Case 2 – Slow Physiological Deterioration Causing Delayed Alerts

### Scenario
In cases of gradual hypoxia or slow blood pressure decline, vitals drifted slowly over time without sharp changes.

### What Failed
The anomaly detector raised alerts later than desired due to reliance on short-term deviations.

### Root Cause
Window-based statistical detection struggled to capture slow temporal trends effectively.

### Impact
Delayed alerts reduce response time, potentially delaying critical interventions.

### Improvement Strategy
- Trend-sensitive detection models
- Time-series forecasting methods
- Multi-scale temporal windows to capture both rapid and slow changes

---

## Failure Case 3 – SpO₂ Sensor Drop Leading to False High Risk

### Scenario
Poor sensor contact resulted in transient SpO₂ drops unrelated to patient condition.

### What Failed
The risk scoring logic over-weighted SpO₂, leading to false high-risk scores.

### Root Cause
Single-sensor dominance in risk calculation without sufficient cross-signal validation.

### Impact
Unnecessary emergency escalation and operational inefficiency.

### Improvement Strategy
- Multi-signal validation logic
- Confidence-based risk weighting
- Sensor reliability scoring

---

## Summary

These failures highlight the challenges of deploying ML models in safety-critical ambulance environments. Robust artifact suppression, temporal modeling, and cross-sensor validation are essential to minimize false alarms while preserving sensitivity to true deterioration.
