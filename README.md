# Carbon Tracing Workflow Skill

A Codex skill that deploys and orchestrates
[atom-mapped-carbon-tracing](https://github.com/LGJee/atom-mapped-carbon-tracing).
Start from a research question, metabolic model, input Excel workbook or existing
atom mappings. The skill selects the necessary stages, reuses existing tools,
checks their outputs and writes a concise English `reports/workflow_report.md`.

## Install and use

Ask Codex to install this repository with its skill installer:

```text
Use $skill-installer to install https://github.com/LGJee/carbon-tracing-workflow-skill
```

Invoke the installed skill with your project location and request, for example:

```text
Use $carbon-tracing-workflow-skill to deploy the carbon-tracing project and check
its installation. Do not start an analysis yet.
```

```text
Use $carbon-tracing-workflow-skill to compare E. coli supplied with glucose alone
versus simultaneous glucose and xylose. Help me establish the model and conditions.
```

```text
Use $carbon-tracing-workflow-skill to continue my existing analysis from these
mapping workbooks and their analysis plan.
```

The skill can communicate in the user's language. Generated scripts, instructions
and reports use English. Compatible existing installations are reused; missing
dependencies are installed as needed without silently replacing existing ones.

## Workflow and dependent skills

Deployment checks → input preparation → three-tool mapping and selection →
AI-assisted review → carbon propagation → verification and delivery.

Only the requested stages run. Deployment alone does not launch the publication
models, and suitable existing inputs avoid unnecessary upstream calculations.

| Responsibility | Existing capability |
|---|---|
| Select/configure models and export FBA/pFBA inputs | [carbon-tracing-model-setup-skill](https://github.com/LGJee/carbon-tracing-model-setup-skill) |
| Review flagged mappings and deliver reviewed copies | [carbon-tracing-review-skill](https://github.com/LGJee/carbon-tracing-review-skill) |
| Establish evidence-supported enzymatic correspondences | [enzymatic-atom-mapping-skill](https://github.com/LGJee/enzymatic-atom-mapping-skill), installed as `enzymatic-atom-mapping` |
| Predict and propagate mappings | Existing project scripts: LocalMapper, RXNMapper, EnzymeMap and carbon propagation |

AI review is enabled by default for a complete analysis unless excluded by the
user. No review targets means a reported no-op, not an artificial AI copy.
The review skill owns checking, missing-structure completion, affected-reaction
remapping, target refresh, scientific review and validated delivery. Mapper failure
does not stop its scientific analysis. Structure completion may update affected
tables in a staging copy; subsequent correspondence replacements change only
Final_Mapping relative to that copy. The fixed calculation scripts do not call an
LLM themselves. This orchestrator checks the delivery instead of duplicating review.

Manual CSVs retain priority. AI structures and mapping records remain separately
in ai_smiles.csv and ai_atom_mappings.csv, including Source, with detailed immutable
archives. AI data is not promoted into manual tables implicitly. Reuse candidates
through the installed validators and link the review reports in reports/workflow_report.md.

## Compatibility and interpretation

This skill targets the Python/COBRApy project interface with generated analysis
plans and a merged `step3_carbon_flow.py` that discovers ordinary and
`_AIGenerated` mappings. Inspect the installed method settings before running.
The current development interface supports Enumeration, size-1 EMU-inspired
source balances, or both through ANALYSIS_METHODS; older checkouts may support
only Enumeration. Both methods share one result workbook per mapping version:
<model>_3_carbon_flow.xlsx or <model>_3_carbon_flow_AIGenerated.xlsx. No separate
step4 is required; do not invent command-line method switches or require legacy
_Enumeration filenames. Use actual outputs and the current run manifest.
Older checkouts lacking this interface are not certified by the validation below.
An existing newer local development checkout must not be replaced with an older
public revision automatically. This skill release does not publish or upgrade
the main computation project.

The tested platform is Linux under WSL on x86_64. Native Windows, macOS and ARM
execution and a fresh installation were not verified. Inspect current project
settings rather than assuming platform support or silently upgrading dependencies.

The maintained batch-output policy checks actual candidate XLSX size at batch
checkpoints. A candidate reaching 50,000,000 bytes is discarded with its unfinished
round; only preceding complete rounds are published. There is no worksheet
pagination. Memory protection remains separate. Propagation reruns start at round 1.

New cases keep their script, plan, reports and workbooks together under
`results/results_by_AI/<name>/`, sharing model data and environments. Step 3
also collects saved result Summaries into `carbon_flow_summary.xlsx`, one sheet
per ordinary or AI-reviewed workbook. It may include earlier results in that
output directory; use `carbon_flow_run.json` to determine current-run status.

Enumeration describes possible carbon-origin label reachability under the chosen
network, constraints and mappings. EMU gives model-dependent single-carbon mean
source fractions and retains Unknown; it is not a full isotopomer simulator.
Report each method's inorganic-carbon setting separately. Neither method is an
experimental measurement or a physical cultivation-time simulation.

## Validation

Validated against the current local Python development project using the official
skill validator, independent routing evaluations and a small real three-mapper
execution followed by one propagation round. The structured example contained
two reactions and no review targets; no new scientific AI review was claimed.
Separate synthetic checks covered review bookkeeping, ordinary/AI propagation,
provenance checks, resource guards and file-size stops. The 2026-10-17 compatibility update passed skill metadata validation, 19 AI-review
helper tests, 22 Summary/output/memory tests and four project regression scripts.
Full publication models and cross-platform deployment were not rerun for this update.

See [SKILL.md](SKILL.md) for the agent workflow and its linked references for
deployment, stage handoffs and reporting conventions. Project code, models,
environments, caches and private results are not bundled in this repository.


The 2026-10-18 documentation update adopts carbon-tracing-review-skill, delegates
its updated internal workflow, separates manual/AI records, and aligns method
selection, output names and reporting with the installed project interface.
Validation for this update checks skill metadata, local reference links and
published-file consistency; it does not rerun real predictions or research data.

Standalone carbon-tracing-review-skill now runs step 3 after review by default.
This workflow explicitly requests a review-only handoff and owns the subsequent
step-3 execution, so the same analysis is not launched twice.

## AI work cache

Disposable AI workflow/setup/review/reaction files belong in the selected
project's `.cache/ai/`, grouped by stage and run. Final Excel stays in the case
output directory; final readable reports stay in its reports/ subfolder. Original
models, reproducible scripts/plans, adopted chemistry CSVs and scientific evidence
archives remain outside cache. After runs stop, users may delete .cache/ai/;
this drops temporary progress without deleting final results. Existing work is
not automatically relocated, and active caches are never automatically cleared.
