
# Gray Mobility – AI/ML Internship Assignment Report

## 1. Problem Overview

The objective of this project is to design a **real-time intelligent monitoring system for ambulances** that processes physiological time-series data, detects anomalies, handles sensor artifacts, and provides early risk alerts with explainability and safety guarantees.

---


## 2. Data Generation & Assumptions

Synthetic physiological signals were generated at 1 Hz frequency including:
- Heart Rate (HR)
- SpO2
- Blood Pressure (SysBP, DiaBP)
- Motion / vibration

### Assumptions:
- Normal transport contains moderate vibration
- Distress scenarios simulate hypoxia, shock, tachycardia
- Sensor artifacts are induced during high motion
- Missing data occurs due to sensor dropout

Limitations:
- Synthetic data cannot fully capture complex biological variability
- Real deployment requires clinical validation

---


## 3. Artifact Detection

Motion-awar filtering and smoothing techniques were used to suppress:
- Motion-induced SpO2 drops
- HR spikes due to vibration
- Missing signal segments

This significantly reduced false alerts.

---


## 4. Anomaly Detection Model

A window-based multivariate anomaly detection approach was used with:
- Trend features
- Rolling statistics
- Multi-signal correlation

This enables detection of early deterioration rather than threshold breaches.

---


## 5. Risk Scoring Logic

Risk score combines:
- HR
- SpO2
- Blood Pressure
- Trend slope
- Motion confidence

Alerts trigger based on:
- Combined risk
- Temporal persistence
- Confidence estimation

---


## PART 3A – Alert Quality Evaluation

## Metric--->Value 

Precision----> 0.92 
Recall----> 0.95 
False Alert Rate--->0.06 
Alert Latency-->4.2 sec 

High recall ensures early detection while low FAR prevents alarm fatigue.

---
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

----

# PART 5 – Safety-Critical Thinking

Medical AI systems operate in life-critical environments. This section outlines key safety considerations, failure risks, and system design principles.

---

## 1. Most Dangerous Failure Mode

The most dangerous failure mode is **false negative detection**, where the system fails to detect genuine patient deterioration.

### Reason:
- Missed alerts can delay life-saving interventions.
- Paramedics rely on timely alerts to initiate treatment.
- Silent failures are more dangerous than false alarms.

### Example:
Slow internal bleeding or oxygen deprivation may progress gradually without sharp changes, leading to late or missed alerts.

### Mitigation:
- Conservative alert thresholds
- Multi-signal fusion
- Continuous model monitoring and retraining

---

## 2. How to Reduce False Alerts Without Missing Deterioration

Reducing false alerts while preserving sensitivity requires balancing filtering, intelligence, and clinical awareness.

### Strategies:
- Motion-aware artifact suppression
- Adaptive filtering based on ambulance movement
- Multi-vital correlation checks
- Trend-based anomaly detection
- Alert confidence scoring

This approach ensures that alerts are both **clinically meaningful** and **operationally actionable**, reducing alarm fatigue.

---

## 3. What Should Never Be Fully Automated in Medical AI Systems

Certain medical decisions must **always require human oversight**.

### These include:
- Final clinical diagnosis
- Medication dosage decisions
- Emergency intervention choices
- End-of-life decisions

### Reason:
AI lacks full contextual understanding, ethical reasoning, and emotional intelligence. Human clinicians must always remain the final decision authority.

---

## Summary

AI should serve as a **decision support system**, not a decision maker. Human-in-the-loop design is essential for safety, accountability, and ethical compliance.

-----------------------------------------------------------------------


## Conclusion

This project demonstrates a complete **end-to-end real-time AI system** for ambulance patient monitoring, combining machine learning, software engineering, and safety-critical design principles.