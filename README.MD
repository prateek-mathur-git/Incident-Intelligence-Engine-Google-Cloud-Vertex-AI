# AI-Powered Incident Intelligence Engine (AIOps Mini Project)

This project is an **AI-powered incident analysis and correlation engine** built on **Google Cloud Vertex AI (Gemini 2.0 Flash)** and **Python**.

It takes raw incident / alert descriptions (synthetic production-like data), then:
- Summarizes each incident using a large language model (LLM)
- Predicts severity and key characteristics
- Clusters similar incidents using classical ML (TF-IDF + K-Means)
- Runs **cluster-level Root Cause Analysis (RCA)** with LLM
- Produces a final JSON report combining all enriched information

This is a **portfolio project** demonstrating how GenAI can augment traditional AIOps / SRE workflows.

---

## 🔍 Problem Statement

In large enterprise environments, operations teams face:
- Huge volume of noisy alerts
- Duplicate incidents across tools
- Slow manual triage and RCA
- High Mean Time To Resolution (MTTR)

Traditional monitoring tools generate alerts, but **they don’t explain what is happening or what to do next**.

**Goal:**  
Build a lightweight **AI Incident Intelligence Engine** that can:
- Read raw incident text
- Enrich it with summaries, severity, and categories
- Correlate related incidents
- Suggest probable root cause and next actions

---

## 🧩 High-Level Architecture

**Input:**  
`incidents_50.txt` — JSON Lines file  
Each line contains:
- `timestamp`
- `incident_text`
- `affected_users`
- `service`
- `baseline_mttr_minutes`

**Pipeline Steps:**

1. **Load incidents**  
   - Read JSONL from local or GCS.
2. **LLM Enrichment (Gemini 2.0 Flash)**  
   - For each incident:
     - Generate summary  
     - Extract keywords / attributes  
     - Predict severity (P1/P2/P3-like)  
3. **Vectorization & Clustering**  
   - Convert incident text to TF-IDF vectors  
   - Cluster using K-Means (simulating correlation groups).
4. **Cluster-level RCA with LLM**  
   - For each cluster, send all related incidents to Gemini:
     - Generate probable root cause  
     - Suggest remediation  
     - Suggest monitoring improvements
5. **Export Final Report**  
   - Combine incident-level and cluster-level outputs  
   - Save to `final_aiops_report.json`

**Output:**  
- Incident-level enriched data  
- Cluster-level RCA + recommendations  
- A single JSON report ready for dashboards or further tooling.

---

## 🛠️ Tech Stack

- **Language:** Python 3  
- **Cloud:** Google Cloud Platform  
- **AI Platform:** Vertex AI – Gemini 2.0 Flash  
- **Libraries:**
  - `google-cloud-aiplatform`
  - `scikit-learn` (TF-IDF + KMeans)
  - `pandas`
  - `json`

---

## 📂 Project Structure (Suggested)

```bash
aiops-incident-intelligence/
├── data/
│   ├── incidents_50.txt          # synthetic incident dataset (JSONL)
│   └── final_aiops_report.json   # generated enriched report
├── notebooks/
│   └── aiops_incident_pipeline.ipynb   # main notebook (Vertex AI / Colab)
├── src/
│   ├── load_data.py
│   ├── llm_enrichment.py
│   ├── clustering.py
│   ├── rca_generation.py
│   └── export_report.py
├── README.md

└── requirements.txt
