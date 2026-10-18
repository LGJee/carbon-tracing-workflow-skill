---
name: carbon-tracing-workflow-skill
description: >-
  Deploy and orchestrate atom-mapped-carbon-tracing from a research question,
  metabolic model, input workbook or existing mapping. Reuse model-setup and
  review skills, run the three mappers and supported carbon-origin analyses, and
  deliver verified outputs and a concise workflow report. Use for project-level
  setup or multi-stage analysis, not standalone biochemical reaction review.
---

# Carbon Tracing Workflow Skill

Coordinate existing capabilities; do not implement another modeling, mapping,
review or propagation engine. Talk in the user's language and write generated
scripts, reports and runtime messages in English.

## Route the request

Inspect supplied files and prior instructions before asking questions. Choose
the earliest necessary stage, not an automatic restart:

| Request or available material | Action |
|---|---|
| Deploy/check only | Provision and smoke-test; do not run the publication models |
| Publication reproduction | Use the selected project's reference conditions |
| Research question/model | Delegate input preparation to the model-setup skill |
| Valid step-1 Excel/plan | Validate/register it, then map if requested |
| Existing mapping/plan | Review and/or propagate as requested; remap only affected reactions when missing structures are resolved |
| One stage only | Complete that stage without expanding the assignment |

For a new analysis use an unused `results/results_by_AI/<name>/` folder, sharing the project
environment and raw data. Keep the case script, plan, reports and workbooks
directly in that folder, without another results subfolder. The folder name
identifies an AI-organized case, not AI-generated mappings; only the existing
_AIGenerated suffix and workbook flags identify mapping provenance.
Preserve reference scripts, original inputs and prior
completed outputs. An authorized rerun can replace that analysis's own generated
outputs; check whether existing AI deliveries need separate versioned storage.
Do not repeat scientific questions already answered or assume the publication's
IUPD, half-growth or squalene conditions for other models.

## Locate and provision

Locate the actual selected checkout and accessible Linux/WSL/server. Inspect its
README, `docs/interfaces.md`, setup scripts and stage entries before execution.
A local development copy may be ahead of the public repository; do not replace,
reset or upgrade it automatically. Do not hardcode usernames or distribution names.
Read [deployment](references/deployment.md) when locating/installing/checking
dependencies. Install missing dependencies needed by the requested task and reuse
compatible installations. Incompatible existing installs need a concrete proposed
resolution, not silent replacement. Report inaccessible hosts or unsupported platforms.

Invoke these skills only when their stage is needed; read their installed
instructions and relevant references, not copies of their implementations:

- `carbon-tracing-model-setup-skill`: choose/configure/modify models and export
  validated FBA/pFBA inputs. Skip when suitable inputs already exist.
- `carbon-tracing-review-skill`: handle flagged or unmapped reactions in mapping workbooks and reviewed
  copies, including evidence-backed missing-structure completion, affected-reaction remapping, detailed outcomes and archive persistence.
- `enzymatic-atom-mapping`: scientific analysis delegated by the review skill;
  its repository is named `enzymatic-atom-mapping-skill`. Do not duplicate review.

## Execute the necessary stages

Read [stage handoffs](references/stages.md) before connecting inputs or running
analysis. Use one generated plan and the project's existing commands:

```bash
python3 step2_atom_mapping.py --plan results/results_by_AI/my_analysis/analysis_plan.json
# For a complete workflow, perform AI review here using its existing skill.
python3 step3_carbon_flow.py --plan results/results_by_AI/my_analysis/analysis_plan.json
```

For a full analysis, scientific review after mapping is enabled by default
unless excluded by the user. Invoke carbon-tracing-review-skill and let it own
input checking, missing-structure completion, affected-reaction remapping,
refreshed target extraction, scientific review and validated copy delivery.
Explicitly request a review-only handoff: this orchestrator owns the subsequent
step-3 run. Standalone review defaults to running step 3 itself.
Do not duplicate its internal pipeline or select only successful mapper outputs.
Mapper failures still require a scientific attempt under that skill's rules.
No targets is a reported no-op, not a fabricated AI copy. Report unresolved,
partial or blocked review and identify available valid deliverables.

Inspect the installed step3 settings before running. The current development
interface supports Enumeration, size-1 EMU-inspired source balances, or both
through ANALYSIS_METHODS at the script top; older compatible versions may support
only Enumeration. Run selected supported methods without silently adding/removing
methods or inventing CLI switches. Both methods share one output per mapping
version: <model>_3_carbon_flow.xlsx or <model>_3_carbon_flow_AIGenerated.xlsx.
Either mapping version may exist alone. No separate step4 is required. Step3
calls neither mappers nor an LLM. Preserve older outputs and use actual filenames
and the current run manifest rather than hardcoded legacy suffixes.
Record inorganic-carbon assumptions separately by method. Current development
defaults terminate inorganic-carbon labels for Enumeration and allow recycling
for EMU; inspect actual settings. These choices do not establish biological CO2 loss.

Use the installed stage's round/resource/inorganic-carbon settings, recording
actual values and any authorized overrides. For Enumeration, the tested default size threshold
is 50,000,000 bytes. Use the installed batch-output policy: stage records on
disk, measure an actual candidate XLSX at batch/layer checkpoints, and discard
an unfinished round reaching the threshold. Publish only complete rounds below
the limit. Do not reintroduce pagination; row overflow stops with `row_limit`.
Record `file_size_limit` or `row_limit`, never convergence. A zero-round stop
does not make an older workbook a current result. Temporary files may overshoot
between checks; required label states still consume memory, so guards remain.

## Verify and deliver

Check exit codes, hashes and current reports; old files alone are not success.
Distinguish success, no-op, partial review, errors, convergence and safety stops.
Use carbon_flow_run.json to identify the actual mapping version and saved rounds.
Step 3 also collects saved Enumeration Summary tables in `carbon_flow_summary.xlsx`,
one sheet per normal or AI-reviewed workbook in the output directory. This can
include older files; use the run manifest to identify current outputs. Do not retry an unchanged
failure repeatedly or restart all mapping because a later review/propagation failed. Missing-structure completion through the review skill may remap affected ready reactions using its documented handoff.
Reuse compatible mapper/review caches through their validators. Propagation
reruns start at round 1; saved rounds are not resumable checkpoints.

Write a short English `workflow_report.md` following
[reporting](references/reporting.md), linking detailed stage reports and Excel
files rather than adding decorative sheets or another statistics implementation.
Report unfinished work and interface discrepancies explicitly. Interpret
Enumeration as possible source-label reachability, not quantitative source
fractions, experimental isotope abundance, physical time or confirmed allocation.
Read carbon_flow_run.json and the selected methods' saved tables. Enumeration
uses Summary; EMU-only outputs have no Enumeration Summary. Report EMU source
and Unknown fractions, numerical checks and assumptions separately from possible
label reachability. EMU is not a full isotopomer simulator. Do not fit curves.

The skill repository is https://github.com/LGJee/carbon-tracing-workflow-skill.
Ordinary deployment/analysis does not authorize publishing the project, changing
dependent skills or uploading private inputs to prediction services.
