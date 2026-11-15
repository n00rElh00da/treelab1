# TreeLab — ML Experimentation Prototype

TreeLab is a high-fidelity frontend prototype for a multi-dataset machine-learning experimentation environment. It is intentionally a product-design prototype rather than a working ML application. The interface is built with HTML5, CSS3, Vanilla JavaScript, and Plotly.js from a CDN.

## Run locally

From the project directory, start any static server on port 3000. For example:

```bash
python3 -m http.server 3000 --bind 0.0.0.0
```

Then open `http://localhost:3000` in a browser. The Webdev Preview is configured to look for port 3000.

No package installation or build step is required. Plotly.js is loaded from `https://cdn.plot.ly/plotly-2.35.2.min.js`.

## What is included

- Landing page with TreeLab product framing, workflow explanation, and interactive product preview.
- Dashboard with sample metrics, recent activity, quick actions, and Plotly performance chart.
- My Datasets library with Iris Dataset, Breast Cancer Wisconsin Dataset, Wine Dataset, and Customer Churn Dataset.
- Dataset search, task filters, contextual workspace navigation, and session-only simulated CSV upload flow.
- Dataset Explorer with preview table, schema profile, distributions, target distribution, and correlation heatmap.
- Preprocessing Studio with stateful target/features, missing-value handling, encoding, scaling, duplicates, imbalance, and train/test controls.
- Model Laboratory with Decision Tree and Random Forest configuration, simulated progress, and illustrative metrics.
- AutoML Laboratory with Controlled and Extended modes, search budget, validation, estimator selection, and simulated winner state.
- Model Comparison with experiment selection, metric comparison chart, and illustrative confusion matrix.
- Explainability Studio with feature importance, permutation importance, a simplified tree diagram, and sample prediction explorer.
- Experiment History with experiment records and detail modals.

## Explicitly simulated features

The following intentionally require a future backend or real ML implementation:

- CSV parsing, schema inference, data validation, and durable file storage.
- Dataset persistence, experiment persistence, user accounts, authentication, and permissions.
- Actual preprocessing pipelines, feature transformations, train/test splits, and leakage checks.
- Real Decision Tree or Random Forest fitting, scoring, model artifacts, and reproducibility controls.
- FLAML or any other AutoML execution, candidate search, budget enforcement, and validation runs.
- Real confusion matrices, feature importance, permutation importance, prediction probabilities, and explainability calculations.
- Background jobs, progress events, cancellation, model deployment, monitoring, and collaborative review.

All progress, metrics, charts, and experiment records in this prototype are mock values. The UI labels simulated and illustrative results so it does not imply that a real model was trained.
