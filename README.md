# Gray Smart Ambulance – AI/ML Internship Assignment

This project implements a **real-time intelligent patient monitoring system for ambulances** using **time-series machine learning, anomaly detection, explainable AI, drift detection, and a live dashboard**.

The system continuously monitors patient vitals, detects early warning signals, handles motion artifacts, assigns risk scores, and exposes predictions via an API and real-time dashboard.

---

## Key Features

- Synthetic realistic time-series data generation
- Motion artifact detection & correction
- Multivariate anomaly detection
- Risk scoring & triage logic
- Explainable AI (feature contribution)
- Drift detection for sensor reliability
- Live FastAPI inference service
- Real-time monitoring dashboard (Streamlit)
- Full evaluation metrics & failure analysis

---

## 🏗 Project Architecture

Vitals Sensors → Artifact Handling → Anomaly Model → Risk Scoring
↓
Explainability + Drift Detection
↓
FastAPI Service → Live Dashboard


---

## 📂 Folder Structure

gray-smart-ambulance-ai/
│
├── api/ # FastAPI inference service
├── dashboard/ # Live monitoring dashboard (Streamlit)
├── data/
│ ├── raw/
│ └── processed/
├── notebooks/ # EDA + visualization notebooks
├── src/ # Core ML modules
├── report.md # Technical & safety report
├── README.md # Project documentation
├── requirements.txt # Dependencies


## Data Assumptions & Simulation Logic

 We generated synthetic physiological time-series data based on clinically realistic assumptions.

### Sampling Rate
Vitals are sampled at 1Hz (one sample per second), reflecting real-world ambulance monitoring systems.

### Physiological Ranges
- Heart Rate (HR): 60–100 bpm
- SpO₂: 95–100%
- Systolic BP: 110–130 mmHg
- Diastolic BP: 70–85 mmHg
- Motion: 0.05–0.3 (normalized scale)

### Distress Scenarios
We simulated three deterioration patterns:
1. Hypoxia – progressive SpO₂ drop with HR rise
2. Hemodynamic shock – falling BP with compensatory tachycardia
3. Cardiac stress – sharp HR escalation with BP instability

### Motion Artifacts
Ambulance movement induces:
- Spurious HR spikes
- Transient SpO₂ drops
- Blood pressure jitter

These artifacts are explicitly filtered before anomaly detection.

### Missing Data
Short sensor dropouts (1–5 seconds) are simulated to mimic telemetry packet loss and sensor displacement.

### Limitations
The data does not capture all physiological variability, patient demographics, or comorbidities and should be considered representative rather than clinically exact.


# Running the system

----> FastAPI backend:
uvicorn api.app:app --reload
or
python -m uvicorn api.app:app --reload

## api avaliable at:
http://127.0.0.1:8000/docs


---->start frontend dashboard
streamlit run dashboard/app.py

## dashboard at:
http://localhost:8501


## FastAPI backend:
--> open http://127.0.0.1:8000/docs

-->POST /predict

--> click on : try it


## add sample data:
{
  "HR": 130,
  "SpO2": 88,
  "SysBP": 85,
  "DiaBP": 55,
  "Motion": 0.6
}

--> click on : evaluate

## sample response:

{
  "anomaly": 1,
  "risk_score": 90,
  "confidence": 0.91,
  "explanation": {...},
  "drift": {...},
  "evaluation": {...}
}




