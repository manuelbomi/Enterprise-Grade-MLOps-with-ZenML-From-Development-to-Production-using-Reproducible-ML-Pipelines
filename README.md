# Enterprise-Grade MLOps with ZenML : <sub>from Development to Production using Reproducible ML Pipelines</sub>

### Why this repository exists

##### Most machine learning projects fail in production, not because the model is bad, but because:

- Training code is not reproducible

- Experiments are not tracked properly

- Data lineage is unclear

- Dev and prod environments behave differently

- Deployment is manual and fragile

##### This repository demonstrates how to build a real, enterprise-ready MLOps system using ZenML, with:

- Clean pipelines

- Experiment tracking

- Dev → Prod parity

- CI/CD integration

- Infrastructure abstraction

---
<ins>One-sentence definition</ins>

*ZenML is an MLOps framework that turns ML code into versioned, reproducible, deployable pipelines that can run consistently across development and production environments.*

---

### The MLOps Problem (Before ZenML)

##### Typical ML code:

```python
data = load_data()
model = train(data)
accuracy = evaluate(model)
save(model)
```

This works once, on your laptop, for experiments.

##### But in real systems, we need to answer:

- Which exact data produced this model?

- Can I reproduce this run next month?

- Can this run on Kubernetes instead of my laptop?

- How do I deploy only if metrics pass?

- How does CI/CD retrain and redeploy models?

**These problems define MLOps.**

---

### Core ZenML Concepts
#### <ins>1. Step</ins>

##### A step is a single logical unit of work.

Examples:

- Load data

- Clean data

- Train model

- Evaluate model

- Deploy model

from zenml import step

```python
@step
def load_data():
    return df
```

---

#### <ins>2. Pipeline</ins>

A pipeline connects steps into a reproducible workflow.

```python
from zenml import pipeline

@pipeline
def training_pipeline():
    data = load_data()
    clean_data = preprocess(data)
    model = train_model(clean_data)
    metrics = evaluate_model(model, clean_data)

```

##### Running the pipeline:

```python
training_pipeline()
```

---

###  What Makes ZenML an MLOps Tool?
##### <ins>1. Reproducibility</ins>

##### Every pipeline run tracks:

- Code version

- Parameters

- Inputs & outputs

- Artifacts (models, datasets, metrics)

We can always answer:

**“How was this model produced?"**

##### <ins>2. Artifact Tracking</ins>

##### Outputs are not just Python variables — they are versioned artifacts.

Examples:

- Dataset artifact

- Model artifact

- Metrics artifact

Instead of:

```python
model.pkl

```

We get:

```python
Model v17 — Pipeline run 2024-10-12_14-32
```

##### <ins>3. Infrastructure Abstraction</ins>

##### Our pipeline code never changes, but where it runs does.

```python
zenml stack set local
zenml stack set production
```

With ZenML, the same pipeline can run on:

- Local machine

- Docker

- Kubernetes

- Cloud (AWS, GCP, Azure)

##### <ins>4.  Clear Separation of Concerns </ins>

#### MLOps Team Responsibilities

| Responsibility | Owner | Key Activities |
|----------------|-------|----------------|
| **Model logic** | ML Engineer | • Algorithm development<br>• Model training & optimization<br>• Performance tuning |
| **Data handling** | Data Engineer | • Data pipeline development<br>• Feature engineering<br>• Data quality assurance |
| **Infrastructure** | Platform / DevOps Engineer | • Cloud infrastructure<br>• CI/CD pipelines<br>• Monitoring & scaling |
| **Experiment tracking** | MLOps Engineer | • Experiment versioning<br>• Model registry management<br>• Performance tracking |


#### MLOps Responsibility Matrix (RACI)

| Responsibility | R (Responsible) | A (Accountable) | C (Consulted) | I (Informed) |
|----------------|-----------------|-----------------|---------------|--------------|
| **Model Development** | ML Engineer | Lead ML Engineer | Data Scientist | Product Manager |
| **Data Pipeline** | Data Engineer | Lead Data Engineer | ML Engineer | MLOps Engineer |
| **Infrastructure** | DevOps Engineer | Platform Lead | Security Team | All Engineering Teams |
| **Experiment Tracking** | MLOps Engineer | ML Lead | Data Scientist | Stakeholders |
| **Model Deployment** | MLOps Engineer | DevOps Lead | ML Engineer | Business Teams |
| **Monitoring & Maintenance** | MLOps Engineer | Platform Lead | ML Engineer | Support Teams |






##### ZenML enforces this separation by design.

---

### ZenML vs MLflow


- MLflow = experiment tracking

- ZenML = full MLOps framework

- ZenML can use MLflow internally

MLflow example (manual tracking)

```python
import mlflow

with mlflow.start_run():
    mlflow.log_param("lr", 0.01)
    mlflow.log_metric("accuracy", 0.92)
    mlflow.sklearn.log_model(model, "model")
```

##### MLflow answers:

*“Which experiment performed best?”*

##### ZenML answers bigger questions:

- How was the model trained?

- Which data produced it?

- Can I reproduce it?

- Can I deploy it automatically?

- Can CI/CD run it?

#### Comparison Table

#### MLOps Platform Comparison: MLflow vs ZenML

| Feature | MLflow | ZenML | Notes |
|---------|--------|-------|-------|
| **Experiment tracking** | ✅ Native | ✅ (via integration) | Both support experiment tracking, but MLflow has more mature native capabilities |
| **Pipeline orchestration** | ❌ Not supported | ✅ Native | ZenML has built-in pipeline orchestration; MLflow requires external tools |
| **Data lineage** | ❌ Limited | ✅ Native | ZenML tracks data artifacts and transformations natively |
| **Reproducibility** | ⚠️ Partial | ✅ Strong | ZenML's containerized steps ensure better reproducibility |
| **Infrastructure abstraction** | ❌ None | ✅ Native | ZenML abstracts infrastructure; MLflow requires manual setup |
| **CI/CD support** | ❌ Basic | ✅ Native | ZenML designed for CI/CD pipelines from the start |
| **Production workflows** | ⚠️ Weak | ✅ Strong | ZenML focuses on production ML; MLflow more experimental |

#### MLOps Platform Feature Comparison

| Feature | MLflow | ZenML | Winner | Importance |
|---------|--------|-------|--------|------------|
| **Experiment Tracking** | ⭐⭐⭐⭐⭐ (5/5) | ⭐⭐⭐⭐ (4/5) | MLflow | High |
| **Pipeline Orchestration** | ⭐ (1/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | High |
| **Data Lineage** | ⭐⭐ (2/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | Critical |
| **Reproducibility** | ⭐⭐⭐ (3/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | High |
| **Infrastructure Abstraction** | ⭐ (1/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | Medium |
| **CI/CD Support** | ⭐⭐ (2/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | High |
| **Production Workflows** | ⭐⭐ (2/5) | ⭐⭐⭐⭐⭐ (5/5) | ZenML | Critical |
| **Total Score** | **16/35** | **34/35** | **ZenML** | |

**Key Insights:**
- MLflow wins for **experimentation and research**
- ZenML wins for **production and scalability**
- For McDonald's scale, **ZenML is recommended** for production
- Consider **hybrid approach** using both platforms

