---
name: carbon-tracing-workflow-skill
description: >-
  Deploy and orchestrate atom-mapped-carbon-tracing from a research question,
  metabolic model, input workbook or existing mapping. Reuse model-setup and
  AI-review skills, run the three mappers and unified carbon propagation, and
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
| Existing mapping/plan | Review and/or propagate as requested; do not rerun mappers |
| One stage only | Complete that stage without expanding the assignment |

For a new analysis use an unused `analyses/<name>/` folder, sharing the project
environment and raw data. Preserve reference scripts, original inputs and prior
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
- `carbon-tracing-ai-review-skill`: handle flagged mapping workbooks and reviewed
  copies, including detailed outcomes and archive persistence.
- `enzymatic-atom-mapping`: scientific analysis delegated by the review skill;
  its repository is named `enzymatic-atom-mapping-skill`. Do not duplicate review.

## Execute the necessary stages

Read [stage handoffs](references/stages.md) before connecting inputs or running
analysis. Use one generated plan and the project's existing commands:

```bash
python3 step2_atom_mapping.py --plan analyses/my_analysis/analysis_plan.json
# For a complete workflow, perform AI review here using its existing skill.
python3 step3_carbon_flow.py --plan analyses/my_analysis/analysis_plan.json
```

For a full analysis, AI review after mapping is enabled by default unless the
user excludes it. Process all flagged targets under the review skill's rules;
do not silently sample or mark omitted targets reviewed. No targets means a
reported no-op, not a fabricated AI copy. Unresolved or partially completed
review must remain explicit, with valid available deliverables identified.

Current step 3 discovers ordinary and `_AIGenerated` mappings and propagates
each independently. Use this unified entry once; do not require/recreate step 4
because an older dependency document mentions it. Preserve raw predictions and
ordinary/AI results. Propagation does not call mappers or an LLM.

Use the installed stage's round/resource/inorganic-carbon settings, recording
actual values and any authorized overrides. The tested default size threshold
is 50,000,000 bytes: save the complete round atomically, then stop that result
at or above the threshold, retaining the round. Record `file_size_limit`, not
convergence. This is not a hard file-size cap or a replacement for memory guards.

## Verify and deliver

Check exit codes, hashes and current reports; old files alone are not success.
Distinguish success, no-op, partial review, errors, convergence and safety stops.
Use the stage run report to identify the actual mapping version and saved rounds;
the aggregate workbook can include historical results. Do not retry an unchanged
failure repeatedly or rerun mapping because a later review/propagation failed.
Reuse compatible mapper/review caches through their validators. Propagation
reruns start at round 1; saved rounds are not resumable checkpoints.

Write a short English `workflow_report.md` following
[reporting](references/reporting.md), linking detailed stage reports and Excel
files rather than adding decorative sheets or another statistics implementation.
Report unfinished work and interface discrepancies explicitly. Interpret coverage
as possible carbon-origin label reachability, not isotope abundance, biological
time, quantitative allocation or experimental confirmation. Do not fit curves.

The skill repository is https://github.com/LGJee/carbon-tracing-workflow-skill.
Ordinary deployment/analysis does not authorize publishing the project, changing
dependent skills or uploading private inputs to prediction services.
