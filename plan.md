# TreeLab — Implementation & Design Plan

## Product scope

TreeLab will be delivered as a high-fidelity, frontend-only product prototype. It will use HTML5, CSS3, Vanilla JavaScript, and Plotly.js loaded from a CDN. No backend, persistence, authentication, CSV parsing, or real machine-learning execution will be introduced. Every simulated workflow will be labeled as simulated or illustrative in the UI so the prototype feels credible without making false claims.

The experience will be a single-page application with a public landing view and an in-app shell. In-app navigation will be stateful rather than URL-heavy: the landing page routes into the dashboard, the dashboard opens datasets into a contextual workspace, a

nd the workspace switches between the seven requested labs/sections without full-page reloads.

## Proposed project structure

```
index.html                 # App shell, landing page, in-app layout, modal mount points
styles.css                 # Design system, responsive layout, components, motion
script.js                  # Application state, rendering, interactions, simulation timers
plan.md                    # Approved implementation and design plan
TODO.md                    # Outcome-based acceptance criteria and progress
README.md                  # How to run the prototype and future backend work
public/
  manus-routes.json        # Static route manifest required by the Webdev runtime
```

The implementation will stay intentionally compact: one HTML entry point, one stylesheet, and one JavaScript module split by clear regions for mock data, state, rendering, charts, and event handlers. Plotly charts will render into dedicated containers and re-render from current state when a dataset or comparison selection changes.

## Information architecture and state model

- `view`: `landing`, `dashboard`, `datasets`, `workspace`.

- `workspaceTab`: `overview`, `explorer`, `preprocess`, `models`, `automl`, `compare`, `explain`, `history`.

- `selectedDatasetId`: defaults to Iris Dataset.

- `datasets`: four realistic mock records plus any session-only uploads.

- `experiments`: realistic predefined records plus new simulated training/AutoML results.

- `preprocessing`: target, features, missing-value strategy, encoding, scaling, duplicates, imbalance, and split.

- `modelConfig`: decision-tree and random-forest controls.

- `comparisonSelection`: experiment IDs selected for comparison.

- `notifications`, `activeModal`, `uploadState`, `trainingState`, and `automlState` for transient UI feedback.

The state will remain in memory across navigation so that changing controls updates dependent panels, charts, summaries, and notifications consistently during the session.

## Design direction

### Alternatives considered

1. **Scientific command center** — 0.07 probability: dense observatory-like tooling with glowing telemetry accents.

1. **Quiet data atelier** — 0.04 probability: editorial, pale surfaces, and more exploratory whitespace.

1. **Research-grade midnight console** — 0.06 probability: restrained navy instrumentation, crisp technical typography, and a compact lab rhythm.

### Chosen movement: research-grade midnight console

TreeLab should feel like a lab instrument rather than a generic AI dashboard: precise, calm, and visibly engineered.

- **Core principles:** evidence before decoration; progressive disclosure for complex settings; persistent context; motion only when it communicates state.

- **Color philosophy:** deep navy keeps attention on the data; electric blue and cyan signal active exploration; violet is reserved for AutoML; emerald indicates completed or healthy experiments; amber highlights simulated or cautionary states.

- **Layout paradigm:** a persistent left rail anchors the workflow while the main canvas uses asymmetrical split panes, broad chart surfaces, and horizontal “lab benches” instead of a repetitive equal-card grid.

- **Signature elements:** a small “signal rail” on key cards, thin gridline motifs behind charts, and compact monospace metadata labels that make the product feel instrumented.

- **Interaction philosophy:** every action should yield immediate feedback—selection changes update context, long-running actions show staged progress, and complex controls expose concise helper copy rather than dumping all detail at once.

- **Animation:** use 150–220ms easing for page/section transitions, a soft shimmer only for active simulated runs, number count-up on dashboard load, and restrained hover elevation. Avoid looping decorative motion.

- **Typography system:** Inter for UI and body copy, Manrope for prominent section titles, JetBrains Mono for metrics, dataset metadata, status labels, and code-like values. Use a high-contrast hierarchy with short line lengths.

- **Brand essence:** TreeLab is the visual lab for learners and researchers who want to understand the full ML workflow before productionizing it. Personality: rigorous, inviting, observant.

- **Brand voice:** direct, technically literate, and encouraging. Example lines: “Start with a dataset. End with evidence.” and “Results are illustrative until you connect a real training backend.”

- **Wordmark & logo:** a compact branching-tree mark made from three connected nodes, paired with a wordmark where the “T” has a split-stem motif. The mark will be drawn with inline SVG/CSS rather than a stock icon.

- **Signature brand color:** `#38BDF8` (signal cyan), used sparingly as the ownable TreeLab action color against navy.

## Page and feature implementation

### Landing page

Build a premium hero with TreeLab identity, a compact product preview showing a dataset explorer and experiment status, a four-step workflow ribbon, feature highlights, and a CTA into the dashboard. The page will set the product promise and explicitly call the experience a prototype.

### Dashboard

Use a left navigation rail and a top context bar. Show four metric blocks, a recent activity feed, recent datasets, an experiment performance overview chart, and quick actions. Quick actions open the upload modal, dataset library, model lab, or AutoML lab in the active workspace.

### My Datasets

Provide search, a dataset-type filter, dataset rows/cards with metadata, sample count, feature count, target, and freshness. The upload modal will support drag/drop styling, file selection, simulated staged progress, and a completion notification. Uploaded files become session-only mock datasets with generated metadata and a safe “prototype only” notice.

### Dataset workspace

The workspace keeps the selected dataset name and metadata visible while a secondary workspace tab bar switches between Overview, Data Explorer, Preprocessing Studio, Model Laboratory, AutoML Laboratory, Model Comparison, Explainability Studio, and Experiment History. Each dataset can be selected from the library and opens the same functional surface with dataset-specific mock values.

### Explorer, preprocessing, and model labs

- **Data Explorer:** preview table, schema/type chips, missing-value summary, descriptive statistics, target distribution, feature distributions, correlation heatmap, and Plotly charts.

- **Preprocessing Studio:** compact form controls for every requested preprocessing setting, a visual pipeline strip, and helper text. State changes update the pipeline and configuration summary immediately.

- **Model Laboratory:** tabbed Decision Tree and Random Forest configuration panels, realistic defaults, sliders/selectors, a “Train model” button, progress phases, a simulated result summary, and metric cards/charts. The result panel always carries a simulated badge.

### AutoML, comparison, explainability, history

- **AutoML Laboratory:** Controlled and Extended modes, time budget, optimization metric, validation strategy, estimator selection, progress timeline, best estimator, and best configuration. Violet accents distinguish it from manual training.

- **Model Comparison:** multi-select experiment list, metric comparison bars/radar-like summary, training duration, confusion matrices, and a responsive table. At least two selected experiments are shown by default.

- **Explainability Studio:** feature and permutation importance bars, a simplified decision tree diagram, prediction probability bars, and a sample-prediction explorer with illustrative labels.

- **Experiment History:** sortable-looking table, statuses, dataset/model association, metrics, dates, durations, and detail modal. Use empty/loading states for filters and interactions where appropriate.

## Simulation rules

- Upload: progress advances through “Reading file”, “Profiling columns”, and “Ready for review” with a short deterministic timer.

- Training: progress advances through “Preparing split”, “Fitting estimator”, “Scoring holdout”, and “Complete”; predefined metrics are selected from mock result templates and annotated as simulated.

- AutoML: progress advances through “Searching candidates”, “Validating trials”, and “Selecting winner”; the best estimator comes from a predefined record.

- Notifications: use a top-right toast stack with dismiss actions and accessible live-region text.

- Loading/empty states: show during transitions, filtered zero-result lists, and when comparison selections are manually cleared.

## Frontend and serving workflow

1. Create `index.html`, `styles.css`, `script.js`, `README.md`, `TODO.md`, and `public/manus-routes.json` in `/home/ubuntu/treelab`.

1. Register host-managed diagnostics for HTML, CSS, JavaScript, and JSON through `webdev.config`.

1. Start a minimal static development server on the configured port 3000, bound to `0.0.0.0`, and verify `GET /manus-routes.json` returns JSON rather than SPA fallback HTML.

1. Validate the source with syntax checks plus focused interaction inspection. Use Webdev screenshot capture only where visual refinement benefits from rendered evidence.

1. Set static build configuration to the committed project root (`command: "true"`, `outputDirectory: "."`) if publication is requested later; do not publish automatically.

1. Commit the finished prototype to the managed canonical Git remote so the project checkpoint is recoverable. Publication remains separate and is not enabled by this plan.

## Explicitly simulated / future backend work

Real CSV ingestion, schema inference, persistent datasets, user accounts, database-backed experiments, real preprocessing pipelines, real Decision Tree and Random Forest training, FLAML/AutoML execution, model serialization, background jobs, real explainability calculations, true comparison metrics, experiment versioning, production deployment, and secure multi-user permissions are intentionally out of scope and will be documented in the README.

