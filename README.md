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

##### But in real systems, you need to answer:

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
