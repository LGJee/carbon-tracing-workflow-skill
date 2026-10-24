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

## Project context and responsibility

Atom-mapped-carbon-tracing connects biological questions to carbon-source analysis:
define culture conditions and obtain a flux solution, prepare molecular structures
and atom mappings, review uncertain correspondences, then calculate carbon
propagation or other methods supported by the selected project. The publication's
non-IUPD/IUPD cases demonstrate this workflow without defining universal conditions.

This skill owns stage selection, deployment, handoffs and verified delivery.
Model setup and scientific reaction review belong to their specialized skills;
numerical stages run through existing project code. Keep their meanings separate:
flux feasibility, mapping coverage, chemical evidence and carbon-source results
answer different questions. The current project runs size-1 EMU-inspired balances;
other method outputs require their own stated assumptions. Neither software
completion nor AI involvement establishes an experimental measurement.

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
environment and raw data. Keep the case script, plan and workbooks directly in
that folder, final readable reports under Ai_reports/, and process files in the AI
cache. Do not create another results subfolder. The folder name
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

## Disposable AI work and final files

Use `<project>/.cache/ai/<stage>/<run>/` for disposable AI process files:
research downloads, extracted inputs, proposals, staging workbooks, retry reports,
logs, validation output and continuation JSON. Stages may be workflow, setup,
review, structures or enzymatic-mapping. Reuse the orchestrator's supplied run
folder when available. Do not put these process files in the results root.

Keep final workbooks in the selected case output directory and readable final
reports under its `Ai_reports/` subfolder. Preserve reproducible preparation scripts
and analysis plans with the case. Downloaded models, adopted AI/manual chemistry
CSVs and immutable evidence supporting adopted records remain in `data/`; they
are not disposable cache. Do not move the only copy of a prior final delivery or
its scientific evidence into cache. A brief same-filesystem temporary file for
atomic final publication is allowed and must be cleaned up.

Users can remove `.cache/ai/` after its runs have stopped. This loses temporary
progress and downloaded research pages, not formal results or adopted evidence.
Do not automatically delete active runs, clear shared scientific tables, relocate
old case files or change the computation project's own caches as a side effect.
For standalone use without a project, use `.cache/ai/` under the chosen work folder.

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
Pass the shared AI work folder and explicitly request a review-only handoff:
this orchestrator owns the subsequent step-3 run. Review returns report_files as
a per-model list: each entry has input_file, markdown and a cached process json.
Use each model's own report and actual completion status; do not count input
screening as scientific review or automatically propagate an incomplete review. Standalone review defaults to running step 3 itself.
Do not duplicate its internal pipeline or select only successful mapper outputs.
Mapper failures still require a scientific attempt under that skill's rules.
No targets is a reported no-op, not a fabricated AI copy. Report unresolved,
partial or blocked review and identify available valid deliverables.

Inspect the installed step3 interface before running. The current project runs
only fixed-flux, size-1 EMU-inspired carbon-source balances. There is no
ANALYSIS_METHODS selector, enumeration, round setting, --rounds or separate step4.
Do not restore removed methods to satisfy an old workflow instruction. If a user
selects a historical checkout, identify its different interface explicitly
rather than assuming this current contract or upgrading it silently.

Step3 reads available ordinary and _AIGenerated mappings independently, producing
<model>_3_carbon_flow.xlsx and <model>_3_carbon_flow_AIGenerated.xlsx. Either version
may exist alone. New workbooks contain only EMU_Metabolites and
EMU_BoundaryInputs. There is no new Summary, Carbon_Flow or summary collector.
Historical combined workbooks remain historical; do not delete them or treat
their tables as current-run results. Step3 calls no mappers and no LLM.

Inspect and record EMU_ALLOW_INORGANIC_CARBON_RECYCLING (current default True),
EMU_TOLERANCE (1e-8), MAX_CARBON_PROCESSES, CARBON_MEMORY_GB,
MIN_AVAILABLE_MEMORY_GB and MAX_EXCEL_SIZE_MB. Inorganic-carbon recycling is a
modeling assumption, not evidence that CO2 escapes or is biologically negligible.
Use the project's shared scheduler; do not wrap it in another model-worker pool.
It retains mapper parallelism, independent model/version workers, live memory
checks, working-disk reserves and atomic complete-workbook saves. The current
50 decimal MB limit rejects an oversized export without replacing prior output;
resource/size failures return nonzero. No unfinished solve is a complete result.

## Verify and deliver

Check exit codes, hashes and current reports; old files alone are not success.
Distinguish success, no-op, partial review, errors, convergence and safety stops.
Use the current carbon_flow_run.json and exit code to identify the mapping
version/hash and saved output, not an old filename alone. emu_solved confirms the
balance was calculated; emu_completed=true, output_file and stop_reason=emu_solved
identify successfully delivered tables. Inspect the emu numerical checks,
assumptions and unresolved positions. Memory, disk, size or input failures can
leave historical outputs intact and can occur before a new report exists.
Do not retry an unchanged failure repeatedly or restart all mapping because a
later review/analysis failed. Remap affected ready reactions only through the
review skill's structure-completion handoff.

Write a short English `Ai_reports/workflow_report.md` following
[reporting](references/reporting.md), linking detailed stage reports and Excel
files rather than adding decorative sheets or another statistics implementation.
Present each model/condition separately in conversation, with its own changes,
completion status and final report link. Do not substitute batch-wide totals.
Report unfinished work and interface discrepancies explicitly. Report source
and Unknown fractions, mapping version, tolerance, recycling assumption and
numerical validation from this run. They are model-dependent mean carbon origins,
not full isotopomer distributions, experimental measurements or physical time.
Do not redistribute Unknown, infer a known source from a missing map, compare
these means as former enumeration coverage, or fit curves automatically.

The skill repository is https://github.com/LGJee/carbon-tracing-workflow-skill.
Ordinary deployment/analysis does not authorize publishing the project, changing
dependent skills or uploading private inputs to prediction services.
