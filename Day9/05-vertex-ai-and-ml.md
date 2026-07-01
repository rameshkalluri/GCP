# Vertex AI & ML Intro

## What is Vertex AI?
**Vertex AI** is Google Cloud's **unified machine learning platform** — one place to
**build, train, deploy, and manage** ML models (and use Google's foundation models like
Gemini).

> **Analogy:** A **fully-equipped workshop for building "smart" machines**. Instead of
> gathering tools from everywhere, you get the **whole ML factory** — data prep,
> training rigs, testing benches, and a showroom (deployment) — in one place.

---

## The ML workflow (simple mental model)
```
Data → Train a model → Evaluate → Deploy (endpoint) → Predict → Monitor → (retrain)
```
1. **Data** — collect & prepare (features).
2. **Train** — the model learns patterns from data.
3. **Evaluate** — check accuracy on unseen data.
4. **Deploy** — put it behind an **endpoint** (an API).
5. **Predict** — send new inputs, get predictions (**inference**).
6. **Monitor** — watch for drift; **retrain** as data changes.

> **Analogy:** teaching an **apprentice**: show many examples (train), test them (evaluate),
> put them on the job (deploy), and keep coaching as things change (monitor/retrain).

---

## Ways to do ML on GCP (from easiest to most custom)

| Approach | What it is | Analogy |
|---|---|---|
| **Pre-trained APIs** | Ready models: Vision, Speech, Translation, Natural Language | **Buy a finished appliance** |
| **AutoML (Vertex AI)** | Train custom models with **little code** | **Assemble from a kit** |
| **Custom training (Vertex AI)** | Bring your own code (TensorFlow/PyTorch) | **Build from raw materials** |
| **Foundation models (Gemini)** | Use/tune large generative models | **Hire a versatile expert** |
| **BigQuery ML** | Train models with **SQL** in BigQuery | **Analyst who also predicts** |

---

## Key Vertex AI pieces (glossary)
- **Datasets** — managed training data.
- **Training / Pipelines** — run training jobs (repeatable ML workflows).
- **Model Registry** — versioned store of trained models.
- **Endpoints** — deployed models serving **online predictions** (API).
- **Batch predictions** — score large datasets offline.
- **Feature Store** — reusable, consistent features for training & serving.
- **Model Monitoring** — detects **drift** (data changing over time).

---

## Training vs Inference (don't confuse them)
- **Training** — the model **learns** from data (expensive, done periodically). Often
  uses **GPUs/TPUs** (Day 3).
- **Inference/Prediction** — using the trained model to **answer** new inputs (cheaper,
  frequent).
- *Analogy:* **studying for the exam** (training) vs **answering a question** (inference).

---

## When to use what (quick guide)
- Common task (detect objects, transcribe audio, translate) → **pre-trained API**.
- Custom prediction, limited ML expertise → **AutoML**.
- Full control / special models → **custom training**.
- Generative text/chat/summarization → **Gemini / foundation models**.
- Predict directly from warehouse data → **BigQuery ML**.

---

## TL;DR (in plain English)
- **Vertex AI = one workshop** to build, train, deploy, and monitor ML models (plus
  Google's foundation models).
- ML flow: **data → train → evaluate → deploy (endpoint) → predict → monitor/retrain.**
- Pick effort level: **pre-trained APIs** (easiest) → **AutoML** → **custom training** →
  **foundation models**; or **BigQuery ML** for SQL-based ML.
- **Training** = learning (heavy, GPUs/TPUs); **inference** = answering (light, frequent).
