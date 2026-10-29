# ML Fundamentals & MLOps

Engineers who have built LLM applications are now interviewing for roles that also own classical models, fine-tuning and model serving. Interviewers check that you know the basics well enough not to make beginner mistakes, and that your platform experience carries over to ML pipelines. Agentic-AI Architect, ML platform Staff/Principal and Forward Deployed Engineer roles ask most.

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [Cloud, DevOps & integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Supervised vs. unsupervised vs. self-supervised learning — give one real example of each.
- [ ] Classification vs. regression; which loss function is typical for each?
- [ ] Training, validation and test sets — why three, and what is data leakage?
- [ ] Overfitting vs. underfitting; how do regularisation, early stopping and more data help?
- [ ] Bias–variance trade-off in plain English.
- [ ] Precision, recall, F1, ROC-AUC, PR-AUC — which would you use for a 1%-positive fraud dataset and why?
- [ ] What is gradient descent, and what do the learning rate and batch size control?
- [ ] What is an embedding in ML terms, and how are text embedding models trained (contrastive learning)?

## B. Working depth
- [ ] Logistic regression, decision trees, gradient boosting, neural networks — when is a simple model the right answer?
- [ ] K-means and clustering — how do you pick k, and how would you use clustering on support tickets?
- [ ] Cross-validation: when is a random split wrong (time series, grouped users)?
- [ ] How do you handle class imbalance (re-weighting, resampling, threshold tuning)?
- [ ] In PyTorch, what are tensors, `nn.Module`, autograd, the optimiser step and `model.eval()`?
- [ ] Write out a minimal PyTorch training loop and name the common bugs (forgetting `zero_grad`, no `eval` mode, device mismatch).
- [ ] Fine-tuning vs. RAG vs. prompting — what problem does each solve?
- [ ] Full fine-tuning vs. LoRA/QLoRA — memory, cost and when parameter-efficient methods are enough *(verify)*.
- [ ] What is a feature store, and what is training/serving skew?
- [ ] Batch vs. online inference; what changes in the serving stack?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Data drift vs. concept drift vs. label drift — how do you detect each in production?
- [ ] How do you monitor a model when ground-truth labels arrive weeks later?
- [ ] Model serving options (TorchServe, Triton, vLLM, KServe, SageMaker/Vertex endpoints) — how do you choose? *(verify)*
- [ ] GPU serving cost: batching, quantisation, autoscaling to zero, and cold starts.
- [ ] How do you version data, code and models together so a result can be reproduced?
- [ ] What does a model registry and promotion process look like (staging, shadow, canary, rollback)?
- [ ] When would you fine-tune a small model to replace an LLM call, and how do you prove it is good enough?
- [ ] How do you keep PII and consent rules intact across training data, features and logs?
- [ ] How do your backend/platform skills (CI/CD, observability, migrations) map onto MLOps, and where are the real gaps?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a churn-prediction pipeline for a subscription billing system: features, labels, training cadence, serving.
- [ ] Design an MLOps pipeline from raw data to a deployed model with automated retraining and approval gates.
- [ ] An LLM-based ticket classifier costs too much. Plan a move to a fine-tuned small model without losing accuracy.
- [ ] A fraud model's precision dropped 20% last month. Walk through the investigation.
- [ ] Design real-time feature computation for a recommendation model with a 50 ms budget.

## E. Hands-on checks (things to be able to DO)
- [ ] Train and evaluate a scikit-learn gradient boosting classifier with a proper split, cross-validation and a PR curve.
- [ ] Write a PyTorch training loop for a small text classifier on top of frozen embeddings.
- [ ] Serve a model behind a FastAPI or Node.js endpoint in Docker with health checks and latency metrics.
- [ ] Add a drift check (e.g. population stability index on key features) that runs daily and alerts.

---

**Good answers show:**
- Correct choice of metric for the business cost of errors, not accuracy by default.
- Instinctive checks for leakage, imbalance and wrong data splits.
- Willingness to start with a simple baseline and justify anything more complex.
- Treating models as deployable artefacts with versioning, monitoring and rollback, like any service.
- Honest framing of what is production experience and what is learning, with a plan to close gaps.

**Prove it:** Build an end-to-end mini pipeline: train a small classifier (e.g. ticket routing) with scikit-learn or PyTorch, track runs with MLflow, serve it in Docker, log predictions, and add a daily drift report; compare its cost and accuracy with an LLM doing the same task.
