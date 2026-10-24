# Carbon Tracing Workflow Skill

A Codex skill that deploys and orchestrates
[atom-mapped-carbon-tracing](https://github.com/LGJee/atom-mapped-carbon-tracing).
Start from a research question, metabolic model, input Excel workbook or existing
atom mappings. The skill selects the necessary stages, reuses existing tools,
checks their outputs and writes a concise English `Ai_reports/workflow_report.md`.

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
through the installed validators and link the review reports in Ai_reports/workflow_report.md.

## Compatibility and interpretation

The current target is the Python/COBRApy project with version-3 generated plans
and an EMU-only step3. Inspect the actual installed interface before execution;
a newer local checkout must not be replaced with an older public one. This skill
does not itself publish or upgrade the computation project.

Step3 independently reads ordinary and available _AIGenerated mappings, producing
<model>_3_carbon_flow.xlsx and <model>_3_carbon_flow_AIGenerated.xlsx. New files
contain only EMU_Metabolites and EMU_BoundaryInputs. There is no enumeration,
round selector, step4, Summary or summary collector. Historical files remain
historical, not current success evidence.

The size-1 EMU-inspired solver calculates fixed-flux mean origin fractions and
retains Unknown, not full isotopomer distributions or measured contributions.
Record recycling (current default True), tolerance and numerical checks. Preserve
unknown regions of partial structures and external imports; more complete maps
or larger resolved fractions do not establish higher accuracy.

Shared mapper/model scheduling and memory/disk guards remain. Atomic size-limited
exports reject oversized candidates without replacing existing files. Resource
or size failures return nonzero; emu_solved is distinct from emu_completed. Verify
the current manifest's mapping hashes and output files, not stale workbooks.
See references/stages.md and references/reporting.md for the actual handoff.

New cases remain under results/results_by_AI/<name>/ with shared data/environments,
per-model reviewed reports under Ai_reports/ and disposable work under .cache/ai/.

## Validation

The EMU-only compatibility update checks metadata, references and a bounded
deterministic stage handoff against the selected local project. The update passed 19 project handoff/resource/scheduler tests and metadata
validation; integrated companion checks passed 34 review, 11 structure-completion,
14 scientific-validator and five FBA/pFBA preparation tests. It does not establish full
biochemical correctness, a fresh installation or cross-platform support.
Previous combined-method validation belongs to earlier Git commits and does not
certify the current interface.

See [SKILL.md](SKILL.md) for the agent workflow and its linked references for
deployment, stage handoffs and reporting conventions. Project code, models,
environments, caches and private results are not bundled in this repository.

Validation on 2026-10-24 used the current local EMU-only development interface.
Official skill metadata checks passed. Tests were isolated and did not modify
reference plans, model files or existing formal results. This does not certify
biochemical accuracy, a fresh deployment, older public project versions or
cross-platform execution. The computation project is not published by this skill
maintenance task.
